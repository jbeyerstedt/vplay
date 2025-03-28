# C3VOC A/V Technician Helper
Python script to make the life an A/V technician at c3voc easier.
It can play:

- individual outputs of voctomix
- RTMP streams (with custom audio track mapping)
- SRT streams (with multiple audio tracks)
- the final transcoded streams from the VOC CDN/ streaming website.

Keep in mind, that this tool uses assumptions about the VOC network and might not simply work in other situations.


## Requirements
Python 3, ffmpeg (with ffplay) and mpv.


## Usage
There are three basic commands with the hall ID as common parameter:
- `./vplay -s $hall [-a $num_audio_tracks] $command` with commands:
    - `cam $cam_no`: Play a camera preview from voctomix
        * Positional argument: Camera Number
    - `mix [-l [-t $track_no] ]`: Play the voctomix mix preview
        * Optionally enable a loudness graph (`-l`) and select which audio track (`-t track_num`) to listen to
    - `stream {hls-hd, hls-sd, webm-hd, webm-sd, slides} [-l [-t $track_no] ]`: Play the transcoded stream from the CDN
        * Positional argument: Transcoding type (hls, webm, slides)
        * The hall ID (`-s`) can also be an arbitrary slug in this command
        * Optionally enable a loudness graph (`-l`) and select which audio track (`-t track_num`) to listen to
    - `rtmp [-l [-t $track_no] ]`: Play RTMP stream (same audio channel config as mix)
        * The hall ID (`-s`) must be the domain and path
        * Optionally enable a loudness graph (`-l`) and select which audio track (`-t track_num`) to listen to
    - `srt [-l [-t $track_no] ]`: Play SRT stream
        * The hall ID (`-s`) must be the domain and path
        * Optionally enable a loudness graph (`-l`) and select which audio track (`-t track_num`) to listen to

Python's help output is split.
`./vplay -h` shows the available commands and `./vplay $command -h` shows the help for the individual command.

The player volume can be changed with the "9" and "0" keys in both ffplay (normal output) and mpv (loudness monitor output).

## Examples
Play SRT stream with 3 audio tracks, show EBUR graphs and listen to native audio:
```
vplay -a 3 -s "ingest.c3voc.de:1337?streamid=play/teststream" srt -l -t 0
```

Play transcoded stream with 3 audio tracks, show EBUR graphs and listen to translation 1:
```
vplay -a 3 -s teststream stream hls-hd -l -t 1
```

Play RTMP stream with simple stereo audio channel:
```
vplay -s ingest2.c3voc.de/relay/test rtmp
```
