---
title: "10x Your Productivity Using Tmux"
date: 2025-01-03T21:59:23+05:30
draft: false
description: Configure tmux to make yourself a 10x developer or a gigachad hacker.
tags: [blog, tmux, productivity, linux, unix, cybersecurity]
---


<div class="dsclmr">Disclaimer: This guide will be having a tutorial to become a 10x dev or an epic GigaChad hacker. Viewer discretion advised.</div>

Let's be straight, in a world where we want our environment to be fast-paced and flexible, we really need a terminal multiplexer which is basically a browser for terminals inside a terminal if you what I am trying to say here. Here **tmux** comes to our rescue. **TMUX** is a **T**erminal **MU**ltiple**X**er which removes the need to juggle through multiple terminal windows and does those jobs in a single terminal window. Using **tmux** is like having a Swiss Army Knife for your CLI -- allowing you to manage sessions, split windows, and streamline workflows with ease.

## Why tmux?
1. **Session Persistence:** Disconnect from your terminal and reconnect to the same without losing your workflow/session.
2. **Window Management:** If you are familiar with Window Managers like Awesome, BSPWM, and others you know what it means but for the normies. Window Management is the way we organize window panes for making the process of switching between each window seamless. Here we can implement the same as it as **split-window** capabilities for both vertical and horizontal splits.
3. **Customizability:** Configure it with custom keymaps and other ease of use shortcuts. It also support plugins but since we follow the suckless way for doing most of our projects. We avoid bulky plugins.
4. **Resource Efficiency:** Managing multiple tasks in a single terminal window may reduce processing overhead (*maybe idk*).

## Download tmux.
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

