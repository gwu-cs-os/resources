# Debugging

Debugging is a skill.
Just like any other skill, practicing doing it well will make you much better at it.
Most students are so rushed by deadlines (often due to procrastination) that they focus only on "making it work" rather than on refining their debugging skills to become better computer scientists.
Finding a fix to any given issue is important, but you _must_ focus on developing your long-term skills, or else you'll find your "ceiling" of capability much sooner than you'd like.

To deliberately and continuously improve your debugging skills, I implore you to focus on the following:

-   listing and check your assumptions,
-   applying the scientific method by treating your assumptions like hypothesis,
-   planning your debugging approach, and feeding this back into the development of your code, and
-   continuously rewriting your code to make it more debuggable and understandable.

They are each briefly discussed below.

#### List and Check Your Assumptions

When something goes wrong with your code, it is because your mental model of how everything should fit together does not match the instructions you're providing the computer.
Your mental model is build of many assumptions about what your code is doing at any point in time.
One of the most important learned skills is recognizing _when_ you're making an assumption, and identifying what the assumptions are.
Examples of these assumptions include:

-   The list head is properly initialized before calling the add function.
-   Error checking code should properly catch an aberrant input.
-   When the thread is initialized, it should use the specific part of the stack previously that was allocated.
-   The addresses used in the stack are within the bounds of the allocated stack.
-   When I use the `sbrk` system call in my code, it expand the heap and allows me to use more memory.
-   When a system call is invoked in `xv6`, the first code in the kernel properly accesses the system call's arguments.
-   ...

Most your debugging time should be going to coming up with creative ways to check your assumptions.
When something goes wrong, (at least) one of your assumptions is wrong.
Your job is to figure out which one(s).
This changes the emphasis of debugging from "why so wrong!?!?" to "which specific part of my understanding of this is incorrect?".
This is how you change debugging from what feels like a _random_ process, to something that is methodical, deliberate, and, most importantly, _productive_.

It is very common for people to get _stuck_ with no obvious path forward.
The program is simply not doing what they think it should, and it _makes no sense_!
If you're at this point, then you've already made too many assumptions (at least one of them being wrong), and have built your entire mental model upon them.
The solution is to go back, and start testing not your code, but the assumptions made throughout the lines of your code.
Where is your understanding of your code going wrong?

#### Scientific Method

The scientific method is based around formulation hypothesis about how some phenomenon will behave, designing tests for checking the hypothesis, running the experiments/tests, and either confirming the hypothesis, or starting the entire process again.
An important aspect of this process is that it is focused on knowledge creation.
Deriving a hypothesis takes a deliberate process that focuses you to apply your brain, and think about the workings of the entire system.
If it is confirmed, you often know not only that the hypothesis is true, but also _why_.
If the hypothesis is wrong, you learn that there is a bug in its formulation, and you might go back to more foundational tests to figure out how and why it is wrong.
This enables you to understand the system better, and made a better hypothesis next time around.

You should treat your assumptions about your own code with the same care.
First, acknowledge that they are assumptions -- never just assume that a part of your code works without testing -- and then treat the assumptions as hypothesis about the code that need to be tested.
Most importantly treat your assumptions as something that you _refine_ through intentional testing.
It is through this _process_ that you learn about how your code works.

#### Planning your Approach

When you sit down with the goal of solving some programming task, it is natural to try and jump in and start developing.
This is a fine approach if the problem that you're trying to solve is below some complexity threshold.
If it is a very comfortable task for you, then just "diving in" can be a perfectly viable approach.
As you get more and more skilled as a programmer, this threshold will increase.
However, as you get more and more skilled, you're responsibilities will likely increase faster than this threshold.
Thus, it is necessary to learn to be productive on complex projects that you cannot easily just "sit down and implement".

It is necessary to do work that is beyond this threshold for your own personal development.
Much of the work in this class is valuable because it is, by design, too complex to implement productively without a plan.
This forces you to practice the act of planning out your approach.
I help with this in the homeworks by providing a set of "levels" that need to be completed.
You must complete the levels one at a time as they provide a path to incrementally add complexity only after you've learned about the problem from the previous levels.

Your development plan is simply a sequence of implementation goals that you implement, and test one at a time.
You don't need to have a complete plan mapping from the start of the implementation to the end, but you do want the first _N_ steps so that you have a plan.
This allows you to implement you code in a way that has a little bit more of a long-term design behind it.

#### Rewriting Code

When we write code, we often don't understand the problem domain well enough to provide an ideal implementation.
The effect of this is that our code is far too complex.
As we learn about different aspects of the problem domain -- "ohhh, I have to worry about `i > N` as well", "yikes, what if the head is `NULL`?", etc.. -- we add more and more conditions and loops.
Keep this in mind: at any point in your code look at the level of indentation; that is a good proxy for the number of things you need to keep in your head to understand a given line of code.
Each condition and loop encodes variables and changes that you need to remember to understand the code in their bodies.
Given that human short-term memory holds only 7$\pm$2, code simply becomes too complex to keep in your head.

It is _necessary_ to revisit and rewrite your code when it becomes to complex.
Debugging code that is too difficult to understand and too challenging to even formulate assumptions about will take orders of magnitude more time to debug than to rewrite your code with a simpler structure and debug that.
I _very frequently_ find that I cannot make sense about how to modify my code to achieve some goal, and the only anecdote that I know is rewriting the code to constrain and abstract away complexity.
You'd be amazing how much a few well-named functions containing well-abstracted code can make your code easier to understand.
Additionally, the act of trying to simplify your code makes your code easier to debug and forces you to understand the fundamental problems, which also makes debugging (and assumption formulation) easier.
In short, real-world programming involves a _lot_ of code rewriting.
For much more information about how to write C code with an eye toward constraining the complexity, see the [composite style guide](https://github.com/gparmer/composite/blob/ppos/doc/style_guide/composite_coding_style.pdf).

#### More Resources

-   Devon O'Dell's [Debugging Mindset](https://queue.acm.org/detail.cfm?id=3068754)
-   Bryan Cantrill's [Debugging Trilogy](http://dtrace.org/blogs/bmc/2018/02/03/talks/#DebuggingTrilogy) (more amusing than informative)
-   Do you have any other suggestions?
    Send me an email!
