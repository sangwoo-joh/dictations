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
 in a dynamically typed language.
