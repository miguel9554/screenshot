# screenshot

Region screenshot with a save dialog.

1. `maim -s` (falls back to `scrot -s`) for the region select.
2. A small GTK window: filename entry + destination folder picker.
3. Writes the PNG to the chosen path.

The image goes to the clipboard (`xclip`) as soon as the region is grabbed,
*before* the dialog opens, so anything you copy while typing a filename
replaces it. Cancelling the dialog still leaves the image on the clipboard.

Copying text (Ctrl+C) inside the dialog works and survives it: X11 clipboard
ownership normally dies with the process and dwm runs no clipboard manager,
so on exit any text you copied is handed to `xclip`, which detaches and keeps
serving it.

The last folder used is stored in `$XDG_STATE_HOME/screenshot/lastdir`
(default `~/.local/state/screenshot/lastdir`) and preselected next time.
First run defaults to `~/Pictures/screenshots`.

The window is sized to the capture (down to ~240px wide, preview capped at
360x200) and asks the WM to float it: it sets `_NET_WM_WINDOW_TYPE_DIALOG`,
which dwm floats without any rule. If you ever need an explicit rule, its
WM_CLASS is `screenshot` / `Screenshot`:

```c
{ "Screenshot", NULL, NULL, 0, 1, -1 },
```

Keys: `Enter` saves, `Esc` cancels. Cancelling the region select or the
dialog leaves nothing behind. Existing names are never overwritten — a
`_1`, `_2`, ... suffix is added.

Deps: python-gobject, gtk3, maim (or scrot), xclip.

## dwm

```c
static const char *selectscreenshotcmd[] = {
    "/home/miguel/dev/suckless/screenshot/screenshot", NULL
};
```
