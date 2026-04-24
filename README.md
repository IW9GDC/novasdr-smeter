# NovaSDR S-Meter Mod

Adds a segmented, color-coded **S-Meter** to the [NovaSDR](https://github.com/Steven9101/NovaSDR) web interface.

The S-Meter appears inside the **Audio panel** and shows real-time signal strength, both as an S-unit label (S1 – S9+40) and as a dBFS bar with 13 color-coded segments.

![S-Meter preview](preview.png)

---

## What it does

The installer replaces `frontend/src/components/receiver/panels/AudioPanel.tsx` with a modified version that adds a `<SMeter>` React component. The component:

- Reads the `pwrDb` value already exposed by `useAudioClient` (no backend changes needed)
- Maps dBFS → S-units using standard amateur-radio thresholds (S1 = −114 dBm … S9 = −66 dBm, then +10/+20/+30/+40 dB over S9)
- Renders 13 segments with smooth partial-fill animation (80 ms)
- Color scale: green (S1–S6) → yellow (S7–S8) → orange (S9) → red (S9+10 and above)
- Displays the numeric dBFS value alongside the S-unit label

The original file is backed up as `AudioPanel.tsx.bak` before any changes are made.

---

## Requirements

- A working **NovaSDR** installation with its frontend source tree present
- **Node.js** and **npm** (to rebuild the frontend after patching)
- Bash

---

## Installation

### 1. Clone or download this repository

```bash
git clone https://github.com/IW9GDC/novasdr-smeter.git
cd novasdr-smeter
```

Or just download `install_smeter.sh` directly:

```bash
curl -O https://raw.githubusercontent.com/IW9GDC/novasdr-smeter/main/install_smeter.sh
```

### 2. Run the installer

Pass the path to your NovaSDR root directory as the first argument:

```bash
bash install_smeter.sh /path/to/NovaSDR
```

Example (NovaSDR cloned in your home directory):

```bash
bash install_smeter.sh ~/NovaSDR
```

The script will:
1. Check that `AudioPanel.tsx` exists at the expected path
2. Save a backup (`AudioPanel.tsx.bak`)
3. Write the patched file

### 3. Rebuild the frontend

```bash
cd /path/to/NovaSDR/frontend
npm run build
```

### 4. Restart the NovaSDR server

```bash
sudo systemctl restart novasdr-server
# or however you normally restart it
```

---

## Reverting

The original file is saved automatically. To restore it:

```bash
cp /path/to/NovaSDR/frontend/src/components/receiver/panels/AudioPanel.tsx.bak \
   /path/to/NovaSDR/frontend/src/components/receiver/panels/AudioPanel.tsx

cd /path/to/NovaSDR/frontend && npm run build
sudo systemctl restart novasdr-server
```

---

## Compatibility

Tested against **NovaSDR** as of April 2026.  
If a NovaSDR update changes `AudioPanel.tsx` significantly, the patched file may be out of date — restore the backup and wait for an updated installer.

---

## Author

**Emanuele — IW9GDC**  
WebSDR: [websdr.iw9gdc.it](https://websdr.iw9gdc.it)
