# Wav2Lip Studio (Real-Time HD) 🎙️👄

A completely overhauled, modern, and production-ready real-time lip-syncing application based on Wav2Lip. 

This repository heavily upgrades the original real-time Wav2Lip architecture to deliver **zero-latency audio**, **HD face restoration**, **universal microphone support**, and a **modern web UI**.

---

## 🌟 What's New in This Version?

1. **Async Audio Pipeline (`sounddevice`)**: Replaced the legacy blocking PyAudio logic with a fully asynchronous background queue. This completely decouples microphone capture from the AI inference thread, resulting in zero audio dropouts.
2. **Universal Microphone Support**: Fixed the notorious PortAudio `-9997` error (which crashed on Bluetooth headsets and virtual cables). Added a real-time `librosa` resampler to capture audio at a universally supported 48kHz and instantly downsample it to 16kHz for Wav2Lip.
3. **HD Face Restoration (GFPGAN)**: Added an optional **GFPGAN v1.4** post-processing pass. It intercepts the famously blurry 96x96 Wav2Lip output and surgically enhances the mouth and teeth back to High Definition in real time.
4. **Video Upload Support**: The Flask app now natively supports uploading MP4/video files! Instead of freezing on the last frame, it seamlessly loops the video seamlessly in the background.
5. **Noise Gate Controls**: Added an adjustable threshold slider to the Web UI to prevent the model from lip-syncing to background static/fan noise.
6. **50 FPS Output**: Upgraded the default generation speed for static images to 50 FPS for buttery smooth outputs.
7. **Modern Web UI**: Completely redesigned the frontend with a dark cinematic layout and better responsiveness.

## 🛠️ Installation

**1. Clone the repo:**
```bash
git clone https://github.com/danMoksh/realtimeWav2lip.git
cd realtimeWav2lip
```

**2. Set up your Virtual Environment:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**3. Download the GFPGAN Checkpoint (Optional but highly recommended for HD):**
Download `GFPGANv1.4.pth` and place it in the `checkpoints/` folder.
```bash
wget https://github.com/TencentARC/GFPGAN/releases/download/v1.3.0/GFPGANv1.4.pth -O checkpoints/GFPGANv1.4.pth
```

*(Note: The `wav2lip_gan.pth` model must also be placed in the `Wav2Lip/checkpoints/` folder as usual).*

## 🚀 Running the App

```bash
python app.py
```
1. Open your browser to `http://localhost:8080`.
2. Upload a face (Image or MP4 Video).
3. Set your noise threshold.
4. Click **Start** and begin speaking into your microphone!

## 🙏 Credits

Massive shoutout to [devkrish23](https://github.com/devkrish23/realtimeWav2lip) for the original Flask and OpenVINO implementation that served as the foundation for this repository. 

The original Wav2Lip model was created by the researchers at [Rudrabha/Wav2Lip](https://github.com/Rudrabha/Wav2Lip). GFPGAN was created by [TencentARC/GFPGAN](https://github.com/TencentARC/GFPGAN).
