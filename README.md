<img width="256" height="64" alt="Drawer1" src="https://github.com/user-attachments/assets/5c7d0816-ed65-47a5-84c4-b5ca5e90f698" />


# Drawer

A drawer for your Mac's menu bar. Tuck the icons you don't want to look at into a glass drawer that drops down from a single chevron, and open any of them from there.

Drawer is modelled on the overflow arrow in the Windows system tray: the chevron always sits at the left end of your menu bar icons, and everything you choose to put away lives behind it.

<!-- Add a screenshot or short screen recording here: docs/screenshot.png -->

## What it does

- **Hides menu bar icons you choose.** Nothing is hidden until you put it in the drawer.
- **Three ways to add an icon.** Right-click it and choose **Add to Drawer**, ⌘-drag it onto the chevron, or ⌘-drag it down into the open drawer.
- **Opens hidden icons from the drawer.** Click a tile and that icon's menu opens, even though the icon is no longer on the bar.
- **Holds apps, files and folders too.** Drop anything onto the drawer for quick access.
- **Two looks.** *Compact* is a single row of icons the same height as the menu bar. *Full* is a panel with a name under each icon. Switch in Settings.
- **Icons that look like the menu bar.** Optionally, tiles show each icon exactly as it appears in the menu bar instead of the app's icon.
- **Stays first on the left.** If another icon lands to the left of the chevron, Drawer moves back in front of it.
- **Never moves your pointer.** Drawer does all of this without simulating mouse input.
- **Starts at login**, if you turn that on.
- **Cleans up after itself.** Quit Drawer and your hidden icons go back on the menu bar.

## Requirements

- macOS 26 or later. Drawer is built and tested on macOS 27.
- Accessibility permission (see below).

## Install

1. Download `Drawer.dmg` from the [latest release](../../releases/latest).
2. Open it and drag **Drawer** to **Applications**.
3. Open Drawer from Applications. A chevron appears in your menu bar.
4. Grant Accessibility access when asked: **System Settings ▸ Privacy & Security ▸ Accessibility**, then switch **Drawer** on.

Drawer should run from the Applications folder. Some menu bar managers hide the icons of apps started from other folders.

## Using it

| To do this | Do this |
| --- | --- |
| Open the drawer | Click the chevron |
| Add an icon | Right-click it ▸ **Add to Drawer**, or ⌘-drag it onto the chevron |
| Open a hidden icon | Click its tile in the drawer |
| Put an icon back on the bar | Drag its tile up onto the menu bar, or right-click it ▸ **Put Back on Menu Bar** |
| Add an app, file or folder | Drop it on the open drawer, or use the **+** button |
| Reach an icon's own right-click menu | Hold Option while right-clicking it |
| Settings, license, quit | Right-click the chevron |

## Why it needs Accessibility

Drawer uses the Accessibility permission for three things, and nothing else:

1. Reading which icons are on the menu bar and where.
2. Opening a hidden icon's menu when you click its tile.
3. Flipping an app's switch under **System Settings ▸ Menu Bar ▸ Allow in the Menu Bar**, which is how an icon is hidden and brought back.

Drawer makes no network connections and does not record your keystrokes.

### Screen Recording (optional)

If you turn on **Settings ▸ Appearance ▸ Show icons as they look in the menu bar**, Drawer asks for Screen Recording access. macOS gives apps no other way to see another app's menu bar icon. Drawer uses it to take one small still picture of each icon, cropped to the icon alone, at the moment the icon is on the bar. The pictures stay on your Mac in `~/Library/Application Support/Drawer/Icons`. Drawer never records video and never captures anything but those icon rectangles.

Leave the option off and Drawer never asks; tiles show each app's own icon instead. After granting the permission, quit and reopen Drawer.

## How hiding works

On macOS 26 and later the system decides which apps may show icons in the menu bar. Drawer hides an icon by switching its app off in that list, and restores it by switching the app back on. Two things follow from that:

- **Hiding is per app.** If an app has two menu bar icons, both are hidden together.
- **Apple's own icons can't be added.** Wi-Fi, Battery, Control Center and the like have their own switches in **System Settings ▸ Menu Bar**.

**Quitting puts everything back.** When you quit Drawer, every icon in the drawer returns to the menu bar, so nothing is left hidden with no way to reach it. Your drawer is remembered: the next time Drawer starts, those icons are tucked away again. Quitting takes a few seconds while the icons are restored.

The one exception is logging out, restarting or shutting down, where there is no time to restore them. If Drawer is set to start at login, it picks up where it left off. If it is not, switch the apps back on under **System Settings ▸ Menu Bar ▸ Allow in the Menu Bar**.

## Trial and license

Drawer is free to try for 7 days. After that it asks for a license key. When a trial ends, the drawer offers a one-click **Put My Icons Back on the Menu Bar**, so nothing is ever held hostage.

A license is for one Mac. To buy one:

1. Right-click the chevron ▸ **License…** and copy the **device code** shown there. It looks like `ABCD-EFGH-JKMN-PQRS`.
2. Include that code with your purchase.
3. You receive a key starting `DRAWER2.`. Paste it into the same window and click **Activate**.

The key works only on the Mac the device code came from. Moving to a new Mac means asking for a new key. Keys are checked on your Mac; no account or internet connection is needed. The device code is a scrambled fingerprint of the Mac's hardware ID and does not reveal the ID itself.

## Building from source

You need Xcode 26 or later.

```sh
git clone <this repository>
cd <repository folder>
open Drawer.xcodeproj
```

Select your own team under **Signing & Capabilities**, then build. Two scripts cover the common cases:

- `scripts/dev-install.sh` builds a development copy with test hooks and installs it to `/Applications`.
- `scripts/ship.sh` builds the shipping copy and packages `dist/Drawer.dmg`.

License keys are signed with Ed25519. The app carries only the public key; `scripts/license-tool.swift` issues keys with a private key that is kept outside the repository. If you fork Drawer, run `swift scripts/license-tool.swift init` and put your own public key in `Drawer/Licensing.swift`.

## Troubleshooting

**The menu bar stays down in full screen.** macOS sometimes gets stuck showing the menu bar (and an app's toolbar) in full screen instead of letting them slide away. Drawer leaves the menu bar alone while an app is in full screen and for a few seconds either side of it. If it happens, try these in order before restarting:

```sh
killall MenuBarAgent
```

```sh
killall Dock
```

Both restart a part of macOS that relaunches by itself within a second or two; nothing is lost. If it keeps happening, the file `~/Library/Application Support/Drawer/activity.log` records what Drawer did to the menu bar and when, which helps track down the cause.

## Known limitations

- The menu of a hidden icon opens where the icon used to sit, not under the chevron.
- Hiding or restoring an icon takes a couple of seconds, and System Settings may appear briefly in the Dock while it happens.
- Dropping onto the chevron needs ⌘ held, because that is the only way macOS lets a menu bar icon be picked up.

## Acknowledgements

Drawer was inspired by [Thaw](https://github.com/thaw-app/Thaw), the open source menu bar manager. It shares no code with it.
