---
title: "10x Your Productivity Using Tmux"
date: 2025-01-03T21:59:23+05:30
draft: false
description: Configure tmux to make yourself a 10x developer or a gigachad hacker.
tags: [productivity, linux, cybersecurity, tech]
---

![my_complicated_pile_of_SHIT](/img/tmux.png)

> Disclaimer: This guide will be having a tutorial to become a 10x dev or
> an epic GigaChad hacker. Viewer discretion advised.

Let's be straight, in a world where we want our environment to be fast-paced
and flexible, we really need a terminal multiplexer which is basically a
browser for terminals inside a terminal if you what I am trying to say here.
Here **tmux** comes to our rescue. **TMUX** is a **T**erminal
**MU**ltiple**X**er which removes the need to juggle through multiple terminal
windows and does those jobs in a single terminal window. Using **tmux** is like
having a Swiss Army Knife for your CLI -- allowing you to manage sessions,
split windows, and streamline workflows with ease.

## Why tmux?

1. **Session Persistence:** Disconnect from your terminal and reconnect to the
   same without losing your workflow/session.
2. **Window Management:** If you are familiar with Window Managers like
   Awesome, BSPWM, and others you know what it means but for the normies.
   Window Management is the way we organize window panes for making the process of
   switching between each window seamless. Here we can implement the same as it as
   **split-window** capabilities for both vertical and horizontal splits.
3. **Customizability:** Configure it with custom keymaps and other ease of use
   shortcuts. It also supports plugins but since we follow the suckless way for
   doing most of our projects. We avoid bulky plugins.
4. **Resource Efficiency:** Managing multiple tasks in a single terminal window
   may reduce processing overhead (_maybe IDK_).

## Download tmux

- **Debian/Ubuntu:**
```bash
sudo apt install tmux
```

- **REHL/CentOS:**
```bash
sudo yum install tmux
```

- **Arch:**
```bash
sudo pacman -Sy tmux
```

- **Void Linux:**
```bash
sudo xbps-install -Sy tmux
```


## Get my configs
You can get the file via my [github repo](https://github.com/iamb4uc/dots) or just copy paste this:
```config
# Unbind default keys
unbind C-b
unbind '"'
unbind %

# Set prefix key to C-a (Ctrl+a)
set-option -g prefix C-a
bind-key C-a send-prefix

# Split window bindings
bind | split-window -h    # Split window horizontally
bind - split-window -v    # Split window vertically

# Reload tmux config
bind r source-file $HOME/.config/tmux/tmux.conf \; display "Reloaded tmux config!"

# Reduce escape time for faster response
set -sg escape-time 5

# Enable mouse support
set -g mouse on

# Prevent automatic renaming of windows
set-option -g allow-rename off

# Use vi keys in status line and copy mode
set -g status-keys vi
setw -g mode-keys vi

# Disable visual alerts
set -g visual-activity off
set -g visual-bell off
set -g visual-silence off
setw -g monitor-activity off
set -g bell-action none

# Customize appearance
setw -g clock-mode-colour colour0
setw -g mode-style 'fg=colour1 bg=colour18 bold'
set -g pane-border-style 'fg=#ebdbb2'
set -g pane-active-border-style 'fg=#ebdbb2'

# Status bar settings
set -g status-position top
set -g status-justify left
set -g status-style 'fg=white'
set -g status-left ''
set -g status-right '%Y-%m-%d %H:%M '
set -g status-right-length 50
set -g status-left-length 10

# Window status format
setw -g window-status-current-format ' #I #W #F '
setw -g window-status-current-style 'fg=#282828 bg=#ebdbb2 bold'
setw -g window-status-style 'fg=#282828 dim'
setw -g window-status-format ' #I #[fg=colour7]#W #[fg=colour1]#F '
setw -g window-status-bell-style 'fg=colour2 bg=colour1 bold'

# Message style
set -g message-style 'fg=colour2 bg=colour0 bold'

# Pane selection bindings
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Pane resizing bindings
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# Window switching bindings
bind -r C-h select-window -t :-
bind -r C-l select-window -t :+

# History limit
set -g history-limit 5000

# Terminal settings
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"

# Window index settings
set -g base-index 1
set -g renumber-windows on

# Monitor activity in windows
setw -g monitor-activity on
set -g visual-activity on

# Copy mode settings (vim-style)
bind Escape copy-mode
bind C-[ copy-mode
bind -T copy-mode-vi 'v' send -X begin-selection
unbind -T copy-mode-vi Enter
bind -T copy-mode-vi Enter send -X copy-pipe-and-cancel "reattach-to-user-namespace pbcopy"
bind -T copy-mode-vi 'y' send -X copy-pipe-and-cancel "reattach-to-user-namespace pbcopy"

# Paste from buffer
bind p paste-buffer

# Quickly switch panes with Ctrl+J
unbind ^J
bind ^J select-pane -t :.+

# Additional improvements
# Automatically rename windows based on current command
setw -g automatic-rename on

# Status bar refresh interval
set -g status-interval 5

# Set default shell
set -g default-shell /bin/zsh
```
