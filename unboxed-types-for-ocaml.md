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
 back in about two years time.
