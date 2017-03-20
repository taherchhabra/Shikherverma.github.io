---
published: false
---
I was Reading the book C# in Depth by Jon Skeet wherein there was a section about Myths developers have about c#. One of the myth was about where the reference types and value types are stored in memory. I used to believe that reference types are stored in Heap and Value types are stored in stack. But the truth is as Below

I am directly copy pasting the excerpt from the book :)

" The first part is correct—an instance of a reference type is always created on the heap. It’s the second part that causes problems. As I’ve already noted, a variable’s value lives wherever it’s declared—so if you have a class with an instance variable of type int, that variable’s value for any given object will always be where the rest of the data for the object is—on the heap. Only local variables (variables declared within methods) and method parameters live on the stack. In C# 2 and later, even some local variables don’t really live on the stack"
