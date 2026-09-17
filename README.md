# realtime wav2lip studio
> **python 3.10+ | tested on linux fedora (gnome)**

![realtime wav2lip studio full ui](put-your-screenshot-link-here.png)

this fork adds a fully asynchronous audio pipeline and built-in gfpgan face restoration, meaning you no longer need virtual audio cables to prevent crashes or separate offline tools to fix blurry mouth outputs.

feel free to fork this if you need further post-processing effects.

**note:** this built-in pipeline is designed for real-time streaming and local inference. for offline video dubbing, professional batch processors will still offer higher control.

### my custom contributions
*   **async audio (sounddevice):** replaced blocking pyaudio loops with an asynchronous queue to completely decouple mic capture from ai inference.
*   **universal mic support (librosa):** added a real-time 48khz to 16khz resampler. prevents portaudio `-9997` crashes on bluetooth headsets and virtual cables.
*   **gfpgan integration:** added an optional face restoration pass that intercepts the 96x96 wav2lip output and upscales the mouth/teeth.
*   **video uploads:** updated the flask backend and ui to support `.mp4` uploads, looping the frames continuously instead of freezing.
*   **noise gate ui:** wired an audio threshold slider into the frontend to prevent the model from lip-syncing to background static.
*   **50 fps generation:** updated default generation for static images from 25 to 50 fps.

---

## 🚀 quick start & installation
the installation process remains largely the same as the original architecture, with a few extra dependencies.

1. clone this repository to your machine.
2. ensure you have python 3.10+ installed.
3. create and activate a virtual environment.
4. install the required dependencies (`pip install -r requirements.txt`).
5. **(optional)** download `GFPGANv1.4.pth` and place it in the `checkpoints/` directory for face restoration.
6. start the server by running `python app.py`.
7. open `http://localhost:8080` in your browser.

---

## the tuning guide (pipeline parameters)
these settings affect how the model interprets your audio and video input.

*   **noise threshold:** sets the volume gate.
    *   *raise it (e.g., 500+):* if you have loud fans or keyboard clicks causing the mouth to twitch when you aren't speaking.
    *   *lower it (e.g., 100):* if the ends of your words are being cut off prematurely.
*   **fps (static images):** sets the mel-spectrogram step size for image generation. `50 fps` provides smoother output but requires more gpu overhead than `25 fps`.
*   **gfpgan restoration:** sharpens the mouth. disable this if you are dropping frames, as it adds roughly 20ms of overhead per frame.

---

## the "clean stream" preset
if you are streaming to obs and want a sharp, non-twitchy output, use this setup:

*   **input media:** static image (requires less overhead than looping an mp4)
*   **fps:** 50
*   **noise threshold:** 400
*   **gfpgan:** enabled
*   **sync offset (in obs):** set mic delay to ~100ms to match the inference buffer.

---

**credits:** thanks to [devkrish23](https://github.com/devkrish23/realtimeWav2lip) for the original flask architecture. wav2lip model by [rudrabha](https://github.com/Rudrabha/Wav2Lip) and gfpgan by [tencentarc](https://github.com/TencentARC/GFPGAN).
