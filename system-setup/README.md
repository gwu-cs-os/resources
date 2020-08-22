# CSCI 3411 - Operating Systems - System Setup

In past years, this course has adopted a relatively [laissez-faire](https://www.merriam-webster.com/dictionary/laissez-faire) approach to individual student setups. We provided rough requirements, a few suggestions, and some particularly virtuous students volunteered to produce resources and help their peers get systems online.

In light of the COVID-19 pandemic and the decision to conduct this course remotely, the instructional team has changed our stance around this in order to more effectively support a standardized student toolset.

This offers several key advantages:

-   All students and members of the instructional team run roughly analogous setups and thus are better equipped to help each other out.
-   Standardization of tools better positions students to pair program and remotely collaborate using modern tools.

The stack that we've chosen essentially includes:

-   a lightweight virtualization solution (WSL2 for Windows, Multipass for all others)
-   a "headless" Ubuntu 18 LTS guest
-   Visual Studio Code
-   An extension to use your Ubuntu guest as a dedicated development backend
-   An extension to provide for a "Google Docs" like remote editing experience

    _Note: students natively running Ubuntu 18 LTS as their host operating system do not require virtualization_

## Setup Instructions

-   [Mac](mac.md)
-   [Windows](windows.md)
-   [Linux](linux.md)

## Feedback Welcome

Given that this is new terrain for the instructional team, as you setup and use this environment, please keep notes on anywhere you struggled and consider making pull requests with fixes or improvements.
