# shorthand

An `abook`-style CLI tool for storing and managing your Linux shell aliases,
with the same kind of ncurses list-and-edit experience.

## Setup (Linux / macOS)

```bash
chmod +x shorthand.py
sudo mv shorthand.py /usr/local/bin/shorthand   # or anywhere on your PATH
```

## Setup (Termux, Android)

Termux ships Python without a `sudo` command and uses its own prefix for
installed binaries, so the steps are slightly different:

```bash
pkg update && pkg upgrade
pkg install python
```

Then, from the folder containing `shorthand.py`:

```bash
chmod +x shorthand.py
mv shorthand.py $PREFIX/bin/shorthand   # puts it on Termux's PATH, no sudo needed
```

Run it with:

```bash
shorthand
```

Everything else — `add`, `rm`, `list`, `export`, `install` — works exactly the
same as on Linux. `shorthand install` looks for `~/.bashrc` and `~/.zshrc`
under Termux's home (`/data/data/com.termux/files/home`); if you're on
Termux's default shell and neither file exists yet, create one first:

```bash
touch ~/.bashrc
shorthand install
source ~/.bashrc
```

## Interactive mode (abook-like UI)

```bash
shorthand
```

Keys:
- `j` / `k` or arrow keys — move down / up
- `a` — add a new alias
- `e` or Enter — edit the selected alias's command
- `d` — delete the selected alias (asks to confirm)
- `/` — search/filter by name or command, `Esc` clears it
- `s` — sync: writes `~/.shorthand/aliases.sh` and wires it into your shell rc
- `q` — quit

## Quick, non-interactive commands

```bash
shorthand add ll "ls -la"      # add or update an alias
shorthand rm ll                # remove one
shorthand list                 # list everything
shorthand export               # (re)write ~/.shorthand/aliases.sh
shorthand install              # add a source line to ~/.bashrc and ~/.zshrc
```

## How it works

- Aliases are stored as JSON in `~/.shorthand/aliases.json`.
- Every add/edit/delete also regenerates `~/.shorthand/aliases.sh`, a plain
  `alias name='command'` file.
- `shorthand install` adds one line to `~/.bashrc` / `~/.zshrc` that sources
  that file, so your aliases load in every new shell. Run it once, then
  `source ~/.bashrc` (or open a new terminal).

No dependencies beyond Python 3's standard library (`curses`, `json`).
