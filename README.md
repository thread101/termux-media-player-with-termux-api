# 🎵 Termux Media Player

A lightweight, interactive command-line music player for Android via [Termux](https://termux.dev). Browse and play MP3s from external storage using a native spinner dialog, with persistent playback controls in your notification shade.

---

## ✨ Features

- 📂 Recursively scans your SD card for `.mp3` files
- 🎛️ Native Android spinner dialog for track selection
- 🔔 Persistent notification with **pause / play / select** controls
- 🍞 Toast feedback for playback status
- 🎨 Coloured terminal output

---

## 📋 Requirements

### Apps

| App | Source | Purpose |
|-----|--------|---------|
| [Termux](https://f-droid.org/packages/com.termux/) | F-Droid | Terminal emulator |
| [Termux:API](https://f-droid.org/packages/com.termux.api/) | F-Droid | Android API bridge (notifications, dialogs, media) |

> ⚠️ **Install both from F-Droid.** The Play Store versions are outdated and may not work correctly together.

---

### Packages

Install the required packages inside Termux:

```bash
# Termux API package — exposes termux-* commands
pkg install termux-api

# jq — JSON processor for parsing dialog output
pkg install jq
```

---

## ⚙️ Configuration

Open the script and update `MPATH` to match your SD card mount point:

```bash
MPATH="/storage/B057-0E1D"
```

To find your SD card path, run:

```bash
ls /storage/
```

Use the alphanumeric folder that isn't `emulated` — that's your SD card.

---

## 🚀 Usage

Make the script executable, then run it:

```bash
chmod +x player.sh
bash player.sh
```

On launch, the script will:

1. Scan `$MPATH` recursively for `.mp3` files
2. Present a native Android spinner to choose a track
3. Begin playback immediately
4. Post a notification with media controls

---

## 🔔 Notification Controls

Once a track is playing, a persistent notification appears with three action buttons:

| Button | Action |
|--------|--------|
| **pause** | Pauses playback and shows current track info |
| **play** | Resumes playback and shows current track info |
| **select** | Reopens the track selector to choose a new song |

Dismissing the notification stops playback automatically.

---

## 📁 Project Structure

```
player.sh          # Main script
```

---

## 🔧 How It Works

```
player.sh
├── get-stream()     Scans MPATH recursively for .mp3 files via termux-media-scan
├── set-stream()     Builds a comma-separated track list (strips base path)
├── main()           Shows spinner dialog → resolves index → starts playback
└── notification()   Posts an interactive Android notification with media buttons
```

---

## 🐛 Troubleshooting

**No tracks found**
- Confirm `MPATH` points to the correct SD card path.
- Run `termux-media-scan -r -v $MPATH` manually to check output.

**Spinner dialog doesn't appear**
- Ensure the **Termux:API** app is installed (not just the `termux-api` package).
- Grant any required Android permissions to Termux:API.

**`jq` command not found**
```bash
pkg install jq
```

**`termux-media-player` command not found**
```bash
pkg install termux-api
```

**Permission denied on SD card**
- Go to Android Settings → Apps → Termux → Permissions and enable **Storage**.
- Or run `termux-setup-storage` inside Termux.

---

## 📄 License

MIT — free to use, modify, and distribute.
