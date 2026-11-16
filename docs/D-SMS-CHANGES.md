# D-SMS: all fixes and features compared with NovaLox v0.9.114

D-SMS is a keypad and D-pad SMS/MMS app for flip phones, built on NovaLox v0.9.114 by theOnionsAreWatching. It was developed and tested on a TCL Flip 2 (Android 11, 240×320, no touch screen). This list covers everything D-SMS 0.9.126 adds or changes compared with upstream v0.9.114. Each line is one change.

House rules followed throughout: actions happen on key release (key-up), everything works with the D-pad and soft keys, pop-ups use the app's own themed style, and the system navigation bar is never hidden.

## Keypad, D-pad and soft keys

- Soft keys work without setup on phones that report the standard SOFT_LEFT / SOFT_RIGHT keys: on first start the app checks the phone's key layout, saves the keys, turns the soft-key bar on and skips the mapping screen.
- Soft-key labels are only redrawn when their text actually changes, so the screen no longer re-measures on every D-pad step.
- In a chat, LEFT/RIGHT, 2/8 and Page Up/Down move one screen up or down; holding keeps paging and older pages load as needed.
- In a chat, 1 jumps to the top of the whole conversation (loading the oldest page) and 7 jumps to the newest message.
- Chats open at the first unread message (with the "New messages" line above it); with nothing unread they open at the bottom.
- OK on a long message (500+ characters, or taller than the screen) opens a full-screen reader.
- The long-message reader scrolls by line with UP/DOWN, by page with LEFT/RIGHT or 4/6, jumps with 1/7, and closes with OK or Back.
- In the reader, a soft key or Menu opens the links-and-numbers menu (only when the message has links or numbers).
- Selection mode in a chat: the left soft key or Menu opens Forward (n) / Copy (n) / Delete (n) / Cancel selection.
- While scrolling messages the top bar and compose bar become compact; they return when focus goes back to the text box.
- On keypad phones the text box no longer opens the keyboard just from getting focus, and the half-typed word is ended when focus leaves the box.
- Leaving a screen hides the keyboard and its input-mode badge (for example "[ English ]"); the chat list clears a leftover badge.
- The chat top bar hides the three-dot icon whenever the soft-key bar is shown, and uses a small, clear back arrow.
- Every pop-up acts on key-up and starts with the cursor on OK or the main button, never on Cancel; lists start on the chosen row.
- A Settings "Key guide" row and the About window explain all keys; the reader and voice-note keys are included.
- BACK, OK and soft-key actions were moved to key-up in the new screens (pickers, reader, player, panels) so a held key never fires twice.

## Chat list (main screen)

- New one-line top bar: "D-SMS" centered, the number of chats on the left, and "All read" or an "N new" pill on the right (99+ above 99).
- Long counts in the top bar are cut with "…" so they never touch the title.
- Chat rows are rounded cards with a bold name, grey snippet, time, an accent unread-count badge and a thin unread stripe.
- The focused card gets a light accent tint and outline; the selection highlight is rounded to match.
- The card shadow was removed because redrawing it every scroll frame was slow on the phone's GPU.
- Unread chats stay on top (under pinned chats) until read, with a Settings switch "Unread chats on top" (default on).
- The list starts on the top row when the app opens and when returning from a chat or New message.
- Back from a chat keeps the cursor on that chat; the list only moves to the top if that chat moved to the top.
- A real new incoming message while away brings the list back to the top; read changes and sent messages do not.
- The list position survives the app being recreated by the system when nothing new arrived.
- Scrolling is steady: one scroll at a time, a short glide per D-pad step, at most 2 rows per key repeat, and extra rows laid out ahead.
- List refreshes use exact row updates (DiffUtil) with stable ids, so the highlight no longer jumps on each new message.
- Selecting a chat redraws only that row.
- Refreshes that arrive while one is running are merged (at most one runs and one waits).
- The chat list is read from the database once per refresh instead of twice.
- The "is this number blocked?" check is cached per address instead of asking the system on every row bind.
- Row shapes, avatar clip and size are set up once per row and only recolored on rebind.
- Options menu no longer has Sort/Filter, so it fits without scrolling.
- Chat options: the separate Call and the app-wide Customize entries were removed; saved contacts get "Call / View contact", unsaved numbers keep "Add contact".
- Group-name pop-ups use a short "Group name" hint with the full explanation as helper text.

