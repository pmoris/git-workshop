.. Workshop: git collaboration documentation master file, created by
   sphinx-quickstart on Tue Aug 16 16:40:34 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Workshop: git collaboration
=======================================================

.. toctree::
   :numbered:
   :maxdepth: 2
   :caption: Contents:

   chapters/1-brief-summary.md
   chapters/2-revising-the-basics.md
   chapters/3-rewriting-history.md
   chapters/4-remotes.md
   chapters/5-branching.md
   chapters/6-collaboration.md
   chapters/7-what-is-next.md

.. Indices and tables
.. ==================

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`


Welcome to this workshop on collaboration in git! It will teach you the basics of how you can use git to work together with other people. We will cover pushing and pulling changes from remotes, branching strategies and pull requests to get your code reviewed before merging it with the work of others. But first, we will revise some of the basic aspects of git.

.. figure:: ./img/mona-lisa-final.jpg
   :align: center
   :width: 600px

   *Does this sounds familiar?*

.. note::
   The example solutions to all exercises is provided in the form of bash CLI commands, but of course you could perform most (*not all*) of these actions inside RStudio's or VSCode's git plugins, or even in a fully-fledged git GUI like Fork or GitKraken.

   Personally, learning how to use the git CLI was very beneficial to understanding it - the abstraction of certain tasks in some GUIs can sometimes be a hindrance in this regard. Moreover, unlike a GUI, it is available everywhere, gives you access to all features and will always behave the same. And it makes writing a tutorial like this feasible without requiring dozens of screenshots ;)

   That being said, I do find a GUI to be extremely useful when diff'ing files, interactively staging files or looking back in the commit history, so it certainly has its place.

   .. figure:: ./img/git-gui.jpg

.. attention::
   Before starting this workshop, ensure that you have a fully functional version of git available on your workstation. For Windows machines, this ideally is git installed inside an (Ubuntu) WSL, alongside a proper shell like Windows Terminal, but the "Git for Windows" and "Git Bash" CMD applications can also work. In addition, a fully featured git GUI like Fork, Git Kraken or a similar application, or the git plugin of your favorite IDE like RStudio or VSCode (I highly recommend the GitLens extension) can be useful if you prefer to not do all of your work on the command line, especially for comparing files.
