Heroku FFmpeg Buildpack

A lightweight custom Heroku buildpack that downloads and installs a prebuilt FFmpeg binary during deployment.

This buildpack installs FFmpeg into:

/app/vendor/ffmpeg

and automatically adds it to the application PATH.

Features

- Automatic FFmpeg installation during build
- Uses prebuilt FFmpeg binaries
- Supports:
  - H.264 (libx264)
  - H.265 / HEVC (libx265)
  - AV1 (libsvtav1, libaom)
  - Opus
  - VP9
  - ASS subtitles
  - Drawtext overlays
- Compatible with Heroku Dynos
- Suitable for Telegram encoding bots and media processing applications

Directory Structure

.
├── bin
│   ├── detect
│   ├── compile
│   └── release
└── README.md

Installation

Add the buildpack to your Heroku application:

heroku buildpacks:add https://github.com/USERNAME/heroku-buildpack-ffmpeg

Or set it as the primary buildpack:

heroku buildpacks:set https://github.com/USERNAME/heroku-buildpack-ffmpeg

Deployment

Push your application normally:

git push heroku main

During the build process the buildpack will:

1. Download FFmpeg
2. Extract files into:

/app/vendor/ffmpeg

3. Expose FFmpeg through PATH

Verification

Open a Heroku shell:

heroku run bash

Check FFmpeg:

ffmpeg -version

Or:

which ffmpeg

Expected output:

/app/vendor/ffmpeg/bin/ffmpeg

Usage

Basic

ffmpeg -i input.mp4 output.mkv

H.265 Main10

ffmpeg -i input.mkv \
-c:v libx265 \
-pix_fmt yuv420p10le \
-profile:v main10 \
-crf 27 \
-preset veryfast \
output.mkv

AV1

ffmpeg -i input.mkv \
-c:v libsvtav1 \
-crf 38 \
-preset 10 \
output.mkv

Tested On

- Heroku Eco Dynos
- Heroku Basic Dynos
- Heroku Standard Dynos

License

This buildpack only automates installation of FFmpeg binaries.

FFmpeg and all included codec libraries remain subject to their respective licenses.
