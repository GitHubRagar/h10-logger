# H10 Logger

Records a Polar H10 in Chrome on Android: ECG (130 Hz), accelerometer (25–200 Hz), RR intervals and HR.
No install and no treadmill. It's a web page (Web Bluetooth) that can be added to the home screen.

## Use
1. Open the page in Chrome. Set the ACC rate (200 Hz for the meditation sessions).
2. Close Polar Beat, Polar Flow, Polar Sensor Logger and any watch paired to the strap. The H10 streams ECG/ACC to one app only.
3. **Connect H10 and record**. Within ~8 s all three dots should be green (RR, ECG, ACC), and the rates should read ~130 Hz and ~200 Hz.
4. Keep the screen on and Chrome in front. The page holds a wake lock. Android may pause Bluetooth for a background tab.
5. **Stop recording**, then **Share** (to Google Drive) or **Save** (to Downloads).

## What it does that Polar Sensor Logger did not
- Red banner, vibration and beep when a stream stops or never starts. It then restarts the stream (stop and start, up to 3 tries every 10 s) and logs the strap's own error code ("already in state", "invalid state", ...).
- It reads which streams the strap offers when it connects. "ECG NO / ACC NO" means another app holds the strap.
- It reconnects by itself after a dropout and logs the gap.
- Every sample is written to the phone's storage every 4 s. A crash or reload keeps the data, and the recording appears under "Saved recordings".
- An `H10LOG_<time>.txt` file lists every event (starts, errors, restarts, disconnects) for the session.

## Files
Same names and columns as Polar Sensor Logger, so the analysis scripts read them unchanged:
`Polar_H10_<id>_<YYYYMMDD_HHMMSS>_ECG.txt`, `_ACC.txt`, `_RR.txt`, `_HR.txt`, `MARKER_<time>.txt`.
Sensor timestamps are the strap's own clock (ns since 2000-01-01). The frame timestamp belongs to the frame's last sample, and earlier samples are spaced by the interval between frames, so the true ~130.24 Hz is kept.
RR values keep the strap's 1/1024 s resolution (4 decimals in ms). The phone timestamp of the last RR in a packet is its arrival time.

## Hosting
GitHub Pages: repository Settings → Pages → Deploy from branch `main`, root. Then open `https://<user>.github.io/h10-logger/`.
Web Bluetooth needs https, which Pages provides.
