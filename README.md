# EntropyForge

A single-file, fully client-side generator for **passwords** and **usernames**. No build step, no dependencies, no server. Open `entropy_forge.html` in a browser and it works.

It merges two earlier tools, EntropyBits (password generator) and IDENTITY_FORGE (username generator), into one interface with a third tab that produces a matched pair.

## Quick start

1. Download `entropy_forge.html`.
2. Open it in any modern browser (double-click is fine).
3. For passwords, use `https://` or `localhost` if you host it. Web Crypto is only available in secure contexts, and password generation is disabled without it.

The page loads two fonts from Google Fonts. Offline, it falls back to system fonts and everything still works.

## Tabs

### Password
- Character sets: uppercase, lowercase, numbers, symbols, plus an option to exclude ambiguous characters (`0 O 1 l I |`).
- Structure: segment length (3–30) and segment count (1–8), joined with dashes.
- Guarantees at least one character from every enabled set (when the length allows it).
- Live entropy in bits, a rating (WEAK, MODERATE, EXCELLENT, OVERPOWERED), and an estimated offline crack time (assumes 10 billion guesses per second).
- Password is masked by default; the eye button reveals it.
- **Wipe** (or press `Esc`) clears the password, the display, and the clipboard. Auto-wipe fires after 90 seconds.
- Copy clears the clipboard after 30 seconds if it still holds the password (best effort, browser permitting).

### Username
- **Name mode** builds from a base name you type. **Word mode** combines random cyberpunk words (for example `NeonWraith`).
- Options: prefix, suffix, custom numbers or random digits (2–6), case style, 1337sp34k, and append mode.
- Editable pattern templates using `{prefix}`, `{name}`, `{suffix}`, `{numbers}`, one per line.
- Per-result actions: favorite, reroll, copy, and GH / X / R buttons that open that profile URL in a new tab so you can check availability by hand.
- Favorites, pattern templates, and recent-history dedupe are saved in `localStorage`. Copy all and export to `.txt` are included.

### Identity Kit
One click generates a username and a password together, using the current settings from the other two tabs. The password is masked until you click it. **Copy both** puts the username and password on two lines.

## Privacy and security notes

- Everything runs locally in your browser. Nothing is sent anywhere; the GH / X / R buttons only open a new tab.
- Passwords use `crypto.getRandomValues` with rejection sampling to avoid modulo bias. There is no `Math.random()` fallback for passwords.
- Usernames use Web Crypto too, and only fall back to `Math.random()` if it is unavailable. Usernames are not secrets.
- Passwords are never written to `localStorage`. Only usernames, favorites, and patterns are stored.
- Clipboard clearing is best effort. Clipboard managers or history tools may still keep a copy, so consider disabling clipboard history for sensitive work.
- The crack-time figure is an estimate under one attacker model, not a guarantee.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `Esc` | Wipe the current password |
| `Enter` (in a username field) | Generate 10 usernames |

## Customizing

- Colors and fonts live in the CSS variables at the top of the `<style>` block (`--mag`, `--cy`, `--vio`, `--ui`, `--mono`).
- Word lists are the `ADJ` and `NOUN` arrays in the script.
- Default patterns are `DEF_PAT`. Auto-wipe and clipboard timers are `WIPE_S` and `CLIP_S`.

## Storage keys

`ug_used_v1`, `ug_favorites_v1`, `ug_patterns_v1`. These match the original IDENTITY_FORGE tool, so existing favorites carry over when opened from the same origin.