## Threads, reading and performance

- Fixed: the newest message stayed "unread" after viewing the chat. Read marking now runs after the list is drawn and once more when leaving.
- When the newest message is on screen, the whole chat is marked read (same as "Mark as read" in the list), including the notification.
- Read marking only counts while the chat is really in front; returning from screen-off loads new messages first.
- The read check no longer loads the whole chat after every scroll; the heavy part runs once per visit and only when something new was marked.
- The read-report scan asks the database only for recent received MMS.
- Live updates in big chats reload only the newest 60 rows, with a safe full reload if anything does not match.
- Startup maintenance of old picture sizes runs in small ordered pages and skips bad files instead of loading everything at once.
- The import "is the store empty?" check uses a fast EXISTS query instead of counting all rows.
- Fixed duplicate messages (mostly in groups): MMS saving, SMS saving and sync passes can no longer run at the same time.
- A re-delivered copy (same Message-ID, or same sender, text and network time stamp) is skipped; duplicates already stored are hidden, not deleted.
- Real repeat texts are kept: "ok" sent twice by the same person now shows twice.
- A text sent to your own number shows once, and the sent bubble shows "Delivered".
- SMS counter counts real SMS parts: 160/153 for plain text, 70/67 when the text has emoji or Hebrew.
- Drafts keep their attachments, and Options starts with "Save draft and exit" and "Discard draft" when there is a draft.
- New message saves its draft when you leave (Home, a call, pickers) and restores it; send, schedule, share and Discard clear it.
- New message is one screen: a name/number box over a filtered contact list (names, numbers, T9); picking a contact opens the chat.
- New message soft keys: "Add more" (for a group) and "Start"; the soft-key bar is there from the first frame.
- Multi-select Copy puts the selected texts on the clipboard, oldest first.
- Forward sends the selected messages oldest to newest, with all attachments.
- Optional "Notes" chat (Settings > Messages > Notes chat, default off): saves notes only on the phone, never sends, never notifies.
- Notes can hold pictures, videos, voice notes, files and contacts, stored in their own folder and never sent as MMS.
- Notes are kept through backup, restore and re-import, and "Keep per chat" never trims them.

## MMS, pictures and video

- One "Photo/Video" item in Attach replaces the separate Photo, Video and From gallery items.
- New "Camera" item in Attach: choose Photo or Video, the phone's camera app opens, and the result is attached with the normal limits.
- The camera result still arrives if the system closes D-SMS while the camera is open.
- Attach limits checked when adding: at most 10 attachments and one video; audio, files and GIFs must fit the carrier size together.
- Items over the limit are skipped with a message ("N not added: too many or too big for MMS").
- Sending an MMS checks file sizes before reading anything, reads each file once, and shrinks pictures straight from the file.
- An MMS with an unreadable attachment stops, is marked Failed and says why ("Can't read <name>. Not sent.") instead of sending without it.
- Fixed "Unknown" or blank MMS: late-arriving text, parts and sender of an MMS are filled into the same message row.
- An MMS with no sender is kept under its own unresolved identity instead of all landing in one shared "Unknown" chat.
- An MMS with missing content shows a clear explanation instead of an empty bubble; a known failed MMS says it failed.
- MMS content types are normalized (case and parameters) so parts are recognized reliably.
- Contact cards in messages are read and their photo decoded in the background, then cached per attachment.
- OK on a message with a picture or video opens it full screen straight away.
- A message with several attachments, or long text plus attachments, shows a compact list with sharp thumbnails (and "Read text" first).
- Other parts stay reachable from the hold-OK menu under "Attachments (n)".
- Sharing into D-SMS from another app shows a full-screen "Send to" list: "New number…" first, then every chat as a card.
- Sharing several pictures or videos at once (SEND_MULTIPLE) accepts mixed photo + video, reads all items, and copies them off the main thread.
- Forward and share use the same full-screen chat picker; OK ticks several chats and "Send (n)" forwards to all of them.
- The chat picker shows all chats, not only the 15 newest.

