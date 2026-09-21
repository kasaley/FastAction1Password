# 1Password for FastAction

[Русская версия](README.md)

The 1Password items you reach for most, in the
[FastAction](https://github.com/kasaley/fastActionVersions) panel: fields within reach,
one-click copying, and filling straight into the app you are working in.

![The 1Password tab](https://github.com/kasaley/fastActionVersions/raw/main/docs/screenshots/tab-com.fastaction.onepassword.png)

## What it does

- **A list of items** from your vault — logins or everything, and you can narrow it to one vault.
- **The item's fields** in the panel: username, password, one-time code, link. One click copies.
- **The clipboard clears itself** after a time you choose — 45 seconds by default.
- **Filling**: the username and password go into the frontmost app at the press of a button.
  Switch it off in the plugin's settings.

## Installing

You need the official [1Password CLI](https://developer.1password.com/docs/cli/) (`op`)
with its 1Password app integration turned on — it is the 1Password app that asks you for
access, not the plugin.

**Settings → Plugins** in FastAction: the plugin is in the catalogue. By hand: download the
archive from [releases](../../releases) and pick it in **Settings → Plugins → Install plugin…**.

## What it asks for, and why

| Permission | Why |
| --- | --- |
| `exec` | to run `op`, the official 1Password CLI, and nothing else |
| `clipboard` | to copy an item's fields |
| `autofill` | to type the username and password into the frontmost app on request |
| `openURL` | to open the site an item points at |

The plugin has no network access at all: there is nowhere for it to send anything. It gets
passwords from `op` and puts them on the clipboard; they go no further.

`exec` is the one permission that reaches outside the sandbox, which is why it is the
narrowest: the app runs only `op`, only from known paths, never through a shell, and rejects
arguments such as `--config` or `--session` that could point the tool at a different source
of data.

## Development

Python 3 is all you need.

```sh
tools/fastplugin check     # the manifest parses and makes sense
tools/fastplugin stamp     # required after every edit to plugin.js
tools/fastplugin package   # build the archive and print its sha256
tools/fastplugin release   # tag, release, and the entry for the catalogue
```

`stamp` is not optional: the app compares the sha256 of the script with the one recorded in
the manifest and refuses to run the plugin when they disagree. That is what stops a plugin
edited after installation from using the permissions you granted earlier.

How plugins work in general: [the author's guide](https://github.com/kasaley/fastActionPlugins/blob/main/docs/PLUGINS.en.md).
The catalogue that carries the release entries: [fastActionPlugins](https://github.com/kasaley/fastActionPlugins).
