# D-SMS

**A text and picture messaging app for keypad flip phones.** D-SMS is built on
[NovaLox](https://github.com/theOnionsAreWatching/NovaLox) by
theOnionsAreWatching, a messaging app made for keypad phones, and adds many
fixes and features on top. It's built for the TCL Flip 2.

The fixes and features in D-SMS were made with the help of AI.

---

## What's different from NovaLox

D-SMS starts from NovaLox v0.9.114 and adds nearly 200 fixes and features. The
highlights:

- **Made for the keypad.** Every action happens when you release a key, every
  screen works with the D-pad and soft keys, soft keys work without setup on
  phones that report them, and the navigation bar is never hidden.
- **Faster big chats and chat list.** Steady scrolling, no jumping highlight,
  chats open at the first unread message, jump keys (1 = top, 7 = bottom,
  LEFT/RIGHT = one screen), and a full-screen reader for long messages.
- **Fewer message problems.** Fixes for duplicate messages in groups, blank or
  "Unknown" MMS, messages staying unread, and attachments that were silently
  dropped.
- **Better attachments.** Built-in Photo/Video picker, Camera, file browser,
  multi-pick emoji, a sharp zoomable picture viewer, and MMS size limits
  checked before sending.
- **Voice notes that fit one MMS.** Short file names, auto-stop at the carrier
  limit, smaller files, earpiece playback and a keypad-friendly player.
- **Stronger backup and restore.** SMS only, MMS only or both; settings saved
  inside the backup; restore finds the file for you, shows real progress, runs
  faster and can be cancelled safely.
- **Cleaner settings.** Ten short sections, a settings search, current values
  shown on each row, Save/Restore settings, more themes, accents and message
  styles.
- **New vibration patterns** for incoming messages.
- **Reactions.** "Liked …", "Loved …" and similar texts from iPhones and Google
  Messages show as a small badge on the original message.
- **One themed look.** Rounded pop-ups and toasts in your accent color, sized
  to fit a small screen.
- **Optional Notes chat** that saves notes on the phone and never sends them.

The full list, one line per change, is in
**[docs/D-SMS-CHANGES.md](docs/D-SMS-CHANGES.md)**.

## Features

- Conversations open immediately, even with years of history
- Full D-pad operation: hold to scroll, jump keys, a clear focus outline, and
  soft-key labels on every screen
- Physical soft-key support, detected automatically where possible, with a
  setup screen as a fallback
- Press a message to act on its contents: call a number, open a link, save a
  contact
- Group messages as one group conversation or as individual replies
- Per-conversation notification tone, vibration, or silence; a choice of
  vibration patterns; keyword blocking
- Lock messages, recycle bin, scheduled sending, drafts with attachments
- Attach pictures, videos, voice notes, audio, files and contact cards, several
  at once; share into the app from the gallery
- Reactions from iPhones and Google Messages shown on the original message
- Read and delete messages stored on the SIM card
- Whole-app size from 80% to 120%, plus separate text size settings
- Built-in update check that works behind filtered networks
- Backup and restore to a single file, including attachments and settings;
  restore writes into the phone's message storage

## Installing

1. On the phone, open **[Releases](../../releases/latest)** and download the
   `D-SMS-X.Y.Z-PF.apk` file.
2. Open the downloaded file. If the phone asks, allow installing from this
   source (one-time).
3. Open D-SMS and follow the setup: make it the default messaging app, grant
   the permissions it asks for, and let it import your existing messages.
4. The import can run in the background. Press **Run in background** and start
   using the app right away.

The APKs on this page are the PureFlip build. On a PureFlip phone they install
as an update over the D-SMS that comes with the ROM. D-SMS uses the same app id
as NovaLox, so it can't be installed over NovaLox. To switch, back up in
NovaLox first, uninstall it, install D-SMS, then restore.

## Updating

D-SMS checks this page for a new release about once a week and asks before
downloading. You can also check any time in **Settings → Advanced → Check for
updates**, or turn the weekly check off there.

Before the install screen opens, D-SMS checks that the download is really a
newer D-SMS signed with the same key. Anything else is deleted with a short
message. The downloaded file is removed after the update installs (there's a
switch for that in the same place).

## Moving to a new phone

1. Old phone: **Settings → Backup & restore → Back up**, choose SMS, MMS or
   both. The file goes to the D-SMS/Backups folder.
2. New phone: install D-SMS, then **Settings → Backup & restore → Restore**.
   D-SMS looks in the usual folders, can search the whole phone, or lets you
   pick the file yourself.

Restore writes your messages into the phone's real message storage, so they
become normal messages visible to any messaging app. Messages already on the
phone are kept without duplicates.

## Tips

- **A soft key doesn't respond?** Some phones never deliver those keys to apps.
  Turn the soft-key bar off (**Settings → Keys → Soft key bar**) and on-screen
  buttons that work with the D-pad appear instead.
- **Group messages look mixed up, or pictures sit in the wrong chat?** Run
  **Settings → Advanced → Rebuild all chats** once. It rebuilds everything from
  the phone's message storage.
- **Can't find a setting?** Use **Settings → Search**.
- **Forgot a key?** **Settings → Keys → Key guide** lists them all.

## Privacy

Your messages stay on your phone. D-SMS has no servers, no ads and no
analytics. It makes no network connections except checking this page for
updates (and sending and receiving MMS through your carrier, of course).

## Credits

- **NovaLox** by theOnionsAreWatching:
  https://github.com/theOnionsAreWatching/NovaLox. D-SMS is a modified
  version of NovaLox and keeps its license.
- **DPAD Messaging** (https://github.com/jbriones95/DPAD-Messaging, reactions
  work from the UncleAndy123 fork): the reaction parser and matcher are ported
  from it under the MIT License, Copyright (c) 2025 DPAD Messaging. The notice
  is kept in the ported files.
- D-SMS changes by [naftulyhash101](https://github.com/naftulyhash101).

## License

D-SMS, like NovaLox, is free to use, copy, and share for **noncommercial
purposes** (personal use, family, schools, charities and the like) under the
[PolyForm Noncommercial License 1.0.0](LICENSE.md).

Using or distributing it **commercially** (for example, preinstalling it on
phones you sell, or bundling it with a paid service) requires permission from
the NovaLox author.

Parts ported from DPAD Messaging are also under the MIT License (see the
notices in those files).

Required Notice: Copyright theOnionsAreWatching
(https://github.com/theOnionsAreWatching/NovaLox)
