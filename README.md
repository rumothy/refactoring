# Refactoring

---

## Comments on the Starting Program

What are your thoughts on the design of this program? The first thing I'd say is that its's tolerable as it is--a program so short doesn't require any dep structure to be comprehensible. But remember my earlier point that I have to keep examples small. Imagin this program on a larger scale--perhaps hundreds of lines long. At that size, a single inline function is hard to understand.

Given that the program works, isn't any statement about its structure merely an aesthetic judgement, a dislike of "ugly" code? After all, the compiler doesn't care whether the code is ugly or clean. But when I change the system, there is a human involved, and humans do care. A poorly designed system is hard to change--because it is difficult to figure out what to change and how these changes will interact with the existing code to get the behavior I want. And if it is hard to figure out what to change, there is a good chance that I will make mistakes and introduce bugs.

Thus, if I'm faced with modifying a program with hundreds of lines of code, I'd rather it be structured into a set of functions and other program elements that allow me to understand more easily what the program is doing. If the program lacks structure, it's usually easier for me to add structure to the program first a, and then make the change I need.

In this case, I have a couple of changes that the users would like to make. First, they want a statement printed in HTML. Consider what impact this change would have. I'm faced with adding conditional statement around every statement that adds a string to the result. That will add a host of complexity to the function. Faced with that, most people prefert to copy the menthod and change it to emit HTML. Making a copy may not seem too onerous a task, but it sets up all sorts of problems for the future. Any changes to the charging logic would force me to update both methods--and to ensure they are updated consistently. If I'm writing a program that will never change again, this kind of copy-and-paste is fine. But if it's a long-lived program then duplication is a menace.

This brings me to a second change. The players are looking to perfom more kinds of plays: they hope to add history, pastoral, pastoral-comical, historical-pastoral, tragical-historical, tragical-comical-historical-pastoral, scene individable, and pem unlimited to their repertoire. They havent' exactly decided yt what they want to do and when. This change will affect both the way their plays are charged for and the way volume credits are calculated. As an experienced devloper I can be sure that whatever scheme they come up with, they will change it again witthin six months. After all, when feature requests come, they come not as single spiees but in battalions.

Again, that statement method is where the changes need to be made to deal with changes in classification and charging rules. But if I copy statement to htmlStatement, I'd need to ensure that any changes are consistent. Furthermore, as the rules gron in complexity, it's going to be harder to figure out where to make the changes and harder to do them without making a mistake.

Let me stress that it's these changes that drive the need to perform refactoring. If the code works and doesn't ever need to change, it's perfectly fine to leave it alone. It would be nice to improve it, but unless someone nees to understand it, it tisn't causeing any real harm. Yet as soon as someone does need to understand how that code works, and struggles to follow it, then you have to do something about it.

## The First Step in Refactoring

Whenever I do refactoring, the first step is always the same. I need to ensure I have a solid set of tests for that section of code. The tests are essential because even though I will follow refactorings structured to avoid most of the opportunities for introducing bugs, I'm still human and still make mistakes. The larger a program, the more likely it is that my changes will cause something to break inadvertently--in the digital age, frailty's name is software.

An important part of the tests is the way they report their results. They either go green, meaning that all the strings are identical to the reference strings, or red, showing a list of failures--the lines that turned out differently. The tests are thus self-checking. It is vital to make tests self-checking. If I don't, I'd end up spending time hand-checking values from the test against values on the desk pad, and that would slow me down. Modern testing frameworks provide all the features needed to write and run self-checking tests.

As I do the refactoring, Ill lean on the tests. I think of them as a bug detector to protect me against my own mistakes. By writing what I want twice, in the code and in the test, I have to mke the mistake consistently in both places to fool the detector. By double-checking my work, I reduce the chance of doing something wrong. Although it takes time to build the tests, I end up saving that time, with considerable interest, by spending less time debugging. This is such an important part of refactoring that I devote a full chapter to it (Building Tests (85)).

---

## Decomposing the _statement_ function