## Pickers, viewer and files

- New in-app Photo/Video picker: a full-screen grid of the phone's media, newest first, with Photos and Videos tabs.
- Picker: OK ticks several items (numbered accent badges), videos show ▶ and their length, "Done (n)" adds them.
- Picker: press 0 to switch between Photos and Videos; UP from the top row reaches the tabs.
- Picker: thumbnails load lazily on background threads with cancellable requests, so thousands of items stay smooth.
- Picker sort order matches the stock gallery (date modified, then id).
- Picker opens straight away when the app already has all-files access (no repeated permission pop-up), with a Settings button if denied.
- Picker left soft key "Gallery" opens the phone's real gallery app directly (no chooser, never the file manager).
- Gallery requests ask for multi-select in all known formats and accept any result format (ClipData, data, stream list).
- Attach > File opens the phone's file manager directly (no app chooser); the in-app D-pad file browser is used if there is none.
- New in-app D-pad file browser for phones without a system file picker.
- Attach > Contact opens the phone's Contacts app; Attach > Audio opens the Music app; in-app lists are only a fallback.
- A short "D-SMS needs access…" explanation is shown before asking for media or contacts permission.
- Picture viewer fills the screen and stays sharp (decoded larger, drawn with proper filtering).
- Picture viewer: * zooms in, # zooms out, OK zooms 2×, the D-pad pans when zoomed and LEFT/RIGHT changes picture otherwise.
- Picture viewer bars float over the picture and hide by themselves; the first soft-key press just brings them back.
- Saving from D-SMS goes to a D-SMS folder with one folder per type: Pictures, Videos, Voice notes, Audio, Contacts, Files, Logs.
- With all-files access the D-SMS folder is at the top of storage, otherwise under Download/D-SMS; "Save to SD card" uses the same layout.
- The "Saved" message names the folder, and saving copies off the main thread so big videos don't freeze the app.

## Voice notes

- The screen stays on while recording.
- "Record audio" is in the first Attach pop-up; the system recorder stays available as "Audio".
- Voice notes get short names like Voice_1030-0915.m4a, never overwriting an existing file.
- Recording stops by itself at the carrier's MMS size limit ("Stopped: MMS size limit") and shows the time left while recording.
- .m4a voice notes are recorded at 16 kHz mono, 24 kbps (about 3× smaller than before).
- A failed recording start releases the microphone.
- Cancel or Back after recording deletes the file; it is kept only when you press Attach.
- Closing the flip, screen off, Home or a call stops the recording and keeps it so it can still be attached.
- New voice-note player: accent play circle, progress bar, elapsed/total time and a one-line title ("Voice 11-02 15:42").
- Player keys: OK = Play/Pause, left soft key = Earpiece on/off, right soft key = Options, LEFT/RIGHT = seek 5 seconds.
- "Voice notes on earpiece" setting, plus a switch in the player that keeps the place and keeps playing.
- Player Options "Play in stock player" opens the phone's default audio app directly.
- The player's Options pop-up lines up with the player, with a short fade and focus returning to the player.
- A note that can't be played closes the player with a themed message.
- Record pop-up: compact layout with a fixed one-line status and a narrow centered Record/Stop pill.

## Backup and restore

