# Managing Multiple Apps with tmux
A comprehensive guide to using tmux for running multiple terminal sessions within a single SSH connection.

## What is tmux?
tmux (terminal multiplexer) allows you to run multiple terminals in the background within a single SSH session. It's perfect for managing long-running processes, organizing your workflow, and maintaining persistent sessions even when disconnected.

**Key Concept**: All tmux commands use `Ctrl + B` as the prefix key combination.

## Installation
```bash
sudo apt install tmux
```

## Session Management
These commands are run from your main terminal (outside of tmux):

### Starting Sessions
```bash
# Start a new named session
tmux new -s my-session-name
# Start a new session with default name
tmux new
```

### Managing Existing Sessions
```bash
# List all running sessions
tmux ls
# Attach to a specific session
tmux attach -t my-session-name
# Attach to the most recent session
tmux attach
```

### Killing Sessions
```bash
# Kill a specific session
tmux kill-session -t my-session-name
# Kill all sessions
tmux kill-server
```

## Inside a Session: Essential Keybindings
**Remember**: Press `Ctrl + B`, release, then press the command key.

### Session Control
| Key | Action |
|-----|--------|
| `d` | Detach from current session (session keeps running) |
| `s` | Switch sessions using interactive list |
| `L` | Switch to last active session |

### Window Management
| Key | Action |
|-----|--------|
| `c` | Create new window (like a browser tab) |
| `p` | Go to previous window |
| `n` | Go to next window |
| `0-9` | Switch to window number |
| `&` | Kill current window |
| `,` | Rename current window |

### Pane Management
| Key | Action |
|-----|--------|
| `%` | Split window vertically |
| `"` | Split window horizontally |
| `arrow keys` | Navigate between panes |
| `x` | Kill current pane |
| `z` | Zoom/unzoom current pane |

### Scrolling in tmux
**Problem**: Your mouse wheel doesn't scroll in tmux like normal terminals.

**Quick Fix** - Enable mouse mode:
```bash
# Add to ~/.tmux.conf
echo "set -g mouse on" >> ~/.tmux.conf
# Reload config
tmux source-file ~/.tmux.conf
```
Now your mouse wheel works normally!

**Alternative** - Keyboard scrolling:
| Key | Action |
|-----|--------|
| `[` | Enter copy mode (scroll mode) |
| `arrow keys` | Scroll line by line |
| `Page Up/Down` | Scroll by page |
| `q` | Exit copy mode |

### System Commands
| Key | Action |
|-----|--------|
| `:` | Open tmux command prompt |
| `?` | Show all keybindings |

## Common Workflow Examples

### Basic Development Setup
```bash
# Start a named session for your project
tmux new -s myproject
# Create windows for different tasks
Ctrl + B, c  # New window for code editor
Ctrl + B, c  # New window for server
Ctrl + B, c  # New window for logs
# Navigate between windows
Ctrl + B, 0  # Switch to window 0
Ctrl + B, 1  # Switch to window 1
```

### Detaching and Reattaching
```bash
# While working in tmux
Ctrl + B, d  # Detach (session continues running)
# Later, from terminal
tmux attach -t myproject  # Reattach to continue work
```

## Tips and Best Practices
- **Name your sessions**: Use descriptive names like `tmux new -s webserver` instead of default names
- **Detach, don't exit**: Use `Ctrl + B, d` to detach rather than closing terminals
- **Use windows for organization**: Create separate windows for different aspects of your project
- **Learn pane splitting**: Use `%` and `"` to split windows into multiple panes for side-by-side work
- **Enable mouse mode**: Add `set -g mouse on` to ~/.tmux.conf for easier scrolling and pane selection

## Troubleshooting
**Session not found**: Use `tmux ls` to see available sessions
**Can't attach**: Session might be attached elsewhere - try `tmux attach -d -t session-name` to force detach others
**Commands not working**: Make sure you're pressing `Ctrl + B` first, releasing, then the command key
**Can't scroll**: Enable mouse mode or use `Ctrl + B, [` for keyboard scrolling

---
*This guide covers the essential tmux commands for managing multiple applications efficiently. For advanced features, consult the tmux manual with `man tmux`.*
