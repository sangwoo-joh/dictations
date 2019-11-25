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