- Back up asks "SMS only / MMS only / Both" and saves straight to the D-SMS Backups folder.
- Backup export streams in pages of 500 rows to save memory.
- Clear error reasons: not enough space, can't read the file, can't write the file, out of memory, not a D-SMS backup, no messages.
- Backup format v3 (documented in docs/BACKUP-FORMAT.md); older app versions can still restore v3 files.
- Every backup also contains the app's settings, and restoring it puts them back.
- Fixed: two MMS sent in the same second are both restored.
- Restore looks in the usual folders first and lists what it finds, plus "Select manually".
- If nothing is found, restore offers to search the whole phone (asking for all-files access if needed), with a cancelable progress pop-up.
- The whole-phone search recognizes backups by name or by their contents, and skips system and hidden folders.
- "Select manually" uses the system picker, or the in-app D-pad file browser when there is none.
- Restore shows one continuous progress bar with real counts ("Message N of M", "Checking sent MMS", "Rebuilding conversations").
- Restore is faster: app-side writes run in one database transaction, SMS in batches of 500, chat lookups cached, no per-MMS recount.
- Backup and restore have "Run in background" and "Cancel"; a cancelled backup deletes its partial file.
- A cancelled restore stops safely and still rebuilds the chat list from what was written; settings are not applied.
- Progress pop-ups keep a fixed height: one-line details cut in the middle, so the title no longer jumps.
- Restoring settings applies theme and accent to every open screen at once.
- Note attachments are included in backups and restored without duplicates.
- Adds the all-files access permission (MANAGE_EXTERNAL_STORAGE) so backups from an earlier install can be found after a reset.

## Settings

- Settings has a new layout: Search, Notifications, Messages, Appearance, Keys, Blocked & hidden, Storage, Backup & restore, Advanced, About.
- Each section opens its own screen with a header and the soft-key bar (left = back, middle = select).
- New Settings search: type to find any setting and see its current value; Select jumps to it.
- Settings rows are compact and left-aligned, show their current value next to the explanation, and long titles wrap.
- Help text at the bottom of a screen can be reached with the keypad; OK opens the full text.
- Save settings / Restore settings to a file, keeping each value's type; one-install values (first-run flags, drafts, update state) are skipped.
- "Keep per chat" (All / 100 / 500 / 1000 / 2000 / 5000 / Other) replaces the auto-delete switch and number, reusing the same saved values.
- Short choice pop-ups (like Clear notification) wrap their content instead of filling the screen.
- Fixed: changing Vibrate now refreshes the notification channels (it listened to the wrong setting).
- UI scale and focus outline changes apply at once.
- Size choices are 80–120%; saved or restored values outside that range snap to the nearest step.
- Settings > About opens the About window (version, credits, license) and a separate Key guide row.
- Theme changes apply to the whole app at once (light/dark is set before each screen builds its colors).
- Themes: Follow system, Light, Dark, Graphite, Dusk, Linen (older theme names map to the closest new one).
- 14 more accent colors: Amber, Gold, Coral, Crimson, Rose, Violet, Indigo, Sky blue, Cyan, Mint, Lime, Olive, Brown, Slate.
- Theme and accent pickers show color dots, open on the current choice, and keep text readable on the focused row.
- 4 more message styles: Round (no tail), Outline, Full-width blocks, Lines (no bubbles).
- SIM messages screen (Settings > Storage): read messages stored on the SIM and delete one after a clear confirmation.
- Update section (ROM build only): Check for updates, Check for updates weekly, Delete update file after installing.

## Notifications and vibration

- Muted chats post to their own quiet channel: no sound, no vibration, no light, but still a silent notification.
- Muted chats are also silent and alert-once on Android versions before 8.
- One-notification mode: Mark read, Mark all read and Reply now remove the notification, or quietly update it when other chats are still unread.
- New vibration Pattern setting (Settings > Notifications): Default, Long, Two short, Three short, Heartbeat, Pulse, Gentle, Off.
- The Pattern picker plays a short preview buzz when you move to a choice or press OK.
- Each pattern uses its own notification channel; Default keeps the original channels, so nothing changes for existing users.
- The existing Vibrate setting (Always / Only on vibrate / Never) still decides whether it buzzes; the pattern is saved in settings backups.

## Reactions

