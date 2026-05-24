# open_as_ghostty_tab

macOS AppleScript utility for opening a command in a new Ghostty tab. It can be run with a command-line argument, or without arguments to show a GUI selector backed by `connect.conf`.

## Requirements

- macOS
- Ghostty
- some macOS permissions

## Files

- `open_as_ghostty_tab.applescript`: main script
- `connect.conf.example`: example menu configuration
- `connect.conf`: local configuration file, expected next to the script

## Usage

Run with a command argument to open that command directly:

```console
$ osascript open_as_ghostty_tab.applescript "ssh -A user@host.example.com"
```

Run without arguments to show the GUI selector:

```console
$ osascript open_as_ghostty_tab.applescript
```

For GUI mode, create `connect.conf` in the same directory as `open_as_ghostty_tab.applescript`:

```console
$ cp connect.conf.example connect.conf
```

Ghostty launch behavior:

1. If Ghostty is already running with a front window,
    - The script creates a new tab in that window, sets `TERM` from the script environment's `$TERM`, applies the selected title, moves the new tab to the end of the tab list, and returns focus to the previously selected tab.
2. If Ghostty is running without a front window
    - The script creates a new window with the selected command.
3. If Ghostty is not running
    - The script launches Ghostty with the selected command and applies the selected title to the initial tab.

## Configuration

`connect.conf` supports one entry per line. Blank lines and lines whose trimmed text starts with `#` are ignored.

```conf
# comment
title="=======    SEPARATOR    ======="
command="ssh -A user@host.example.com"
title="some command"; command="/path/to/command"
title="zsh (localhost)"; command="zsh -l"
```

Supported line forms:

```conf
title="menu title"; command="/path/to/command --option value"
command="/path/to/command --option value"
title="menu header"
raw command string
```

- If both `title` and `command` are present, `title` is shown in the menu and `command` is run.
- If only `command` is present, the command is also used as the menu title.
- If only `title` is present, it acts like a non-launching menu header. Selecting it returns to the menu.
- If neither key is present, the whole line is used as both the menu title and the command.

The parser reads quoted values after `title="` and `command="`. It does not interpret escape sequences inside quoted values.

## Notes

- Only the first command-line argument is used.
- In GUI mode, `connect.conf` is resolved relative to the script file. If the script is run unsaved from Script Editor, it falls back to `~/Desktop/connect.conf`.
- Commands are passed to Ghostty as the command string for the new surface.

## License

GPL-3.0. See [LICENSE](LICENSE).
