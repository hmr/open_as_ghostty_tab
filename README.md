# open_as_ghostty_tab.applescript

A macOS AppleScript utility to execute commands in a new Ghostty tab.

It supports command-line arguments, or falls back to a connect.conf-powered GUI selector when run without parameters.

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

For GUI mode, place `connect.conf` in the same directory as the script:
:

```console
$ cp connect.conf.example connect.conf
```

## How this script launches Ghostty

- If Ghostty is already running <u>with a front window:</u>
    - creates a new tab in that window
- If Ghostty is running <u>without a front window:</u>
    - creates a new window with the specified command
- If Ghostty is <u>not running:</u>
    - launches Ghostty with the specified command

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
- If only `title` is present, it acts like a header in the menu. Selecting it returns to the menu.
- If neither key is present, the whole line is used as both the menu title and the command.

The parser reads quoted values after `title="` and `command="`. It does not interpret any escape strings inside quoted values.

## License

GPL-3.0. See [LICENSE](LICENSE).