- Reaction texts from iPhones and Google Messages ("Liked …", "Loved …", "Laughed at …", "Emphasized …", "Questioned …", "Disliked …") are recognized.
- Removals ("Removed a like from …"), "Reacted 😮 to …" and "👍 to …" forms are recognized too.
- Reactions show as a small badge (like 👍2 ❤️) overlapping the bottom edge of the original message, in one-to-one chats and group MMS.
- When a message has reactions, hold OK shows "View reactions" as the first option; it lists who reacted. It also appears in Details.
- Chat list previews and notifications read naturally, like "Alice 👍 your message".
- Settings > Messages > "Show reactions" (default on, saved in settings backups) turns it off and shows the raw texts again.
- The phone's SMS store is never changed; reactions are only a display layer.
- Reaction parsing and matching are ported from DPAD Messaging (MIT), with its copyright notice kept.

## Contacts and photos

- Saved contact photos are found with a fresh phone lookup and the current photo address, not an old cached one.
- Contacts changes are picked up quickly (debounced) and a failed read no longer erases cached names or photos.
- A changed photo with the same address still refreshes.
- Chats without a photo get a circle in a stable per-chat color with the contact's initials (Hebrew and Yiddish names supported).
- Unknown numbers show their last two digits; groups use their members' initials (or G).
- Contact photos are a round cover-crop in the same circle.
- Finished photo badges are kept in memory, so leaving a chat no longer flashes the placeholder before the photo.

## Emoji

- The emoji picker is full screen with a fixed bottom bar: Back, Select, and a bold "Done" pill.
- OK picks several emoji in a row; a "Picked: …" line shows them, and Done types them all at the cursor.
- Back in the grid returns to the categories and keeps your picks; DEL removes the last pick.
- New "Recent" category with the last 24 emoji used.
- The keyboard stays closed while the picker is open, so it no longer covers the bar.
- Bigger cells, a clear focus outline and a subtler picked tint; the last row can be scrolled fully into view.

## Pop-ups, toasts and look

- All pop-ups share one rounded, themed style: card with soft corners, bold title with a short accent line, accent focus pill, accent buttons.
- All AlertDialogs go through one builder, which styles the buttons, removes the grey focus box and sets the starting focus.
- Pop-ups are sized and focused before their first frame, so they no longer open too big and jump.
- Pop-up width fits the content (longest row + margin, at least 170dp, at most 85% of the screen).
- List pop-ups use compact, left-aligned rows with the title centered; the Attach pop-ups and emoji categories stay centered.
- Pop-up titles are a fixed header with a 6dp gap; lists are cut at their top edge so rows never slide under the title.
- One themed toast for the whole app (accent pill, readable text, RTL-aware) replaces every plain system toast while the app is open.
- Toasts and pop-ups take the current accent and light/dark theme when shown; text on the accent is black or white, whichever reads better.
- Hand-built pop-up rows (number lists, schedule picker, voice player and recorder) use the same accent focus pill.
- Pasted-text preview in the Attach pop-up is bigger (3 lines) with an accent border and a "▼ Will paste:" label.
- Settings preference pop-ups get the same button style and focus on OK.

## Other

- Clipboard guard: text pasted inside D-SMS is cleared from the clipboard after sending or leaving, only if it is still that same text.
- Read-marking, MMS content, contact photo, SIM reading and reaction rules live in small pure-Kotlin helpers that can be tested without a phone.
- Manifest: READ_MEDIA_* for Android 13+ pickers, and queries so Android 11 lets the app find the file manager, gallery, music and camera apps.
- Update check (ROM build only) points at the D-SMS fork's Releases and verifies the downloaded APK (same app, newer version, same signing key) before installing.
- Diagnostics log is saved to the D-SMS Logs folder.

## D-SMS build only (left out of the upstream PR)

- Two build variants from the same source: ROM ("D-SMS for PureFlip", version x.y.z-PF) and JTech (plain "D-SMS", version x.y.z).
- PureFlip launcher icon (green circle with a speech bubble).
- About shows the D-SMS name and version and keeps the NovaLox credit and license; the project-page button was removed.
- Update check is limited to the ROM build and uses the fork's Releases, because ROM builds are signed with the PureFlip key.
