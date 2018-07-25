# Bitmap Images

## Source formats

| Format | PUIDS | Ext | MIME |
|--------|-------|-----|------|
| Windows Bitmap | `fmt/114  1.0`<br/>`fmt/115  2.0`<br/>`fmt/116  3.0`<br/>`fmt/117  3.0 NT`<br/>`fmt/118  4.0`<br/>`fmt/119  5.0` | `.bmp` | `image/bmp` |
| Graphics Interchange Format | `fmt/3  87a`<br/>`fmt/4  89a` | `.gif` | `image/gif` |
| JPEG | `fmt/42  1.00`<br/>`fmt/43  1.01`<br/>`fmt/44  1.02` | `.jpg`<br/>`.jpeg`<br/>`.jpe` | `image/jpeg` |
| Tagged Image File Format | `fmt/353  TIFF`<br/>`fmt/152  DNG`<br/>`fmt/153  TIFF/IT`<br/>`fmt/154  TIFF/EP`<br/>`fmt/155  GeoTIFF`<br/>`fmt/156  TIFF-FX` | `.dng`<br/>`.tif`<br/>`.tiff` | `image/tiff` |
| Portable Network Graphics | `fmt/11  1.0`<br/>`fmt/12  1.1`<br/>`fmt/13  1.2` | `.png` | `image/png` |

### RAW Image Formats
`image/tiff`

| Name | PUID | Ext |
| ---- | ---- | --- |
|  Canon 1.0 | `fmt/593` | `.crw` |
| Canon 2.0 | `fmt/592` | `.cr2` |
| Epson | `fmt/641` | `.erf` |
| FujiFilm | `fmt/642` | `.raf` |
| Hasselblad | `fmt/1062` | `.3fr` |
| Kodak | `fmt/192` | `.dcr` |
| Minolta | `fmt/669` | `.mrw` |    
| Nikon | `fmt/202` | `.nef`, `.nrw` |
| Olympus | `fmt/668` | `.orf` |
| Panasonic | `fmt/662` | `rw2` |
| Sony ARW v1.x | `fmt/191` | `.arw` |
| Sony ARW v2.x | `fmt/1127` | .arw |
| Sony SR2 | `fmt/1126` | `.sr2` |

## Target Formats
### Preservtion Format
TIFF

- TODO: UNHCR define tiff version (`fmt/353`?)
- TODO: Multi-stream TIFFs for all multi image stream files?

### Access Formats
JPEG for single image

PDF/A container of JPEGs for multi-image stream files

### Properties
These can be measured with the ImageMagick identify utility and compared across normalised versions and the original.

| Property             | Measured With | Example |
| -------------------- | ------------- | ------- |
| /Structure/IsMulti   | identify      | Identify will characterise every image stream in a multi-image file, the streams can be counted, see example below. |
| ImageWidth           | identify -verbose | `Geometry: 363x382+0+0` |
| ImageHeight          | identify -verbose | `Geometry: 363x382+0+0` |
| X Sampling Frequency | identify -verbose | `Resolution: 96x96`<br/>`Units: PixelsPerInch`<br/>Note that this is a metadata field, not a property of the image.|
| Y Sampling Frequency | identify -verbose | `Resolution: 96x96`<br/>`Units: PixelsPerInch`<br/>Note that this is a metadata field, not a property of the image. |
| Bits per sample      | identify -verbose | `Depth: 8-bit`<br/>`Channel depth:`<br/>`  red: 8-bit`<br/>`  green: 8-bit`<br/>`blue: 8-bit` |
| Samples per pixel    | ?      |         |
| Extra Samples        | ?      |         |

## Tasks

