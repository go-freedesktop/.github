<p align="center"><img src="https://raw.githubusercontent.com/go-freedesktop/brand/main/social/go-freedesktop.png" alt="go-freedesktop" width="640"></p>

<h1 align="center">go-freedesktop</h1>
<p align="center">Pure-Go implementations of the freedesktop.org specifications — no cgo, no D-Bus C library, no external tools.</p>
<p align="center"><a href="https://go-freedesktop.github.io/docs/"><img src="https://img.shields.io/badge/docs-mkdocs--material-1E40AF?style=flat-square&logo=materialformkdocs&logoColor=white" alt="docs"></a> <a href="https://go-freedesktop.github.io/"><img src="https://img.shields.io/badge/site-go--freedesktop-3B82F6?style=flat-square" alt="site"></a> <img src="https://img.shields.io/badge/modules-9-2563EB?style=flat-square" alt="modules"> <img src="https://img.shields.io/badge/Go-1.26-00ADD8?style=flat-square&logo=go&logoColor=white" alt="Go"> <img src="https://img.shields.io/badge/license-BSD--3--Clause-1E40AF?style=flat-square" alt="license"></p>

---

## What is this?

`go-freedesktop` is a family of independent, dependency-light Go modules that
implement the [freedesktop.org](https://www.freedesktop.org/) specifications a
launcher, file manager or desktop shell relies on — enumerating installed
applications, resolving icons, classifying files by MIME type, choosing the
application that opens a type, building the category menu, and serving desktop
notifications. Every module is **pure Go, `CGO_ENABLED=0`**, and reuses the
standard XDG base-directory resolution rather than reinventing it.

They are the desktop-integration layer beneath [go-widgets](https://github.com/go-widgets)
launchers and shells: each spec is one small module, so a caller pulls in only
what it needs.

## Modules (9)

| Module | freedesktop specification | What it gives you |
|---|---|---|
| [`desktopentry`](https://github.com/go-freedesktop/desktopentry) | [Desktop Entry](https://specifications.freedesktop.org/desktop-entry-spec/latest/) | Enumerate the installed applications and expand their `Exec=` launch commands (field codes, `TryExec`, actions) — the piece a dock or app menu needs. |
| [`icontheme`](https://github.com/go-freedesktop/icontheme) | [Icon Theme](https://specifications.freedesktop.org/icon-theme/latest/) | Resolve an icon **name** (e.g. a `.desktop` `Icon=` value) to a file **path** at a target size and scale, following theme inheritance and the hicolor fallback. |
| [`mime`](https://github.com/go-freedesktop/mime) | [Shared MIME-info Database](https://specifications.freedesktop.org/shared-mime-info-spec/latest/) | Answer *"what type is this file?"* — resolve a canonical MIME type from a file's name, its content (magic), or both, against the on-disk shared database. |
| [`mimeapps`](https://github.com/go-freedesktop/mimeapps) | [MIME Applications Associations](https://specifications.freedesktop.org/mime-apps-spec/latest/) | Given a MIME type, the **default** application and the **ordered** list of candidates (`mimeapps.list`), with round-trippable edits — the engine behind *"Open With…"*. |
| [`menu`](https://github.com/go-freedesktop/menu) | [Desktop Menu](https://specifications.freedesktop.org/menu-spec/latest/) | Turn `applications.menu` XML into a resolved, categorized tree of app entries (Accessories, Graphics, System, …) for a launcher. |
| [`notifications`](https://github.com/go-freedesktop/notifications) | [Desktop Notifications](https://specifications.freedesktop.org/notification-spec/latest/) | The `org.freedesktop.Notifications` D-Bus **service** side — decode `Notify` calls (`notify-send`, libnotify, …) and render them as [go-widgets](https://github.com/go-widgets/toolkit) `Toast`s. |
| [`secretservice`](https://github.com/go-freedesktop/secretservice) | [Secret Service](https://specifications.freedesktop.org/secret-service-spec/latest/) | Store and read secrets where the desktop already keeps them — the `org.freedesktop.Secret.Service` D-Bus client that GNOME Keyring and KWallet answer. The Linux half of [go-keyring](https://github.com/go-keyring). |
| [`screencast`](https://github.com/go-freedesktop/screencast) | *not a spec — X11* | Capture the pixels of displays and windows: MIT-SHM 1.2 with descriptor passing, `GetImage` fallback, RANDR 1.5 / XINERAMA, XFIXES cursor. **Wayland is not implemented**, and `Diagnose()` says so rather than failing obscurely. |
| [`x11`](https://github.com/go-freedesktop/x11) | *not a spec — the protocol itself* | The foundation every X11 client needs first: wire codec, `.Xauthority`, connection setup, the MIT-SHM segment, the `SCM_RIGHTS` transport. Shared with [go-widgets/window](https://github.com/go-widgets/window), so one protocol parser cannot drift into two. |

> The list reflects the repos that actually exist in the org today. More
> freedesktop layers are added over time. Two entries are marked *not a spec*:
> `x11` is the X11 protocol's byte layer and `screencast` is the capture built
> on it. They live here because they are what a Linux desktop needs next, but
> the column heading would be a lie if they were left blank.

## Design

- **Pure Go, `CGO_ENABLED=0`** — one portable module per spec; cross-compiles to a static binary.
- **Reuse, don't reinvent** — XDG base-directory resolution comes from [`adrg/xdg`](https://github.com/adrg/xdg); the later waves build on the earlier ones (`mimeapps` and `menu` stand on `desktopentry`) instead of re-parsing `.desktop` files.
- **100% test coverage** is the bar, error branches included, on native amd64/arm64 plus emulated 64-bit targets.
- **BSD-3-Clause** throughout.

## Links

- 📖 Docs — <https://go-freedesktop.github.io/docs/>
- 🌐 Site — <https://go-freedesktop.github.io/>
- 🎨 Brand assets — <https://github.com/go-freedesktop/brand>

---
<p align="center"><sub>Branding in <a href="https://github.com/go-freedesktop/brand">go-freedesktop/brand</a>.</sub></p>
