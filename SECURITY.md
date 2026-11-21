# Security policy

## Supported versions

Only the latest release of Drawer receives security fixes. Drawer updates itself, so most people are on the latest version within a day of a release.

| Version | Supported |
| --- | --- |
| Latest release | ✅ |
| Older releases | ❌ |

## Reporting a vulnerability

Please report security problems **privately**, not in a public issue.

Message **`@Drawer_app` on Telegram: [t.me/Drawer_app](https://t.me/Drawer_app)**, starting your message with "Security report".

Include:

- what the problem is and what someone could do with it;
- the steps to reproduce it;
- your Drawer version (right-click the chevron ▸ Settings… ▸ Updates) and macOS version.

You can expect a first reply within a few days. Once a fix is released, you're welcome to be credited in the release notes if you'd like.

## What counts

Examples of things worth reporting:

- anything that lets another app or website use Drawer's Accessibility or Screen Recording access;
- anything that could make Drawer install an update that didn't come from this repository;
- anything that sends data from your Mac somewhere other than this repository's update check.

## What Drawer does and doesn't access

Drawer uses Accessibility to read and open menu bar icons and to switch apps on and off under **System Settings ▸ Menu Bar ▸ Allow in the Menu Bar**. Screen Recording is optional and only used to take small pictures of menu bar icons, which stay on your Mac. Its only network connection is checking this repository's releases for updates, and every update is verified against a signature before it is installed.
