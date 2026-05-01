# Project Notes

This project is used to prepare MP3 audio files for simple, low-cost MP3 players.

## Directory Structure

- `audio/`: source audio files to be converted. This directory may contain files with an `.mp3` extension that still need compatibility checks or re-encoding.
- `fixed/`: converted MP3 files ready to copy to a player. Files here should use compatibility-focused settings such as MP3 audio, 128 kbps CBR, 44.1 kHz sample rate, audio-only streams, and no album artwork or album metadata.
- `plan/`: written notes and repeatable plans for the conversion workflow.
- `.codex/`: project instructions and working notes for Codex.

## Requirements

- Install `ffmpeg` before running conversion or verification commands.
- The workflow uses `ffmpeg` for audio conversion and `ffprobe`, which is included with ffmpeg, for file inspection and verification.

## Handling Notes

- Do not record individual song names or source file names in project instructions.
- Prefer documenting directory purposes and repeatable conversion requirements instead of listing personal media contents.
