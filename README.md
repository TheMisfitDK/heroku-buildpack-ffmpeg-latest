# Heroku FFmpeg Buildpack

A lightweight custom Heroku buildpack that installs FFmpeg automatically during deployment.

This buildpack downloads a prebuilt FFmpeg package and exposes both `ffmpeg` and `ffprobe` globally through the PATH environment variable.

## Features

- Automatic FFmpeg installation
- Automatic PATH configuration
- Supports:
  - H.264 (libx264)
  - H.265 / HEVC (libx265)
  - AV1 (libsvtav1, libaom)
  - Opus
  - VP9
  - ASS/SSA subtitles
  - Drawtext overlays
- Works with Heroku Dynos
- Ideal for Telegram encoding bots
- No additional configuration required

## Installation

Add the buildpack at index 1:

```bash
heroku buildpacks:add --index 1 https://github.com/TheMisfitDK/heroku-buildpack-ffmpeg-latest
```

Verify:

```bash
heroku buildpacks
```

Expected output:

```text
=== Buildpack URLs

1. https://github.com/TheMisfitDK/heroku-buildpack-ffmpeg-latest
2. heroku/nodejs
```

Deploy your application:

```bash
git push heroku main
```

## Installed Location

FFmpeg is installed into:

```text
/app/vendor/ffmpeg
```

Executables:

```text
/app/vendor/ffmpeg/bin/ffmpeg
/app/vendor/ffmpeg/bin/ffprobe
```

Since the buildpack adds FFmpeg to PATH, you can simply use:

```bash
ffmpeg
```

or

```bash
ffprobe
```

from your application.

## Verification

Open a Heroku shell:

```bash
heroku run bash
```

Check FFmpeg:

```bash
ffmpeg -version
```

Check path:

```bash
which ffmpeg
```

Expected output:

```text
/app/vendor/ffmpeg/bin/ffmpeg
```

## Example Usage

### Basic Conversion

```bash
ffmpeg -i input.mp4 output.mkv
```

### H.265 Main10 Encode

```bash
ffmpeg -i input.mkv \
-c:v libx265 \
-preset veryfast \
-crf 27 \
-pix_fmt yuv420p10le \
-profile:v main10 \
-c:a libopus \
-b:a 64k \
output.mkv
```

### AV1 Encode

```bash
ffmpeg -i input.mkv \
-c:v libsvtav1 \
-preset 10 \
-crf 38 \
-c:a libopus \
output.mkv
```

## Project Structure

```text
.
├── bin
│   ├── detect
│   ├── compile
│   └── release
├── README.md
```

## How It Works

During deployment:

1. Heroku detects the buildpack.
2. FFmpeg binaries are downloaded.
3. Files are extracted into `/app/vendor/ffmpeg`.
4. PATH is updated automatically.
5. Application deployment continues normally.

## Supported Applications

- Telegram Bots
- Discord Bots
- Media Processing Services
- Video Compression Pipelines
- Audio Processing Tools
- Subtitle Burn-in Workflows

## Requirements

- Heroku Application
- Heroku Buildpacks enabled
- Internet access during build

## License

This repository only automates installation of FFmpeg.

FFmpeg and all bundled codec libraries remain subject to their respective licenses.

## Current FFmpeg version:
N-125157-gefa8b20987-20260622

## Source:
BtbN FFmpeg Builds
