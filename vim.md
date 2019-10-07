# Table of Contents <!-- omit in toc -->
- [What is vim](#what-is-vim)
- [How do I Use vim](#how-do-i-use-vim)
- [Modes](#modes)
  - [Normal/Command [Esc]](#normalcommand-esc)
  - [Insert [i]](#insert-i)
  - [Visual [v]](#visual-v)
- [Commands](#commands)
  - [Saving and Quitting](#saving-and-quitting)
  - [Movement](#movement)
    - [By Characters](#by-characters)
    - [By Words](#by-words)
    - [Modifiers](#modifiers)
    - [Moving in Lines](#moving-in-lines)
    - [Moving to Lines](#moving-to-lines)
    - [Moving in Blocks](#moving-in-blocks)
  - [Search](#search)
  - [Inserting](#inserting)
  - [Deleting](#deleting)
  - [Deleting then Inserting](#deleting-then-inserting)
  - [Multiple Insert](#multiple-insert)
  - [Replacing](#replacing)
  - [Repeat Command](#repeat-command)
  - [Undo and Redo](#undo-and-redo)
  - [New Lines](#new-lines)
- [References](#references)

# What is vim
`vim` is a command-line text editor based on Bill Joy's original `vi` text editor.

# How do I Use vim
You'll need to be able to navigate the command line

# Modes
## Normal/Command [Esc]
**Normal mode** is the first mode your vim will be set as when you start it. Hitting `Esc` will revert you 
back to Normal mode from any other mode. Considered to be the *browsing* or *reading* mode, it provides you with more efficient ways to navigate and manipulate the file. 

## Insert [i]
**Insert mode** can be accessed by pressing the `i` key while in Normal mode. While in insert mode, you are able to write and enter text like in a regular text editor. 

## Visual [v]
**Visual mode** can be accessed by pressing the `v` key while in Normal mode. While in Visual mode, you are able to select characters, strings and lines before using a command to modify it.

# Commands
## Saving and Quitting
While in Normal mode, you are able to type in the `Command Palette`, the line at the very bottom of your screen.

Typing in `:q` in the command palette and hitting `Enter` will quit you out of vim.

Typing in `:q!` in the command palette and hitting `Enter` will quit you out of vim without saving.

Typing in `:w` in the command palette and hitting `Enter` save your current work.

## Movement
### By Characters
While in Normal mode, Basic one cursor movements are handled by the `h`, `j`, `k` and `l` keys.

`h` - Left

`j` - Up

`k` - Down

`l` - Right

### By Words
Word specific movements are handled by the `b`, `e` and `w` keys.

`b` - beginning of the word.

`e` - end of the word.

`w` - beginning of the next word.

### Modifiers
Movement can be modified by `numbers`.

`2b` will move to the beginning of the send word.

`4l` will move 4 characters to the right.

`0` will move you to the beginning of the line.

`$` will move you to the end of the line.

### Moving in Lines
`*` will find the *next* occurence of the word your cursor is under.

`#` will find the *previous* occurence of the word your cursor is under.

### Moving to Lines
`gg` will move you to the beginning of the file.

`G` will move you to the end of the file.

`36G` will move you to line 36.

### Moving in Blocks
When in Normal mode, while hovering over a`(`, `[` or `{` character, you can go to the matching `)`, `]` or `}` with the `%` key.

## Search
`/` will bring up search on the command palette. 
`/text` will search for the string `text` in the file.

`n` will move to the next instance of the string you're searching for.

`N` will move to the previous instane of the string you're searching for.

## Inserting
`i` will enter you into Insert mode and let you type to the left of the cursor.

`I` will enter you into Insert mode and let you type at the first non-blank character.

`a` will enter you into Insert mode and let you type to the right of the cursor

`A` will enter you into Insert mode and let you type at the end of the line.

`o`

`O`



## Deleting
`x` will delete characters to the RIGHT of the cursor.

`X` will delete character to the LEFT of the cursor.

`dd` will delete the line you are hovering over.

`db` will delete from the cursor to the beginning of the word.

`de` will delete from the cursor to the beginning of the next word.

`dw` will delete from the cursor to the end of the current word.

You can modify the commands above with `numbers` after the `d` command.

`d4w` will delete 4 words starting from the cursor.

## Deleting then Inserting
`s` will delete 

`S`

`C`

## Multiple Insert
While in Normal mode, you can insert text multiple times by modifying the `i` function with a `number`.

`6i-` then hitting the `Esc` key will enter `------`.

## Replacing
`r` will allow you to replace/insert onto the character you are highlighting.

## Repeat Command
`.` will repeat the previous command used.

## Undo and Redo
`u` will undo your previous action.

`CTRL+R` will redo you previous action.

## New Lines
`o` will create a new line underneath your selected line.

`O` will create a new line above your selected line.


# References
- Shawn Biddle's vim classes
  - https://github.com/shawncplus/vim-classes
  - https://youtu.be/Nim4_f5QUxA
- Open Vim's Tutorial
  - https://www.openvim.com/