### Characterisation
ImageMagick Identify: https://www.imagemagick.org/script/identify.php
```
$ identify -version
Version: ImageMagick 6.8.9-9 Q16 x86_64 2018-07-10 http://www.imagemagick.org
Copyright: Copyright (C) 1999-2014 ImageMagick Studio LLC
Features: DPC Modules OpenMP
Delegates: bzlib cairo djvu fftw fontconfig freetype jbig jng jpeg lcms lqr ltdl lzma openexr pangocairo png rsvg tiff wmf x xml zlib
```
#### Identify multi-stream images
##### Single image GIF
```
$ identify w3c_home.gif
w3c_home.gif GIF 72x48 72x48+0+0 8-bit sRGB 64c 988B 0.000u 0:00.000
```
##### Dual image GIF
```
$ identify w3c_home_animation.gif
w3c_home_animation.gif[0] GIF 72x48 72x48+0+0 8-bit sRGB 64c 1.93KB 0.000u 0:00.000
w3c_home_animation.gif[1] GIF 72x37 72x48+0+4 8-bit sRGB 64c 1.93KB 0.000u 0:00.000
```
##### Single image TIFF
```
$ identify CCITT_1.TIF
CCITT_1.TIF TIFF 1728x2376 1728x2376+0+0 1-bit Bilevel Gray 18.4KB 0.000u 0:00.000
```
##### Multi image TIFF
```
$ identify Multi_page24bpp.tif
Multi_page24bpp.tif
Multi_page24bpp.tif[0] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.000
Multi_page24bpp.tif[1] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.009
Multi_page24bpp.tif[2] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.009
Multi_page24bpp.tif[3] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.009
Multi_page24bpp.tif[4] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.009
Multi_page24bpp.tif[5] TIFF 363x382 363x382+0+0 8-bit sRGB 73.9KB 0.000u 0:00.009
```
### Normalisation
ImageMagick Convert: https://www.imagemagick.org/script/convert.php

TODO: decide image quality for JPEG

#### Convert to TIFF preservation derivative
```
convert -quality 80% <source> <target>.tiff
```

#### Convert to JPEG access derivative
```
convert -quality 80% <source> <target>.jpeg
```

#### Convert multi-image GIF (TIFF identical)
```
$ ls
total 32K
-rw-rw-r-- 1 cfw cfw 1.9K Jul 20 20:01 w3c_home_animation.gif

$ convert -quality 80% w3c_home_animation.gif w3c_home_animation.jpeg

$ ls
total 32K
-rw-rw-r-- 1 cfw cfw 1.8K Jul 20 21:47 w3c_home_animation-0.jpeg
-rw-rw-r-- 1 cfw cfw 2.1K Jul 20 21:47 w3c_home_animation-1.jpeg
-rw-rw-r-- 1 cfw cfw 1.9K Jul 20 20:01 w3c_home_animation.gif

$ convert w3c_home_animation*.jpeg w3c_home_animation.pdf

$ ls
total 32K
-rw-rw-r-- 1 cfw cfw 1.8K Jul 20 21:47 w3c_home_animation-0.jpeg
-rw-rw-r-- 1 cfw cfw 2.1K Jul 20 21:47 w3c_home_animation-1.jpeg
-rw-rw-r-- 1 cfw cfw 1.9K Jul 20 20:01 w3c_home_animation.gif
-rw-rw-r-- 1 cfw cfw  11K Jul 20 21:50 w3c_home_animation.pdf
```

