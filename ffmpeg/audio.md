# FFMPEG commands


## Silence removal of audio files

This command removes all periods of silence that are at least 1 second long and quieter than -40 dB

```bash
ffmpeg -i input.m4a -af silenceremove=stop_periods=-1:stop_duration=1:stop_threshold=-40dB output.mp3
```
