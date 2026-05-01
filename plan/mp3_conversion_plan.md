# MP3 Conversion Plan

Created: 2026-05-01 13:01:25 CST

## Goal

Convert every MP3 file under `audio` into a low-cost MP3-player-friendly file under `fixed`.

The converted files should:

- Use MP3 audio encoded by `libmp3lame`.
- Use constant bitrate, 128 kbps CBR.
- Use 44.1 kHz sample rate.
- Contain audio only, with no embedded album artwork.
- Remove album/title/artist/genre/date metadata and chapters.
- Keep the original filename.

## Conversion Command

Run from the project root:

```bash
mkdir -p fixed
find audio -type f -iname "*.mp3" -print0 | while IFS= read -r -d "" src; do
  base=$(basename "$src")
  out="fixed/$base"
  ffmpeg -nostdin -hide_banner -loglevel error -y -i "$src" \
    -map 0:a:0 -vn -map_metadata -1 -map_chapters -1 \
    -c:a libmp3lame -b:a 128k -ar 44100 -write_xing 0 \
    "$out"
  printf "converted %s -> %s\n" "$src" "$out"
done
```

## ffmpeg Options

- `-nostdin`: prevents `ffmpeg` from consuming `find -print0` input while running inside the loop.
- `-map 0:a:0`: keeps only the first audio stream.
- `-vn`: drops video/image streams, including embedded album artwork.
- `-map_metadata -1`: removes metadata from the output.
- `-map_chapters -1`: removes chapters.
- `-c:a libmp3lame`: encodes MP3 using LAME.
- `-b:a 128k`: sets the audio bitrate to 128 kbps. With `libmp3lame`, this produces CBR unless VBR/quality mode is requested.
- `-ar 44100`: resamples audio to 44.1 kHz.
- `-write_xing 0`: disables Xing/Info header writing for better compatibility with simple MP3 players.

## Verification Command

Run from the project root after conversion:

```bash
for f in fixed/*.mp3; do
  codec=$(ffprobe -v error -select_streams a:0 -show_entries stream=codec_name -of default=noprint_wrappers=1:nokey=1 "$f")
  sample_rate=$(ffprobe -v error -select_streams a:0 -show_entries stream=sample_rate -of default=noprint_wrappers=1:nokey=1 "$f")
  bitrate=$(ffprobe -v error -select_streams a:0 -show_entries stream=bit_rate -of default=noprint_wrappers=1:nokey=1 "$f")
  streams=$(ffprobe -v error -show_entries stream=codec_type -of csv=p=0 "$f" | paste -sd, -)
  tags=$(ffprobe -v error -show_entries format_tags=album,artist,title,genre,date -of default=noprint_wrappers=1 "$f")
  printf "%s | codec=%s | sample_rate=%s | stream_bitrate=%s | streams=%s | tags=%s\n" \
    "$f" "$codec" "$sample_rate" "$bitrate" "$streams" "${tags:-none}"
done
```

Expected output for each file:

- `codec=mp3`
- `sample_rate=44100`
- `stream_bitrate=128000`
- `streams=audio`
- `tags=none`

## Current Result

The current files in `fixed` were converted with this plan and verified as:

- MP3 codec.
- 44.1 kHz sample rate.
- 128000 bps audio stream bitrate.
- Audio-only stream list.
- No common album/title/artist/genre/date metadata.
