# Audio Files

## Source formats

### Advanced Audio Encoding

#### MIME
```
audio/aac
audio/aacp
audio/3gpp
audio/3gpp2
audio/mp4
audio/mp4a-latm
audio/mpeg4-generic
```
#### PRONOM IDs
```
```
#### Extensions
```
.m4a
.mp4
.3gp
.m4b
.m4p
.m4r
.m4v
.aac
```

#### Apple Lossless Audio Encoding
#### MIME
`audio/m4a`
#### PRONOM IDs
```
```
#### Extensions
```
.m4a
.caf
```

#### Free Lossless Audio Encoding
#### MIME
`audio/flac`
#### PRONOM IDs
```
```
#### Extensions
`.flac`

#### MPEG-1 Audio Layer III
#### MIME
```
audio/mpeg
audio/MPA
audio/mpa-roubust
```
#### PRONOM IDs
```
```
#### Extensions
`.mp3`

#### Vorbis
#### MIME
```
application/ogg
audio/ogg
audio/vorbis
audio/vorbis-config
```
#### PRONOM IDs
```
```
#### Extensions
`.ogg`

#### Waveform Audio File Format
#### MIME
```
audio/vnd.wave
audio/wav
audio/wave
audio/x-wav
```
#### PRONOM IDs
```
```
#### Extensions
```
.wav
.wave
```

#### Preservtion Format
WAVE/Broadcast WAV/FLAC?
#### Access Format
MP3 UNHCR to choose bit rate / quality?
### Properties
| Property             | Measured With | Example |
| -------------------- | ------------- | ------- |
| Duration | mediainfo | Duration : 2mn 9s |
| BitRateMode | mediainfo | Bit rate mode : Variable |
| BitRate | mediainfo |  Bit rate : 2 469 Kbps |
| Channels | mediainfo | Channel(s) : 2 channels |
| Channel Positions | mediainfo | Channel positions : Front: L R  |
| Sample Rate | mediainfo |  Sampling Rate : 96.0KHz |
| Bit Depth | mediainfo | Bit Depth : 24 bits  |

### Workflow

#### Characterisation
[MediaInfo](https://mediaarea.net/en/MediaInfo)
[ffprobe](https://www.ffmpeg.org/ffprobe.html)
```
mediainfo <path to file>

Audio
Format                                   : FLAC
Format/Info                              : Free Lossless Audio Codec
Duration                                 : 2mn 9s
Bit rate mode                            : Variable
Bit rate                                 : 2 469 Kbps
Channel(s)                               : 2 channels
Channel positions                        : Front: L R
Sampling rate                            : 96.0 KHz
Bit depth                                : 24 bits
Stream size                              : 38.2 MiB (100%)
```

#### Normalisation
Notes: Use `lame` encoder for best compression & audio quality, may require LAME MP3 library installation.
TODO: Choose audio quality option, `-aq 0` currently gives variable bit rate MP3s at 320Kbps, `-aq 2` 192Kbps might be appropriate

```
ffmpeg -i "<filename>.flac" -acodec libmp3lame -aq 0 "<filename>.mp3"
Input #0, flac, from '07 - E questa vita un lampo.flac': [413/699]
  Metadata:
    ARTIST          : Elin Manahan Thomas, Grace Davidson, Jeremy Budd, Mark Dobell, Stuart Young, The Sixteen, Harry Christophers
    ALBUM           : Monteverdi: Selva morale e spirituale Volume III
    TITLE           : È questa vita un lampo
    track           : 07
    GENRE           : Classical
    DATE            : 2013
    COMMENT         : Copyright 2013 The Sixteen Productions Ltd
    album_artist    : Grace Davidson, Elin Manahan Thomas, Jeremy Budd, Mark Dobell, Simon Berridge, Julian Stocker, Rob Macdonald, Stuart Young, The Sixteen Harry Christophers
    COMPOSER        : Monteverdi (Claudio)
    disc            : 01
    ISRC            : GBEJP1310907
    BARCODE         : 828021610929
    CATALOGNUMBER   : COR16109
    DISCTOTAL       : 01
    TRACKTOTAL      : 12
  Duration: 00:02:09.81, start: 0.000000, bitrate: 2478 kb/s
    Stream #0:0: Audio: flac, 96000 Hz, stereo, s32 (24 bit)
    Stream #0:1: Video: mjpeg, yuvj444p(pc, bt470bg/unknown/unknown), 600x595 [SAR 300:300 DAR 120:119], 90k tbr, 90k tbn, 90k tbc
    Metadata:
      comment         : Other
Stream mapping:
  Stream #0:1 -> #0:0 (mjpeg (native) -> png (native))
  Stream #0:0 -> #0:1 (flac (native) -> mp3 (libmp3lame))
```
