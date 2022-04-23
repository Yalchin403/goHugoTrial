---
title: "Vim Cheatsheet"
date: 2022-04-23T20:21:07+04:00
draft: false
cover: "blog-images/vim.png"
useRelativeCover: true
tags: ["Vim", "Cheatsheet"]
---



| **No** | **Command**      | **Meaning**                                                                                                                                      |
|--------|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 1      | :w               | Save                                                                                                                                             |
| 2      | :q               | Quit                                                                                                                                             |
| 3      | :q\!             | Quit without saving                                                                                                                              |
| 4      | :wq              | Quit with saving                                                                                                                                 |
| 5      | k                | Navigates up                                                                                                                                     |
| 6      | j                | Navigates down                                                                                                                                   |
| 7      | h                | Navigates left                                                                                                                                   |
| 8      | l                | Navigates right                                                                                                                                  |
| 9      | i                | Enter insert mode                                                                                                                                |
| 10     | esc or ctrl \+\[ | Enter normal mode                                                                                                                                |
| 11     | gg               | Navigates to the top                                                                                                                             |
| 12     | G                | Navigates to bottom                                                                                                                              |
| 13     | H                | High: takes you to the top of your code                                                                                                          |
| 14     | M                | Middle: takes you to the middle of your code                                                                                                     |
| 15     | L                | Low: takes you to the bottom of your code                                                                                                        |
| 16     | nG               | Navigates to n\-th line \(example \- 3G navigates to 3rd line\)                                                                                  |
| 17     | \{               | Navigates one code block up                                                                                                                      |
| 18     | \}               | Navigates one code block down                                                                                                                    |
| 19     | nk               | N lines up                                                                                                                                       |
| 20     | nj               | N lines down                                                                                                                                     |
| 21     | nh               | N lines left                                                                                                                                     |
| 22     | nl               | n lines right                                                                                                                                    |
| 23     | u                | Undo                                                                                                                                             |
| 24     | ctrl \+ r        | Redo                                                                                                                                             |
| 25     | \. \(period\)    | Do it again                                                                                                                                      |
| 26     | yy               | Copy                                                                                                                                             |
| 27     | P                | Paste above                                                                                                                                      |
| 28     | p                | Paste below                                                                                                                                      |
| 29     | v                | Switches to visual mode                                                                                                                          |
| 30     | o                | Adds new line below and enters insert mode                                                                                                       |
| 31     | O                | Adds new line up and puts you inside insert mode                                                                                                 |
| 32     | w                | Navigate to next word                                                                                                                            |
| 33     | b                | Navigate to previous word                                                                                                                        |
| 34     | :n               | Takes you to the n\-th line                                                                                                                      |
| 35     | 0 \(zero\)       | Navigates to the beginning of line                                                                                                               |
| 36     | ^ or 0w          | Navigates to the beginning of the first word in the line                                                                                         |
| 37     | W                | Navigates to next word \(ignores punctuation\)                                                                                                   |
| 38     | B                | Navigates to previous word \(ignores punctuation\)                                                                                               |
| 39     | t                | Navigates to the specified character and put the cursor one before the specified character \(example: t\!\)                                      |
| 40     | f                | to the specified character and put the cursor on the specified character \(example: f\!\)                                                        |
| 41     | %                | Navigates to the next same character                                                                                                             |
| 42     | cw               | Changes next word                                                                                                                                |
| 43     | dw               | Deletes next word                                                                                                                                |
| 44     | D                | Deletes rest of the line \(after coursor\)                                                                                                       |
| 45     | C                | Deletes rest of the line \(after coursor\) and enters insert mode                                                                                |
| 46     | ct               | Then press the letter that you want to change up to, will delete till that letter and enter insert mode                                          |
| 47     | dt               | Then press the letter that you want to change up to, will delete till that letter                                                                |
| 48     | \*               | Searches for current word                                                                                                                        |
| 49     | ;                | Goes to next instance                                                                                                                            |
| 50     | a                | Takes you to the next character and put you into insert mode                                                                                     |
| 51     | A                | Navigates to the end of the line and enters insert mode                                                                                          |
| 52     | I                | Takes you to at the beginning of the line and puts you into insert mode                                                                          |
| 53     | ~                | Swaps the letter \(upper to lower case or vice versa\)                                                                                           |
| 54     | /                | Searches particular word or character, press &quot;Enter&quot; to find\. \(Then you can use n to navigate next occurrence or N to the previous\) |
| 55     | :num             | Navigates to specified line\. Example: :55 will take you 55th line                                                                               |
