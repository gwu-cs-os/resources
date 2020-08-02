# Testing

Untested code must be assumed wrong, simply put.
You can never make the assumption that any part of your code works that you haven't tested, no matter how simple.
"It should work" is something that the instructional staff will never believe, and should be an indication of hard debugging ahead if you find yourself uttering it.
"It should work" is a huge assumption with a builtin recognition that you *aren't going to test the assumption*.
Put another way, it is a recognition that you're about to waste a lot of your own time.
So I want to make it clear that you *must* test all parts of your own code.
If your grade is lower because a branch doesn't quite do what it is supposed to, "but it is just a small error" not only is completely incorrect, but demonstrates a complete lack of understanding of the fundamental importance and purpose of testing.

The job of a library, an operating system, or a piece of utility code is to make the programmer able to perform some operation, or to make the operation easier.
In such situations, you have to assume that programmers don't know what they are doing, and that, to your greatest ability, you'll catch their errors and return a useful indication of where they went wrong.
In the case of an OS, you have to check that all parameters/inputs are valid as if they are not, they might cause a bug that could crash the entire system, or allow the user to take over the system.
This is accomplished in a similar way in every language, on every platform: write the code to explicitly check the validity of arguments, write the code to check for edge cases, and return the proper result.
In the case (as in this class) where you're given the specification for each operation, you simply need to ensure that you encode all of these erroneous inputs into conditions, and return the proper value.

Next, since code that isn't tested doesn't work, you have to write tests to make sure that all inputs result in the correct output.
Properly testing your code is simple: codify your assumptions about how your implementation behaves into code.
This absolutely means that you'll spend time writing your testing code to generate all the edge cases, but this is absolutely necessary.

Most submissions that lose points, lose them because:

1. Conditions aren't there for specified inputs.
2. Conditions are there, but aren't properly tested, thus produce incorrect behavior.
3. Implementations are incomplete, or have known bugs.

There is almost no reason to lose points for 1. and 2.
Testing is an essential part of being a developer.
Thus, the instructional staff will not be lenient if you don't do proper testing.
