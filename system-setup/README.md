# CSCI 3411 - Operating Systems - System Setup

In past years, this course has adopted a relatively [laissez-faire](https://www.merriam-webster.com/dictionary/laissez-faire) approach to individual student setups. We provided rough requirements, a few suggestions, and some particularly virtuous students volunteered to produce resources and help their peers get systems online.

In light of the COVID-19 pandemic and the decision to conduct this course remotely, the instructional team has changed our stance around this in order to more effectively support a standardized student toolset.

This offers several key advantages:

-   All students and members of the instructional team run roughly analogous setups and thus are better equipped to help each other out.
-   Standardization of tools better positions students to pair program and remotely collaborate using modern tools.

What's required:

-   Running Ubuntu 18 LTS operating system (in a VM is fine). The Mac and Linux setup instructions below outline which steps can be skipped for this configuration
    All homeworks will be _graded_ in this environment, so if you do your homeworks in any other environment, it is likely that you'll get compiler errors.
-   Running VS Code with remote collaboration extensions for in-lab and in-class group exercises.

You can still use your own editor and setup for the homeworks, but during class, these will be used.
If you don't have a means to set these up, we've provided a _default_ stack that essentially includes:

-   a lightweight virtualization solution (WSL2 for Windows, Multipass for all others)
-   a "headless" Ubuntu 18 LTS guest
-   Visual Studio Code
-   An extension to use your Ubuntu guest as a dedicated development backend
-   An extension to provide for a "Google Docs" like remote editing experience

    _Note: students natively running Ubuntu 18 LTS as their host operating system do not require virtualization_

## Setup Instructions

-   [Windows](windows.md)
-   [Mac and Linux](multipass.md)

## Feedback Welcome

Given that this is new terrain for the instructional team, as you setup and use this environment, please keep notes on anywhere you struggled and consider making pull requests with fixes or improvements.
