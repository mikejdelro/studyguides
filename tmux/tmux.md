# tmux - terminal multiplexer

## What the?
`tmux` allows for multiple terminal sessions to be accessed simultaneously in a single window. Supposing that you're running a backup on one SSH instance, and it estimatedly has a run time of 3-4 hours-- this pretty much means you're stuck keeping that SSH instance open, as closing or killing that connection will kill the backup process. `tmux` allows you to run terminal processes and then *detach* from them, allowing the SSH session to remain active without being visible.

## Installation
```bash
sudo apt-get update -y && sudo apt-get install tmux && tmux
```

## Usage
First off, we have to understand that `CTRL+B` is now bound to `tmux`, specifying that any next command is no longer directed towards `bash`, but to `tmux`. 

## Commands

### Windows

`CTRL+B`, `c` : New Window

`CTRL+B`, `w` : List open Windows

`CTRL+B`, `&` : Close current Window

`CTRL+B`, `,` : Rename current Window

`CTRL+B`, `p` : Previous Window

`CTRL+B`, `n` : Next Window

### Panes

`CTRL+B`, `%` : Split Window vertically

`CTRL+B`, `"` : Split Window horizontally

`CTRL+B`, `x` : Close current Pane

### Sessions

`tmux new -s <nameOfSession>`




## References
[tmux](https://en.wikipedia.org/wiki/Tmux) *from Wikipedia, the free encyclopedia*

[Basic Tmux Tutorial - Windows, Panes, and Sessions Over Ssh](https://www.youtube.com/watch?v=BHhA_ZKjyxo) *by tutorialLinux*