### Multipage TIFF Example
`identify -verbose multi-page.tiff`
```
Image: Multi_page24bpp.tif
  Format: TIFF (Tagged Image File Format)
  Mime type: image/tiff
  Class: DirectClass
  Geometry: 363x382+0+0
  Resolution: 96x96
  Print size: 3.78125x3.97917
  Units: PixelsPerInch
  Type: Palette
  Endianess: LSB
  Colorspace: sRGB
  Depth: 8-bit
  Channel depth:
    red: 8-bit
    green: 8-bit
    blue: 8-bit
  Channel statistics:
    Pixels: 138666
    Red:
      min: 0 (0)
      max: 255 (1)
      mean: 217.256 (0.851983)
      standard deviation: 59.4639 (0.233192)
      kurtosis: 7.64527
      skewness: -2.94357
    Green:
      min: 0 (0)
      max: 255 (1)
      mean: 217.864 (0.85437)
      standard deviation: 59.9424 (0.235068)
      kurtosis: 8.20855
      skewness: -3.07688
    Blue:
      min: 0 (0)
      max: 255 (1)
      mean: 223.011 (0.874555)
      standard deviation: 54.1348 (0.212293)
      kurtosis: 11.661
      skewness: -3.5853
  Image statistics:
    Overall:
      min: 0 (0)
      max: 255 (1)
      mean: 219.377 (0.860303)
      standard deviation: 57.9069 (0.227086)
      kurtosis: 9.00897
      skewness: -3.18826
  Colors: 157
  Histogram:
      5962: (  0,  0,  0) #000000 black
      1845: (  0,  0,255) #0000FF blue
         2: ( 20, 22, 22) #141616 srgb(20,22,22)
         1: ( 24, 24, 24) #181818 srgb(24,24,24)
         1: ( 27, 67, 74) #1B434A srgb(27,67,74)
         2: ( 28, 28, 28) #1C1C1C grey11
         1: ( 28, 84, 91) #1C545B srgb(28,84,91)
       362: ( 40,207,228) #28CFE4 srgb(40,207,228)
       396: ( 52, 52, 52) #343434 srgb(52,52,52)
       354: ( 63,206,242) #3FCEF2 srgb(63,206,242)
         2: ( 66, 66, 66) #424242 grey26
         1: ( 66,207,242) #42CFF2 srgb(66,207,242)
        80: ( 67, 20, 34) #431422 srgb(67,20,34)
         1: ( 68,207,242) #44CFF2 srgb(68,207,242)
         1: ( 71,208,242) #47D0F2 srgb(71,208,242)
         1: ( 74,209,243) #4AD1F3 srgb(74,209,243)
         1: ( 78,210,242) #4ED2F2 srgb(78,210,242)
         4: ( 79, 38, 53) #4F2635 srgb(79,38,53)
         2: ( 80, 41, 56) #502938 srgb(80,41,56)
         1: ( 82,211,242) #52D3F2 srgb(82,211,242)
        32: ( 83, 86,102) #535666 srgb(83,86,102)
         1: ( 86,212,242) #56D4F2 srgb(86,212,242)
         1: ( 91,213,243) #5BD5F3 srgb(91,213,243)
         2: ( 93, 61, 77) #5D3D4D srgb(93,61,77)
         1: ( 95,213,243) #5FD5F3 srgb(95,213,243)
       192: (100,100,100) #646464 srgb(100,100,100)
         1: (100,215,243) #64D7F3 srgb(100,215,243)
        93: (105,105,105) #696969 DimGray
         1: (105,217,243) #69D9F3 srgb(105,217,243)
         1: (110,218,243) #6EDAF3 srgb(110,218,243)
         1: (115,219,243) #73DBF3 srgb(115,219,243)
         1: (121,220,244) #79DCF4 srgb(121,220,244)
         1: (125,221,244) #7DDDF4 srgb(125,221,244)
         2: (130, 77, 75) #824D4B srgb(130,77,75)
         2: (131, 81, 78) #83514E srgb(131,81,78)
         1: (131,222,244) #83DEF4 srgb(131,222,244)
         2: (133, 85, 83) #855553 srgb(133,85,83)
         1: (135, 90, 89) #875A59 srgb(135,90,89)
         1: (135,224,244) #87E0F4 srgb(135,224,244)
         4: (138, 95, 95) #8A5F5F srgb(138,95,95)
         1: (143,143,143) #8F8F8F grey56
         2: (148,116,117) #947475 srgb(148,116,117)
         2: (149,117,119) #957577 srgb(149,117,119)
         1: (150,119,121) #967779 srgb(150,119,121)
         4: (151,122,124) #977A7C srgb(151,122,124)
       355: (152,180,208) #98B4D0 srgb(152,180,208)
       357: (153,180,209) #99B4D1 srgb(153,180,209)
       359: (154,182,210) #9AB6D2 srgb(154,182,210)
         2: (155,159,181) #9B9FB5 srgb(155,159,181)
       328: (155,183,211) #9BB7D3 srgb(155,183,211)
         1: (156,229,244) #9CE5F4 srgb(156,229,244)
       328: (157,184,212) #9DB8D4 srgb(157,184,212)
       328: (158,185,212) #9EB9D4 srgb(158,185,212)
        89: (160,160,160) #A0A0A0 srgb(160,160,160)
         2: (160,166,188) #A0A6BC srgb(160,166,188)
         1: (161,230,244) #A1E6F4 srgb(161,230,244)
       328: (164,190,217) #A4BED9 srgb(164,190,217)
       309: (165,191,218) #A5BFDA srgb(165,191,218)
         1: (166,231,244) #A6E7F4 srgb(166,231,244)
       312: (167,192,220) #A7C0DC srgb(167,192,220)
       313: (168,193,221) #A8C1DD srgb(168,193,221)
       286: (169,195,221) #A9C3DD srgb(169,195,221)
         1: (169,214,224) #A9D6E0 srgb(169,214,224)
         1: (170,233,245) #AAE9F5 srgb(170,233,245)
        16: (171,171,171) #ABABAB grey67
       293: (171,196,223) #ABC4DF srgb(171,196,223)
       293: (172,197,224) #ACC5E0 srgb(172,197,224)
       293: (174,199,225) #AEC7E1 srgb(174,199,225)
       296: (176,200,226) #B0C8E2 srgb(176,200,226)
       277: (177,201,228) #B1C9E4 srgb(177,201,228)
       328: (178,203,229) #B2CBE5 srgb(178,203,229)
       328: (179,204,230) #B3CCE6 srgb(179,204,230)
       328: (180,205,230) #B4CDE6 srgb(180,205,230)
       328: (181,206,231) #B5CEE7 srgb(181,206,231)
       359: (183,207,232) #B7CFE8 srgb(183,207,232)
        20: (184, 67, 44) #B8432C srgb(184,67,44)
       359: (184,208,233) #B8D0E9 srgb(184,208,233)
         1: (185,185,185) #B9B9B9 srgb(185,185,185)
      1426: (185,209,234) #B9D1EA srgb(185,209,234)
         2: (186, 73, 51) #BA4933 srgb(186,73,51)
         2: (186, 74, 51) #BA4A33 srgb(186,74,51)
         2: (186,154,155) #BA9A9B srgb(186,154,155)
         5: (187, 73, 51) #BB4933 srgb(187,73,51)
         2: (187, 73, 52) #BB4934 srgb(187,73,52)
         4: (187, 74, 51) #BB4A33 srgb(187,74,51)
         3: (187, 74, 52) #BB4A34 srgb(187,74,52)
         2: (188,154,155) #BC9A9B srgb(188,154,155)
         1: (191, 83, 61) #BF533D srgb(191,83,61)
        15: (191, 83, 62) #BF533E srgb(191,83,62)
      8925: (192,192,192) #C0C0C0 silver
         1: (193,238,245) #C1EEF5 srgb(193,238,245)
         2: (195, 94, 75) #C35E4B srgb(195,94,75)
         8: (196, 94, 74) #C45E4A srgb(196,94,74)
         2: (196, 94, 75) #C45E4B srgb(196,94,75)
         4: (196, 95, 74) #C45F4A srgb(196,95,74)
         1: (201,105, 87) #C96957 srgb(201,105,87)
         8: (201,106, 87) #C96A57 srgb(201,106,87)
         8: (201,106, 88) #C96A58 srgb(201,106,88)
        26: (206,117,100) #CE7564 srgb(206,117,100)
         1: (206,117,101) #CE7565 srgb(206,117,101)
         5: (210,126,110) #D27E6E srgb(210,126,110)
         8: (210,126,111) #D27E6F srgb(210,126,111)
         4: (210,127,110) #D27F6E srgb(210,127,110)
         5: (210,127,111) #D27F6F srgb(210,127,111)
         1: (210,240,247) #D2F0F7 srgb(210,240,247)
         3: (211,126,111) #D37E6F srgb(211,126,111)
         1: (211,127,110) #D37F6E srgb(211,127,110)
         1: (211,127,111) #D37F6F srgb(211,127,111)
         6: (215,215,215) #D7D7D7 srgb(215,215,215)
         4: (217,154,142) #D99A8E srgb(217,154,142)
         2: (218,157,145) #DA9D91 srgb(218,157,145)
         1: (219,233,236) #DBE9EC srgb(219,233,236)
         6: (220,220,220) #DCDCDC gainsboro
         2: (221,163,152) #DDA398 srgb(221,163,152)
         2: (223,148,135) #DF9487 srgb(223,148,135)
        10: (223,149,135) #DF9587 srgb(223,149,135)
         2: (224,148,135) #E09487 srgb(224,148,135)
         4: (224,149,135) #E09587 srgb(224,149,135)
         2: (224,171,160) #E0ABA0 srgb(224,171,160)
         1: (225,152,139) #E1988B srgb(225,152,139)
         1: (225,152,140) #E1988C srgb(225,152,140)
         9: (225,153,139) #E1998B srgb(225,153,139)
         1: (225,153,140) #E1998C srgb(225,153,140)
         1: (226,152,139) #E2988B srgb(226,152,139)
         3: (226,153,139) #E2998B srgb(226,153,139)
         5: (226,226,226) #E2E2E2 srgb(226,226,226)
         2: (227,157,144) #E39D90 srgb(227,157,144)
         1: (227,158,144) #E39E90 srgb(227,158,144)
        89: (227,227,227) #E3E3E3 grey89
        10: (228,157,144) #E49D90 srgb(228,157,144)
         3: (228,158,144) #E49E90 srgb(228,158,144)
         2: (228,180,171) #E4B4AB srgb(228,180,171)
         2: (228,228,228) #E4E4E4 srgb(228,228,228)
         1: (229,162,149) #E5A295 srgb(229,162,149)
        14: (230,162,149) #E6A295 srgb(230,162,149)
         2: (230,163,149) #E6A395 srgb(230,163,149)
         6: (231,166,153) #E7A699 srgb(231,166,153)
        12: (232,166,153) #E8A699 srgb(232,166,153)
         2: (232,166,154) #E8A69A srgb(232,166,154)
         7: (232,167,153) #E8A799 srgb(232,167,153)
         2: (232,189,181) #E8BDB5 srgb(232,189,181)
        27: (233,169,156) #E9A99C srgb(233,169,156)
         3: (233,233,233) #E9E9E9 srgb(233,233,233)
        27: (236,198,192) #ECC6C0 srgb(236,198,192)
         4: (237,196,189) #EDC4BD srgb(237,196,189)
         2: (239,200,193) #EFC8C1 srgb(239,200,193)
         2: (240,203,196) #F0CBC4 srgb(240,203,196)
    109606: (240,240,240) #F0F0F0 grey94
         2: (242,206,200) #F2CEC8 srgb(242,206,200)
         2: (243,209,202) #F3D1CA srgb(243,209,202)
        27: (244,211,204) #F4D3CC srgb(244,211,204)
         1: (247,247,247) #F7F7F7 grey97
         5: (250,250,250) #FAFAFA grey98
         1: (252,252,252) #FCFCFC grey99
         6: (253,253,253) #FDFDFD srgb(253,253,253)
       843: (255,  0,  0) #FF0000 red
       822: (255,255,255) #FFFFFF white
  Rendering intent: Perceptual
  Gamma: 0.454545
  Chromaticity:
    red primary: (0.64,0.33)
    green primary: (0.3,0.6)
    blue primary: (0.15,0.06)
    white point: (0.3127,0.329)
  Background color: white
  Border color: srgb(223,223,223)
  Matte color: grey74
  Transparent color: black
  Interlace: None
  Intensity: Undefined
  Compose: Over
  Page geometry: 363x382+0+0
  Dispose: Undefined
  Iterations: 0
  Scene: 0 of 6
  Compression: LZW
  Orientation: TopLeft
  Properties:
    date:create: 2018-07-04T12:25:01+01:00
    date:modify: 2018-07-04T12:25:01+01:00
    signature: 4281cc379f68af58c899287b5952036848dcd382eecf084f5e96529d8b15dd0a
    tiff:alpha: unspecified
    tiff:endian: lsb
    tiff:photometric: RGB
    tiff:rows-per-strip: 382
  Artifacts:
    filename: Multi_page24bpp.tif
    verbose: true
  Tainted: False
  Filesize: 73.9KB
  Number pixels: 139K
  Pixels per second: 6.933MB
  User time: 0.020u
  Elapsed time: 0:01.019
  Version: ImageMagick 6.8.9-9 Q16 x86_64 2018-06-11 http://www.imagemagick.org
  Image: Multi_page24bpp.tif
    Format: TIFF (Tagged Image File Format)
    Mime type: image/tiff
    Class: DirectClass
    Geometry: 363x382+0+0
    Resolution: 96x96
    Print size: 3.78125x3.97917
    Units: PixelsPerInch
    Type: Palette
    Endianess: LSB
    Colorspace: sRGB
    Depth: 8-bit
    Channel depth:
      red: 8-bit
      green: 8-bit
      blue: 8-bit
    Channel statistics:
      Pixels: 138666
      Red:
        min: 0 (0)
        max: 255 (1)
        mean: 217.256 (0.851983)
        standard deviation: 59.4639 (0.233192)
        kurtosis: 7.64527
        skewness: -2.94357
      Green:
        min: 0 (0)
        max: 255 (1)
        mean: 217.864 (0.85437)
        standard deviation: 59.9424 (0.235068)
        kurtosis: 8.20855
        skewness: -3.07688
      Blue:
        min: 0 (0)
        max: 255 (1)
        mean: 223.011 (0.874555)
        standard deviation: 54.1348 (0.212293)
        kurtosis: 11.661
        skewness: -3.5853
    Image statistics:
      Overall:
        min: 0 (0)
        max: 255 (1)
        mean: 219.377 (0.860303)
        standard deviation: 57.9069 (0.227086)
        kurtosis: 9.00897
        skewness: -3.18826
    Colors: 157
    Histogram:
        5962: (  0,  0,  0) #000000 black
        1845: (  0,  0,255) #0000FF blue
           2: ( 20, 22, 22) #141616 srgb(20,22,22)
           1: ( 24, 24, 24) #181818 srgb(24,24,24)
           1: ( 27, 67, 74) #1B434A srgb(27,67,74)
           2: ( 28, 28, 28) #1C1C1C grey11
           1: ( 28, 84, 91) #1C545B srgb(28,84,91)
         362: ( 40,207,228) #28CFE4 srgb(40,207,228)
         396: ( 52, 52, 52) #343434 srgb(52,52,52)
         354: ( 63,206,242) #3FCEF2 srgb(63,206,242)
           2: ( 66, 66, 66) #424242 grey26
           1: ( 66,207,242) #42CFF2 srgb(66,207,242)
          80: ( 67, 20, 34) #431422 srgb(67,20,34)
           1: ( 68,207,242) #44CFF2 srgb(68,207,242)
           1: ( 71,208,242) #47D0F2 srgb(71,208,242)
           1: ( 74,209,243) #4AD1F3 srgb(74,209,243)
           1: ( 78,210,242) #4ED2F2 srgb(78,210,242)
           4: ( 79, 38, 53) #4F2635 srgb(79,38,53)
           2: ( 80, 41, 56) #502938 srgb(80,41,56)
           1: ( 82,211,242) #52D3F2 srgb(82,211,242)
          32: ( 83, 86,102) #535666 srgb(83,86,102)
           1: ( 86,212,242) #56D4F2 srgb(86,212,242)
           1: ( 91,213,243) #5BD5F3 srgb(91,213,243)
           2: ( 93, 61, 77) #5D3D4D srgb(93,61,77)
           1: ( 95,213,243) #5FD5F3 srgb(95,213,243)
         192: (100,100,100) #646464 srgb(100,100,100)
           1: (100,215,243) #64D7F3 srgb(100,215,243)
          93: (105,105,105) #696969 DimGray
           1: (105,217,243) #69D9F3 srgb(105,217,243)
           1: (110,218,243) #6EDAF3 srgb(110,218,243)
           1: (115,219,243) #73DBF3 srgb(115,219,243)
           1: (121,220,244) #79DCF4 srgb(121,220,244)
           1: (125,221,244) #7DDDF4 srgb(125,221,244)
           2: (130, 77, 75) #824D4B srgb(130,77,75)
           2: (131, 81, 78) #83514E srgb(131,81,78)
           1: (131,222,244) #83DEF4 srgb(131,222,244)
           2: (133, 85, 83) #855553 srgb(133,85,83)
           1: (135, 90, 89) #875A59 srgb(135,90,89)
           1: (135,224,244) #87E0F4 srgb(135,224,244)
           4: (138, 95, 95) #8A5F5F srgb(138,95,95)
           1: (143,143,143) #8F8F8F grey56
           2: (148,116,117) #947475 srgb(148,116,117)
           2: (149,117,119) #957577 srgb(149,117,119)
           1: (150,119,121) #967779 srgb(150,119,121)
           4: (151,122,124) #977A7C srgb(151,122,124)
         355: (152,180,208) #98B4D0 srgb(152,180,208)
         357: (153,180,209) #99B4D1 srgb(153,180,209)
         359: (154,182,210) #9AB6D2 srgb(154,182,210)
           2: (155,159,181) #9B9FB5 srgb(155,159,181)
         328: (155,183,211) #9BB7D3 srgb(155,183,211)
           1: (156,229,244) #9CE5F4 srgb(156,229,244)
         328: (157,184,212) #9DB8D4 srgb(157,184,212)
         328: (158,185,212) #9EB9D4 srgb(158,185,212)
          89: (160,160,160) #A0A0A0 srgb(160,160,160)
           2: (160,166,188) #A0A6BC srgb(160,166,188)
           1: (161,230,244) #A1E6F4 srgb(161,230,244)
         328: (164,190,217) #A4BED9 srgb(164,190,217)
         309: (165,191,218) #A5BFDA srgb(165,191,218)
           1: (166,231,244) #A6E7F4 srgb(166,231,244)
         312: (167,192,220) #A7C0DC srgb(167,192,220)
         313: (168,193,221) #A8C1DD srgb(168,193,221)
         286: (169,195,221) #A9C3DD srgb(169,195,221)
           1: (169,214,224) #A9D6E0 srgb(169,214,224)
           1: (170,233,245) #AAE9F5 srgb(170,233,245)
          16: (171,171,171) #ABABAB grey67
         293: (171,196,223) #ABC4DF srgb(171,196,223)
         293: (172,197,224) #ACC5E0 srgb(172,197,224)
         293: (174,199,225) #AEC7E1 srgb(174,199,225)
         296: (176,200,226) #B0C8E2 srgb(176,200,226)
         277: (177,201,228) #B1C9E4 srgb(177,201,228)
         328: (178,203,229) #B2CBE5 srgb(178,203,229)
         328: (179,204,230) #B3CCE6 srgb(179,204,230)
         328: (180,205,230) #B4CDE6 srgb(180,205,230)
         328: (181,206,231) #B5CEE7 srgb(181,206,231)
         359: (183,207,232) #B7CFE8 srgb(183,207,232)
          20: (184, 67, 44) #B8432C srgb(184,67,44)
         359: (184,208,233) #B8D0E9 srgb(184,208,233)
           1: (185,185,185) #B9B9B9 srgb(185,185,185)
        1426: (185,209,234) #B9D1EA srgb(185,209,234)
           2: (186, 73, 51) #BA4933 srgb(186,73,51)
           2: (186, 74, 51) #BA4A33 srgb(186,74,51)
           2: (186,154,155) #BA9A9B srgb(186,154,155)
           5: (187, 73, 51) #BB4933 srgb(187,73,51)
           2: (187, 73, 52) #BB4934 srgb(187,73,52)
           4: (187, 74, 51) #BB4A33 srgb(187,74,51)
           3: (187, 74, 52) #BB4A34 srgb(187,74,52)
           2: (188,154,155) #BC9A9B srgb(188,154,155)
           1: (191, 83, 61) #BF533D srgb(191,83,61)
          15: (191, 83, 62) #BF533E srgb(191,83,62)
        8925: (192,192,192) #C0C0C0 silver
           1: (193,238,245) #C1EEF5 srgb(193,238,245)
           2: (195, 94, 75) #C35E4B srgb(195,94,75)
           8: (196, 94, 74) #C45E4A srgb(196,94,74)
           2: (196, 94, 75) #C45E4B srgb(196,94,75)
           4: (196, 95, 74) #C45F4A srgb(196,95,74)
           1: (201,105, 87) #C96957 srgb(201,105,87)
           8: (201,106, 87) #C96A57 srgb(201,106,87)
           8: (201,106, 88) #C96A58 srgb(201,106,88)
          26: (206,117,100) #CE7564 srgb(206,117,100)
           1: (206,117,101) #CE7565 srgb(206,117,101)
           5: (210,126,110) #D27E6E srgb(210,126,110)
           8: (210,126,111) #D27E6F srgb(210,126,111)
           4: (210,127,110) #D27F6E srgb(210,127,110)
           5: (210,127,111) #D27F6F srgb(210,127,111)
           1: (210,240,247) #D2F0F7 srgb(210,240,247)
           3: (211,126,111) #D37E6F srgb(211,126,111)
           1: (211,127,110) #D37F6E srgb(211,127,110)
           1: (211,127,111) #D37F6F srgb(211,127,111)
           6: (215,215,215) #D7D7D7 srgb(215,215,215)
           4: (217,154,142) #D99A8E srgb(217,154,142)
           2: (218,157,145) #DA9D91 srgb(218,157,145)
           1: (219,233,236) #DBE9EC srgb(219,233,236)
           6: (220,220,220) #DCDCDC gainsboro
           2: (221,163,152) #DDA398 srgb(221,163,152)
           2: (223,148,135) #DF9487 srgb(223,148,135)
          10: (223,149,135) #DF9587 srgb(223,149,135)
           2: (224,148,135) #E09487 srgb(224,148,135)
           4: (224,149,135) #E09587 srgb(224,149,135)
           2: (224,171,160) #E0ABA0 srgb(224,171,160)
           1: (225,152,139) #E1988B srgb(225,152,139)
           1: (225,152,140) #E1988C srgb(225,152,140)
           9: (225,153,139) #E1998B srgb(225,153,139)
           1: (225,153,140) #E1998C srgb(225,153,140)
           1: (226,152,139) #E2988B srgb(226,152,139)
           3: (226,153,139) #E2998B srgb(226,153,139)
           5: (226,226,226) #E2E2E2 srgb(226,226,226)
           2: (227,157,144) #E39D90 srgb(227,157,144)
           1: (227,158,144) #E39E90 srgb(227,158,144)
          89: (227,227,227) #E3E3E3 grey89
          10: (228,157,144) #E49D90 srgb(228,157,144)
           3: (228,158,144) #E49E90 srgb(228,158,144)
           2: (228,180,171) #E4B4AB srgb(228,180,171)
           2: (228,228,228) #E4E4E4 srgb(228,228,228)
           1: (229,162,149) #E5A295 srgb(229,162,149)
          14: (230,162,149) #E6A295 srgb(230,162,149)
           2: (230,163,149) #E6A395 srgb(230,163,149)
           6: (231,166,153) #E7A699 srgb(231,166,153)
          12: (232,166,153) #E8A699 srgb(232,166,153)
           2: (232,166,154) #E8A69A srgb(232,166,154)
           7: (232,167,153) #E8A799 srgb(232,167,153)
           2: (232,189,181) #E8BDB5 srgb(232,189,181)
          27: (233,169,156) #E9A99C srgb(233,169,156)
           3: (233,233,233) #E9E9E9 srgb(233,233,233)
          27: (236,198,192) #ECC6C0 srgb(236,198,192)
           4: (237,196,189) #EDC4BD srgb(237,196,189)
           2: (239,200,193) #EFC8C1 srgb(239,200,193)
           2: (240,203,196) #F0CBC4 srgb(240,203,196)
      109606: (240,240,240) #F0F0F0 grey94
           2: (242,206,200) #F2CEC8 srgb(242,206,200)
           2: (243,209,202) #F3D1CA srgb(243,209,202)
          27: (244,211,204) #F4D3CC srgb(244,211,204)
           1: (247,247,247) #F7F7F7 grey97
           5: (250,250,250) #FAFAFA grey98
           1: (252,252,252) #FCFCFC grey99
           6: (253,253,253) #FDFDFD srgb(253,253,253)
         843: (255,  0,  0) #FF0000 red
         822: (255,255,255) #FFFFFF white
    Rendering intent: Perceptual
    Gamma: 0.454545
    Chromaticity:
      red primary: (0.64,0.33)
      green primary: (0.3,0.6)
      blue primary: (0.15,0.06)
      white point: (0.3127,0.329)
    Background color: white
    Border color: srgb(223,223,223)
    Matte color: grey74
    Transparent color: black
    Interlace: None
    Intensity: Undefined
    Compose: Over
    Page geometry: 363x382+0+0
    Dispose: Undefined
    Iterations: 0
    Scene: 0 of 6
    Compression: LZW
    Orientation: TopLeft
    Properties:
      date:create: 2018-07-04T12:25:01+01:00
      date:modify: 2018-07-04T12:25:01+01:00
      signature: 4281cc379f68af58c899287b5952036848dcd382eecf084f5e96529d8b15dd0a
      tiff:alpha: unspecified
      tiff:endian: lsb
      tiff:photometric: RGB
      tiff:rows-per-strip: 382
    Artifacts:
      filename: Multi_page24bpp.tif
      verbose: true
    Tainted: False
    Filesize: 73.9KB
    Number pixels: 139K
    Pixels per second: 6.933MB
    User time: 0.020u
    Elapsed time: 0:01.019
    Version: ImageMagick 6.8.9-9 Q16 x86_64 2018-06-11 http://www.imagemagick.org
```
