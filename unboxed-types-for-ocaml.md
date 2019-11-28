# Unboxed Types for OCaml

 Know that you're all here, and stuck in the room with me, I can
 finally reveal the actual title of this talk, which is "Making OCaml
 less like Lisp and more like C++".

 To explain why I'm doing such an awful, awful thing, I'm going to
 talk about performance for awhile. So, this is entirely motivated by
 performance, the only reason to want to unbox things is to make
 things go faster. But to explain just how much faster this sort of
 stuff can make things go, I need to talk about some actual concrete
 numbers. And if you haven't spent spent a long time doing a lot of
 low-level performance work, then the sort of visceral difference
 between nano-seconds and micro-seconds might not be too aparent to
 you, because they're both such a tiny, tiny units of measure. So, I'm
 going to borrow a good trick from Brandon Greg here, which is by
 explaining all of these timing numbers by multiplying them by a
 billion.

 So, addition, now it takes about half a second. Subtraction also
 about half a second. Multiplication about two second. Division takes
 about half a minute. And, these are all like about, reasonable
 processor multiplied by a billion. Some other(of their?) operations,
 a system call, the very best case that you can have, for like asking
 the OS kernel what your user ID, is a couple of minutes. And
 accessing something on an SSD is going to take about an
 hour. Starting an program in C, this is one thing like been choosed,
 telling the operating system to start a new process which does
 nothing at its exit, takes about two days. If the program been
 choosed actually is an under a program written in OCaml rather than
 C, it takes about a week, just to kick the garbage collector to move
 on. If for some reason, your program written in Python and does
 nothing, then it's about two months. If you're writing a video game,
 and you have to update the world and render it 60 frames a second,
 then you have six months of computations, to get the next frame
 out. Of course, there are things that take even longer, if you're
 chosen to do a ping across the Atlantic, that'll expect that to come
 back in about two years time. So, the point trying to make here is,
 these numbers cost a lot more orders of magnitude than you might
 expect. We're not talking about like, one operation being 10% faster
 than another, we're talking about many many zeros in the difference
 between the two.

 So this is well if we start talking about memory. So, accessing
 something which is in the CPU's L1 cache takes a couple of
 seconds. These will vary a lot by the CPU architecture you're using
 or which model the processor and so on, but roughly, we're talking a
 couple of seconds for L1 and the next level of cache a few seconds
 longer, maybe 20 seconds half a minute of the last level of the
 cache. But if you don't manage to hit the cache at all, then it's
 going to be a couple of minutes, in, again, I think multiplied by a
 billilon, before we can get things back from main memory. If you were
 really unlucky, then as well as having to go all the way up to main
 memory, you can get something called a TLB miss, which is the
 processor not only has to go all the way out to main memory, but also
 doesn't actually have the table that tells us where in maybe it's
 supposed to go, so it has to first do some accesses to find that and
 then do the accesses to actually access the data you wanted. So that
 can take five or ten minutes on a good day. This is, again, a lot
 longer than two seconds, we're not talking about like there being a
 10% difference depending on whether you hit the cache or not, we're
 talking about many many times faster or slower. Hapily, this is about
 as bad as things get these days, if I were given this talk a few
 years ago, back in the days, when like swapping something out to a
 rotating disk might have been a reasonable thing that a computer
 could do sometimes, and that meant that when you access to a variable
 in memory, if you were really unlucky, you would actually have to
 wait for this bit of spinning worst to rotation to the right spot,
 and that wouldn't come back for months. So the point I'm trying to
 make here is, each of these levels is tens hundreds of times smaller
 than the previous ones, and if you can make your data very very
 smaller or can fit in the upper levels, and if it does fit in the
 upper levels and access to it would be very very much faster than if
 you have to go down to any of these lower levels to satify any memory
 requests. So, things will be much faster if they're smaller.

 So, what decides how big your data is, in many ways, the compiler,
 the implementation of the programming language you're using, it has
 the job of taking all of the things that you wrote and laying them
 out somehow in memory. So, the biggest question has to answer is,
 well, how many bytes in what order should this value that you're
 representing take up? There are a couple of other ones such are
 important, CPUs of multiple different types of registers, so there's
 should this value if we were passing it to a function, would it
 better to pass it in the integer register or the floating pointer
 register? If it's an integer should go in one and if it's a floating
 point you go in the other, if you make the wrong decision you'll
 waste a lot of time, there's a penalty for moving stuff from one to
 the other. And in the context of OCaml, which is a garbage-collected
 language, we also have to worry about when we're representing a
 value, do we have to tell the garbage collector about this, if we're
 representing something that is, like, an array or a tuple that's on
 the garbage collector's heap, the garbage collector must know about
 all references to that, so that if it needs to mark it or collect it
 or move it, and those were to look. Whereas if it's just some raw
 bits of floating point numbers, say, then the garbage collector need
 not be told about it.

 So, most types, the best way of laying something out depends on the
 type, and most types have a pretty obvious good layout. If we're
 like, weave in a way of strings, that should be probably a pointer to
 some memory hodling all of the array, so we just have to pass over
 this small pointer or than copying all the array everytime. If we
 have an float, that should be represented as like 8 bytes for a
 double-precision floating-point number should pass for the register,
 no need to tell the garbage collector about it. If it's a 32-bit int,
 that only needs 4 bytes. There's a fairly obvious way of laying out
 each of these different types and they're all fairly straightforward
 how you would do this.

 So, anyone who's done any sort of performance work on OCaml, anyone
 who's trying to make an OCaml program run quickly, will know that
 these obvious good layouts are not at all how OCaml lays out in 32s
 or floating-point numbers, it does something much, much
 worse. Integers and floats all get their own separate little
 allocation of a block that's 16 or 24 bytes long, which just contains
 this extra value, and it only exists to contains this value, and the
 pointer to this block takes up just as much space as the value that
 you're trying to represent. So, why on earth does it do this? This is
 not a system that was designed by, like people who didn't know what
 they're doing, and still they did this. And, they did this because of
 a really tricky problem, which is actually what this talk is mostly
 about, how do we lay out an unknown type? If we know that was 32,
 there's an obvious way of doing that, we know it's a float, there's
 an obvious way of doing that, but if we just don't know about what
 the type is, what are we supposed to do to lay it out?

 So, there are two fairly obvious solutions. First of all, this is
 actually a more common problem than you might imagine. You regularly
 find yourself with a program which contains some unknown types. The
 simplest example is you writing a polymorphic function like
 `map`. `map` takes a list and the elements of the list are of any
 possible type, and so when we're type checking or compiling this
 function, we do not know what type the list elements are, because
 this function has to work for any possible type. But writing
 polymorphic functions is not like a thing that you like there are
 many of them, but most lots of functions are just written first
 order, even in cases where we're not writing polymorphic functions
 whenever we use a module that defines an abstract type, we still have
 the same problem. If `Location.t`, there's a module called `Location`
 which defines a type called `t`, then when we're compiling this, we
 don't know what the type is. This is still, we need to compile in the
 presence of an unknown type. We need to know how these values are
 supposed to laid out, even though we don't know what the type is.

 So, there are two standard solutions to this. Number one, we can lay
 out all of the types in exactly the same way, which is the old Lisp
 approach, and then, it doesn't really matter how we're going to lay
 types out, because it doesn't really matter that a type is unknown
 because we're all going to be laid out the same way anyway. Number
 two, is we can delay making any choices until we actually know what
 the type is,which is the C++ version that like when you write a
 template in C++, it doesn't actually generate any code to compile
 anything, it waits until the template is used as a particular
 type. Unfortunately, both of these are horrible in different
 ways. The horribleness of Lisp one is the inefficiency of the
 representations you end up with. You have a particular uniform
 representation, in OCaml that representation is there's machine words
 of 64 bits long and if the low bit is zero it's a pointer to
 something the garbage collector understands, if the low bit is one
 then it's just an integer. And for things which fit into this, which
 are basically pointers to garbage collector stuffs or integers, then
 this is a reasonably efficient, but anything that doesn't fit into
 this, like for instance, a 64 bit float which might actually end in
 zero will not be a pointer, then those have to be encoded via some
 more roundabout way, you end up having to ? what OCaml does is encode
 them as a pointer to a block that was invented it just so that it
 could hold this one thing.
