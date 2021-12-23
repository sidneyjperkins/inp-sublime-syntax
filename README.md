# inp-sublime-syntax
Author: Sidney Perkins | sidney.j.perkins@gmail.com
Date:   22 December 2021

## OBJECTIVE:
The objective of this project is to provide syntax files for use in Sublime for enhancing the legibility of .inp files used for svFSI simulations.

## CONTACT:
If you have any problems, questions, ideas or suggestions, please contact me above at my email address.

## WEBSITES:
(1) To install the latest version of svFSI, please visit: https://github.com/SimVascular/svFSI

(2) To find helpful demos of finite element modeling using the svFSI solver, please visit: https://github.com/SimVascular/svFSI-Tests

## GIT:
To download the latest source off the GIT server, do this:

    cd /Users/Usrname/Library/Application Support/Sublime Text 3/Packages/User/

    git clone https://github.com/sidneyjperkins/inp-sublime-syntax
    
    mv * ../
    
You'll now have the syntax file from the git repository in your Sublime Text 3 Application Support folder. Restart Sublime and it should be ready to use with .inp files.

## NO LIKEY GIT:
If you're like me and are lazy, you may just want to copy pasta the files into the appropriate folder without futzing around in terminal. Totally fine! No judgement. Find the directory that sublime uses for syntax files. To do this:
    
    (1) Open Sublime Text
        
    (2) Select Sublime Text > Preferences > Browse Packages ...
        
    (3) In Packages/User, copy the files my-keyword-hightlighter.sublime-syntax and my-keyword-highlighter.sublime-settings into this directory
        
    (4) Close Sublime. Re-open Sublime.
        
Test the code out by opening a .inp file. The very first time you use the style, you may need to select:
        
    View > Syntax > my-keyword-highlighter

## WHAT EXACTLY DOES THE SYNTAX FILE DO?
Good question. Though .inp files  are not written in a programming language, they require specific formatting for svFSI to properly run. By cleverly examining the source code and example uses of svFSI, you can get a good sense for the appropriate formatting. However, it's still relatively easy to make typographical errors in editing .inp files. .inp file specific syntax definitions make files more legible and increase the liklihood of identifying typos before pushing your simulations to the server, hopefully saving you some time and stress.
    
There are seven variable types defined by the syntax file: (1) keywords, (2) numbers, (3) files, (4) commands, (5) recognized arguments,(6) otherwise undefined text, and (7) comments.
  
More below:

(1) KEYWORDS are encoded (unsurprisingly) in YAML using the scope "keyword." They have been defined by examining svFSI's READFILES.f script and examining definitions of pointers (ctrl-f on "lPtr"). Generally speaking, these phrases correspond to some pre-defined variable that the solver must recognize and therefore are followed by a colon (":") in the inp file. Therefore, we search the inp file for occurrences of these keywords followed by colons. If the colon is not present followed by a single space, the text is no longer formatted as a keyword and will return to text.plain.

(2) NUMBERS are any number formatted in decimal, integer, exponential, dice/index notation, or bool (t/f). In YAML, they are defined using the scope "constant.numeric". If the colon is not present, the text is no longer formatted as a keyword and will return to text.plain.

(3) FILES are uniquely formatted strings that contain file extensions frequently used by the svFSI solver. These include .vtu, .txt, .dat, and .vtp files. FILES are identified in YAML using the scope "string" and occur only following a colon. If the colon is not present, the text will no longer be formatted as a keyword and will instead return to text.plain.

(4) COMMANDS are the closest thing in the inp file to a function call. These generally include the word "Add" and are used to add meshes, faces, equations, and boundary conditions to the simulation. The are also followed by colons. In YAML, COMMANDS are encoded using the scope "variable.function". If the colon is not present, the text will no longer be formatted as a variable.function and will instead return to text.plain.

(5) RECOGNIZED ARGUMENTS are reserved variables that carry meaning to the svFSI solver and should therefore be avoided in defining variables. Examples include equation types "CEP" for cardiac electrophysiology or "fluid", boundary condition types such as "Dir" for dirichlet, "steady" versus "unsteady" flow boundary conditions, "parabolic" flow profiles, "RK" ODE solver, "NS" linear solver type, etc etc. You can quickly appreciate there is a tremendous variety of terms that can be given to the multiphysics solver. The list of reserved words used as RECOGNIZED ARGUMENTS is very, very long. In YAML, RECOGNIZED ARGUMENTS are encoded using the scope "variable.parameter". A RECOGNIZED ARGUMENT that is modified such that it no longer is in the list of RECOGNIZED ARGUMENTS will be reformatted as OTHERWISE UNDEFINED TEXT (see below), so long as it continues to be preceded by a colon. As before, if the colon is not present, the text will no longer be formatted as a variable.parameter and will instead return to text.plain.

(6) OTHERWISE UNDEFINED TEXT is reserved for text that neither contains the aforementioned file extensions nor is in the list of reserved variables but follows a colon. This is denoted in YAML using the scope: "entity.other.attribute-name"

(7) COMMENTS are denoted by the hash ("#") and change the formatting for all characters in a given line that follow the hash symbol. COMMENTS are denoted in YAML using "scope: comment"

## A BRIEF NOTE ON ELEGANCE (OR LACK THEREOF):
In general, I want the syntax defintions to over-call formatting issues (ie the syntax definitions are strict). To determine formatting rules, I manually combed through variables that appeared in or around instances of "lPtr" and "CASE" in the readfiles.f script. Due to my illiteracy in reading Fortran, I took the precaution of assuming that all string variables fed to the svFSI source code were case sensitive. There are obvious exceptions in the online example repository (https://github.com/SimVascular/svFSI-Tests), which have been manually added to the syntax file.

## ENJOY!
I hope this saves you some time and makes your experience more enjoyable.
