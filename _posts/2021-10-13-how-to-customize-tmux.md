---
title: "How to customize tmux"
date: 2021-10-13T00:00:00+00:00
author: Rajashekar Chintalapati
layout: post
toc: true
permalink: /customize-tmux/
categories: tmux
tags: [tmux]
---
With Tmux you can manage multiple sessions/terms & navigate quickly.

## Customizing Tmux
Below is how you can customize tmux. 
- Create `~/.tmux.conf` if it does not exist.
- Enable 256 colors.
    ```bash

    # tmux display things in 256 colors
    set -g default-terminal "screen-256color"
    #  enable terminal compability
    set-option -ga terminal-overrides ",tmux-256color-italic:Tc"

    ```
- Changing default prefix from `Ctrl+b` to `Ctrl+a`
    ```bash

    # unbind default prefix and set it to Ctrl+a
    unbind C-b
    set -g prefix C-a
    bind C-a send-prefix

    ```
- Add shortcut `Ctrl+a r` to apply tmux configuration while inside tmux session
    ```bash

    # reload config file
    bind r source-file ~/.tmux.conf \; display "Config Reloaded!"

    ```
- Add shortcut `Ctrl+a |` for vertical split, `Ctrl+a -` for horizontal split
    ```bash

    bind | split-window -h -c "#{pane_current_path}"
    bind - split-window -v -c "#{pane_current_path}"

    ```
- Add shortcut `Ctrl+a y` to synchronize panes (meaning, if you have 2 or more panes in same window, you can send command to all panes at once)
    ```bash

    # synchronize all panes in a window
    bind y setw synchronize-panes

    ```
- To move between panes - left (`Ctrl+a h`) , down (`Ctrl+a j`), up (`Ctrl+a k`), right (`Ctrl+a l`).
    ```bash

    # pane movement shortcuts
    bind h select-pane -L
    bind j select-pane -D
    bind k select-pane -U
    bind l select-pane -R

    ```
- To move between windows - left (`Ctrl+a Ctrl+h`), right (`Ctrl+a Ctrl+l`)
    ```bash

    bind -r C-h select-window -t :-
    bind -r C-l select-window -t :+

    ```
- Enable mouse and vi mode
    ```bash

    # enable mouse support for switching panes/windows
    setw -g mouse on
    # set vi mode for copy mode
    setw -g mode-keys vi

    ```
- Copy & paste in vi mode and attach OS clipboard.
    ```bash

    # more settings to make copy-mode more vim-like
    unbind [
    bind Escape copy-mode
    unbind p
    bind p paste-buffer
    bind -Tcopy-mode-vi v send -X begin-selection

    # Buffers to/from Mac clipboard, yay tmux book from pragprog
    # Copy-paste integration
    set-option -g default-command "reattach-to-user-namespace -l zsh"
    bind -Tcopy-mode-vi y send -X copy-pipe "reattach-to-user-namespace pbcopy"

    # Update default binding of `Enter` to also use copy-pipe
    unbind -Tcopy-mode-vi Enter
    bind -Tcopy-mode-vi Enter send -X copy-pipe "reattach-to-user-namespace pbcopy"
    # Bind ']' to use pbpaste
    bind ] run "reattach-to-user-namespace pbpaste | tmux load-buffer - && tmux paste-buffer"

    ```
    > **Note**: On MacOS, you need to install `brew install reattach-to-user-namespace`. For unix/windows use another program to handle this
- To quickly jump between panes - `Ctrl+a a`
    ```bash

    # to show all panes
    bind a choose-tree

    ```
    > **Note**: by default, `Ctrl+a w` shows all windows in all sessions, but you need to go to each window to expand window. With above you will get all panes expanded. 
