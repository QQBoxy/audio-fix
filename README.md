# audio-fix

This project prepares MP3 audio files for simple, low-cost MP3 players that may have limited format compatibility.

The workflow keeps original source audio in `audio/`, writes converted player-ready files to `fixed/`, and stores repeatable conversion notes in `plan/`.

Converted files should favor broad MP3-player compatibility:

- MP3 audio
- 128 kbps constant bitrate
- 44.1 kHz sample rate
- audio-only streams
- no album artwork or album metadata

Audio files are intentionally excluded from version control. The repository stores the workflow, notes, and project instructions rather than personal media contents.

## Requirements

Install `ffmpeg` before running conversion or verification commands. The included `ffprobe` tool can be used to inspect output files.
