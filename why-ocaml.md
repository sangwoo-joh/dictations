# Why OCaml

 What is a type system? What are types? When you think about
 programming, there are all these objects floating around in your
 program, values that you're dealing with. You can categorize these
 into classes. And in some senses, you can call those types. Different
 types of values. There's another thing in your program, which is a
 little bit more abstract, which is the idea of variable. What's the
 variable? Variable is a "name", that points to some values. Right?
 It's not, a variable isn't a concrete thing necessarily that points
 to a concrete spot in memory, in a unique way all time, it's just a
 name in your program, a logical name for things. Which is similar in
 many ways to like, a name in an equation. Right? You know, you name
 your variables in equations, you name your variables in your program,
 those things are many ways very similar to each other. So, in a
 dynamically typed language, values rather have types, because they're
 different kinds of things your language deals with, but variables
 don't. In a statically typed langauge, the variables themselves also
 have types, which is to say, at the time your program is written, the
 compiler can do a kind of static analysis, get look at your code, and
 "Oh, this variable always refers to things that have a certain type."
 So why is that important? There're really two different reasons
 important. One reason it is important is because of speed. And the
 reason it matters for speed is if you know in advance what are the
 kinds of the things there being manipulated, then you can write
 extremely efficient code for doing that. So, in a compiled language,
 if you add together two integers, what that turns into it runtime, is
 effectively a single machine instruction that says, "add the content
 of this register to the content of that register and put it in
 another register". A single instruction. If go to a dynamically typed
 language like Ruby, it's ae sort of longer conversation that starts
 like "Oh, you wanna add things together. What are the things? Maybe
 they're string then I should concatenate them. Maybe they're integers
 then I should add them together. Or maybe they're floating point
 numbers then I need different routine for adding them together. Or
 maybe ...??? " There's all sort of complicated things that can happen
 in a dynamically typed language. And it's in that gap of figuring out
 what you need to do, that's where the factor of hundred tends to come
 in.

 There's also correctness. So types were really born for
 performance. If you go back to the 50s where people were first
 inventing the programming languages, there were really two major
 languages: Lisp and Fortran. There's few other things that invented
 back then, but like, the only things that heading steering power were
 those two languages. Lisp was functional dynamic corner, and Fortran
 was static imperative corner. And I think the key reason why Fortran
 was static, was because they needed to be really fast, because they
 were running big jobs of lots of matrix operations for computing
 various physix simulations. And so the types were necessary for
 that. But types are also useful for making your code more correct,
 for helping you nail down properties of your program more reliably.

 What is it mean when you say "functional versus imperative"? There
 are kind of two things come up in this distinction. One part of being
 a functional language is having good support for programming with
 functions. As a kind of lightweight simple abstraction. Functions are
 things that are easy and lightweight to create, and you can deal with
 them as if they were data. You can take a function, and pass it as an
 argument to another function, and have that return another function,
 and take functions that are stored in a table of functions, ... You
 can very easy way kind of fling functions around. That's one thing
 that characterizes the class.

 The other thing is that functional languages give you good support
 for programming in a what's called a "pure" style of programming. I
 think the term "purity" itself, sort of little tendentious, but
 "pure" means something with less inheriant values in it. "Pure" means
 that programming that doesn't involve mutation, that doens't involve
 changing the values of things that your program evloves. And I think
 to someone who hasn't programmed in a functional environment, that
 seems kind of weird. How do you compute something? You always compute
 things by, you have some data, and you compute something, and you
 modify what you had before, and you do that over and over loop, until
 you get the right value and then you're done. That's sort of the
 ordinary way of "compute". And it turns out that it's not the only
 way to express a computation.

 There's a thing that often confuses people about functional
 languages, which is, in functional languages, you have these things
 called "variable", but they don't exactly "vary", in the way that you
 might expect them to. You don't modify the ??? , so why do you call
 them variables? And the reason is, you call them variables for the
 same reason you call variables in equations' variables. Not that, it
 varies over the course of executing the equations, it's because the
 equation can be applied to many different inputs. There're some part
 of the equation which is like static, and some part of it that you
 can think of it as varying over the different possible applications.

 **Mutation is more than just a way of getting a compuation done.** In
 general, when you have mutable state in your program, that mutable
 state, that changable state, is more than just a way of computing
 things, it's a **communication channel**. It is a way for different
 parts of your program to talk to each other. And, the reason you want
 to avoid them when you can, is that having your program that is
 cleanly separable into different pieces that don't talk to each other
 is really valuable for having a program that's easy to reason about
 as you compose it together. It's nice to be able to have different
 pars of your program that maybe running, kind of, together it's part
 of one massive system that you're building, and to be able to think
 about the program that you build by assembling these together as a
 kind of simple composition of the pieces that you started with. The
 more mutation that you have in your program, the harder that it is to
 do, because that mutation gives different kind of back channels, for
 different parts of your program talk to each other. Whereas, in a
 functional setting, you're not mutating anything, and the only form
 of variation is like, the functions being called on different
 values. Then, the functions are really explicit about what their
 inputs are and what their outputs are, and it is easier to understand
 how these pieces connect to each other.

 It's worth of saying that, there's nothing wrong with the side
 effects. In fact, all interesting programs have side effects. You
 write your program because you wanna have some effects on the world,
 otherwise it's kind of boring thing to do. In the end, almost all
 real world programming is about having an effect on the world. So you
 should judge your functional programming language, and any
 programming language, not so much on how much it lets you program
 without effects, but how much it helps you program with effects. The
 reason why it's good to have the ability in the language to program
 in this way without effects is because that part of the program, the
 part that doesn't have effects, is simpler to reason about and
 simpler to think about, and maximizing the amount of your program
 that's easier to reason about is good. But it's not you're trying to
 ban effects from your program. That's sort of at the heart of what
 you wanna do.