- You can customize theme like below, create `~/.dotfiles/tmux/theme.sh` and add below
    ```bash

    #### COLOUR
    tm_icon="🙈 🙉 🙊"
    tm_color_active=colour51
    tm_color_inactive=colour241
    tm_color_feature=colour198
    tm_color_music=colour41
    tm_active_border_color=colour51

    # separators
    tm_separator_left_bold="◀"
    tm_separator_left_thin="❮"
    tm_separator_right_bold="▶"
    tm_separator_right_thin="❯"

    set -g status-left-length 32
    set -g status-right-length 150
    set -g status-interval 5

    # default statusbar colors
    # set-option -g status-bg colour0
    set-option -g status-style fg=$tm_color_active,bg=default
    #set-option -g status-attr default

    # default window title colors
    set-window-option -g window-status-style fg=$tm_color_inactive,bg=default
    set -g window-status-format "#I #W"

    # active window title colors
    set-window-option -g window-status-current-style fg=$tm_color_active,bg=default
    set-window-option -g  window-status-current-format "#[bold]#I #W"

    # pane border
    set-option -g pane-border-style fg=$tm_color_inactive
    set-option -g pane-active-border-style fg=$tm_active_border_color

    # message text
    set-option -g message-style fg=$tm_color_active,bg=default

    # pane number display
    set-option -g display-panes-active-colour $tm_color_active
    set-option -g display-panes-colour $tm_color_inactive

    # clock
    set-window-option -g clock-mode-colour $tm_color_active

    tm_session_name="#[fg=$tm_color_feature,bold]$tm_icon #S"
    set -g status-left $tm_session_name' '

    ```
- You can use [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm) to install tmux plugins, below are some interesting plugins.
    ```bash

    # List of plugins
    set -g @plugin 'tmux-plugins/tpm'
    set -g @plugin 'tmux-plugins/tmux-sensible'
    set -g @plugin 'tmux-plugins/tmux-sessionist'
    set -g @plugin 'tmux-plugins/tmux-copycat'
    set -g @plugin 'tmux-plugins/tmux-prefix-highlight'
    set -g @plugin 'christoomey/vim-tmux-navigator'
    set -g @plugin 'tmux-plugins/tmux-open'
    set -g @plugin 'tmux-plugins/tmux-resurrect'
    set -g @plugin 'tmux-plugins/tmux-continuum'

    set -g status-right "#{prefix_highlight}"

    # Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
    run '~/.tmux/plugins/tpm/tpm'

    ```

## Navigating Tmux
### Terminal to tmux.
- To create new session `tmux new -s sesson-name`
- To kill session `tmux kill-ses -t session-name`
- To re-attach to previous session `tmux a`. To specific session `tmux a -t session-name`
- To list all tmux sessions - `tmux ls`
### Inside Tmux
- Show all shortcuts `ctrl + a ?`
- Copy Mode `ctrl + a ESC`
- Command Mode `ctrl + a :`
#### Sessions
- `:new -s newsession` to create new session
- `ctrl + a )` go to next session
- `ctrl + a (` go to previous session
- `ctrl + a s` show all sessions
- `ctrl + a $` rename session
- `ctrl + a d` exit tmux session go back to terminal
#### Windows
- `ctrl + a c` create new window
- `ctrl + a ,` rename window
- `ctrl + a &` kill window
- `ctrl + a ctrl + w` display all sessions and windows
- `ctrl + a ctrl + l` move to next window in current session
- `ctrl + a ctrl + h` move to previous window in current session
#### Panes
- `ctrl + a |` create vertical pane
- `ctrl + a -` create horizontal pane
- `ctrl + a x` kill pane
- `ctrl + a space` re-arrange panes
- `ctrl + a ;` toggle between panes
- `ctrl + a o` move to next pane
- `ctrl + a l` - move right, `ctrl + a h` - move left
- `ctrl + a j` move up, `ctrl + a` k move down
- `ctrl + a z` zoom to pane (toggle)
- `ctrl + a !` convert pane to window
- `ctrl + a f` search all panes for given text
#### Buffers
- `ctrl + a #` list buffers
- `ctrl + a =` list buffers to copy
- `/` to search in buffers
- `: capture-pane` capture visibile details in buffer
- `:save-buffer` buf.txt to save