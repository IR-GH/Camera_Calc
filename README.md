# Camera Calc

Storage calculator for cinema cameras. Pick **brand → model → raster → fps → codec**, type a duration in minutes or hours, and get the total in GB (plus TB, GiB, data rate, GB/hour and frame count).

Only the combinations each camera can actually record are offered, and options above a camera's media write speed are hidden.

It's a single self-contained `index.html` (no build step, no dependencies). Open it locally or host it with GitHub Pages.

## Cameras

| Brand | Models |
|---|---|
| Sony | FX3, Alpha 7S III, FX6, FX9, BURANO, VENICE (1), VENICE 2 8K, VENICE 2 6K |
| Canon | EOS C300 Mark III, EOS C400, EOS C500 Mark II, EOS C70, EOS C80, EOS R5 C |
| RED | EPIC (MX), EPIC DRAGON, WEAPON 6K, WEAPON 8K S35 (HELIUM), WEAPON 8K VV, MONSTRO 8K VV, KOMODO 6K, KOMODO-X, V-RAPTOR 8K VV / [X] |
| ARRI | ALEXA 35, ALEXA 35 Xtreme, ALEXA Mini, ALEXA Mini LF, AMIRA |

## The maths

```
GB = Mb/s × seconds ÷ 8 ÷ 1000        (decimal GB, as on card labels and macOS)
```

| Codec family | How the rate is worked out |
|---|---|
| ARRIRAW | photosites × bit depth (12-bit; 13-bit on ALEXA 35) + 221 KB frame header. Matches ARRI's published 20.9 MB/frame (Mini LF OG) and 1,386 / 2,214 Mb/s (2.8K / 3.4K OG at 24 fps). |
| ARRICORE | 50% of ARRIRAW. ARRI only publishes it as "half the data rate of ARRIRAW", so it's flagged as an estimate. |
| ProRes (ARRI, VENICE 2) | Bits per pixel from the maker's own tables (about 12% above Apple's targets). |
| ProRes (others) | Apple ProRes white-paper targets, scaled by pixel count. |
| X-OCN | Sony's published Mb/s per imager mode (VENICE 2 / X-OCN white paper); unlisted modes scaled by photosite count. BURANO derived from Sony record times. |
| REDCODE (DSMC1/DSMC2) | width × height × 14 bits ÷ ratio. Matches RED's recording-time calculator to 0.02%. |
| R3D NE (KOMODO, V-RAPTOR) | RED's published MB/s at 6K and 8K 24p (HQ / MQ / LQ / ELQ), scaled by photosite count. |
| Cinema RAW Light, XF-AVC Intra (C400/C80), XAVC-I | Fixed size per frame, from the maker's per-fps tables. |
| Long GOP (XAVC S/HS/L, XF-AVC LGOP, HEVC, H.264) | The bitrate band printed in the manual for that frame rate. |

High-speed modes (Sony S&Q, Canon Slow & Fast) are calculated per second of real shooting time.

## Spot checks against published figures

| Check | Calc | Published |
|---|---|---|
| ALEXA Mini 2.8K 16:9 ARRIRAW 24 fps | 1,386 Mb/s | 1,386 Mb/s (ARRI) |
| ALEXA Mini 3.4K OG ARRIRAW 24 fps | 2,214 Mb/s | 2,214 Mb/s (ARRI) |
| AMIRA UHD ProRes 4444 XQ / HD 4444 XQ 24 fps | 1,784 / 446 Mb/s | 1,784 / 446 Mb/s (ARRI) |
| VENICE 2 8.2K 17:9 X-OCN ST 60 fps | 6,605 Mb/s | 6,606 Mb/s (Sony) |
| VENICE 2 4K ProRes 4444 XQ 24 fps | 1,911 Mb/s | 1,911 Mb/s (Sony) |
| MONSTRO 8K REDCODE 5:1 24 fps | 2,378 Mb/s | 2,378 Mb/s (RED) |
| V-RAPTOR 8K ELQ 24 fps | 800 Mb/s | 100 MB/s (RED) |
| Canon C400 6K RAW HQ 25 fps | 1,800 Mb/s | 1,800 Mb/s (Canon) |
| Canon R5 C 8K RAW LT 59.94 fps | 2,572 Mb/s | 2,570 Mb/s (Canon) |

## Known gaps

- EPIC (MX) frame-rate limits are from the original spec sheet; RED's current docs don't list them.
- VENICE 2 6K 5.7K 16:9 / 4K 6:5 and VENICE (1) 6K 17:9 / 1.85 / 2.39 max frame rates are set conservatively at 60 fps.
- Canon C400 6K 3:2 Open Gate max set at 30 fps (not confirmed).
- V-RAPTOR 7K and 8K 2.4:1 HQ/MQ limits estimated at 72 fps.
- DNxHR (RED DSMC2) and ProRes RAW (external recorders) aren't included.

Confirm against current firmware before a big job.
