# Papaj 21:37 Plugin

AMX Mod X plugin that applies a yellow screen filter and plays music at exactly 21:37 every day or on manual trigger.

[![Papaj Plugin](https://img.youtube.com/vi/rDx6avWTzb8/0.jpg)](https://www.youtube.com/watch?v=rDx6avWTzb8)


## Features

- **Automatic trigger at 21:37** - Every day at 21:37 (9:37 PM), the effect activates automatically
- **Manual console trigger** - Players can activate it anytime via console command
- **Yellow screen filter** - Semi-transparent yellow overlay for all players
- **Music playback** - Plays `papaj2137.wav` during the effect
- **60-second duration** - Effect lasts exactly one minute
- **Round-restart persistent** - Filter persists through round changes

## Installation

### 1. Compile the plugin

```bash
amxxpc papaj.sma
```

This will create `papaj.amxx`

### 2. Install the plugin

Copy `papaj.amxx` to your server:
```
addons/amxmodx/plugins/papaj.amxx
```

Add it to `addons/amxmodx/configs/plugins.ini`:
```
papaj.amxx
```

### 3. Add the sound file

Place your WAV file in the sound directory:
```
cstrike/sound/papaj2137.wav
```

**Sound file requirements:**
- Format: WAV (uncompressed PCM)
- Channels: Mono (1 channel, recommended for smaller file size)
- Sample Rate: 22050 Hz (recommended) or 11025 Hz for smaller file
- Bit Depth: 16-bit
- Duration: ~60 seconds (to match filter duration)

**Converting MP3 to WAV:**
```bash
# Standard quality (~2.7 MB for 60s)
ffmpeg -i papaj2137.mp3 -ar 22050 -ac 1 -sample_fmt s16 papaj2137.wav

# Smaller file size (~1.3 MB for 60s) - Recommended for most use cases
ffmpeg -i papaj2137.mp3 -ar 11025 -ac 1 -sample_fmt s16 papaj2137.wav

# Smallest file (~680 KB for 60s) - Lower quality but very small
ffmpeg -i papaj2137.mp3 -ar 11025 -ac 1 -acodec pcm_u8 papaj2137.wav
```

**Quality vs Size Trade-offs:**
- **22050 Hz, 16-bit**: Best quality, larger file (~2.7 MB/60s)
- **11025 Hz, 16-bit**: Good quality, half the size (~1.3 MB/60s) - **Recommended**
- **11025 Hz, 8-bit**: Lower quality with noise, smallest file (~680 KB/60s)

### 4. Restart server or change map

```
amx_map de_dust2
```

## Usage

### Automatic Trigger
The plugin automatically activates at **21:37** (9:37 PM) server time every day. No action required.

### Manual Trigger
Players can trigger the effect manually:

1. Open console (press `~` key)
2. Type: `papaj2137`
3. Press Enter

**Note:** The console command is hidden - other players won't see who activated it in chat.

## Configuration

Edit these constants in `papaj.sma` before compiling:

```c
#define FILTER_DURATION 60.0          // Duration in seconds (default: 60)
#define SOUND_FILE "papaj2137.wav"    // Sound file name (WAV format)
#define TASK_MAINTAIN 2137            // Task ID for filter maintenance
#define TASK_REMOVE 21370             // Task ID for filter removal
```

### Changing the sound file
1. Edit `SOUND_FILE` constant
2. Recompile the plugin
3. Place the new WAV file in `cstrike/sound/`

### Changing duration
1. Edit `FILTER_DURATION` constant (in seconds)
2. Recompile the plugin
3. Make sure your WAV file length matches the duration

## Technical Details

- **Filter color:** Yellow (RGB: 255, 255, 0)
- **Filter opacity:** 100/255 (semi-transparent)
- **Maintenance interval:** 0.5 seconds (to survive round restarts)
- **Time check interval:** 30 seconds
- **Console command:** `papaj2137` (hidden from chat)
- **Works with:** All Counter-Strike 1.6 servers running AMX Mod X

### WAV vs MP3 Playback

**Why WAV instead of MP3?**

This plugin uses **WAV format** for music playback because:

- ✅ **Won't be interrupted** - Other plugins playing MP3s will not stop this plugin's music
- ✅ **Can be mixed** - WAV sounds can be layered with other game sounds
- ✅ **More reliable** - Native sound format for Half-Life/GoldSrc engine
- ✅ **No interference** - Works independently from MP3 playback system

**Trade-off:**
- ❌ **Larger file size** - WAV files are 10-50x larger than MP3 (e.g., 60s @ 22050Hz mono = ~2.7MB, or ~1.3MB @ 11025Hz)

**Note:** The GoldSrc engine's MP3 system can only play one MP3 per client at a time, which is why this plugin switched to WAV format. If you need to use MP3 instead, be aware that other plugins can interrupt the music playback.

## Troubleshooting

### Sound doesn't play
- Verify the WAV file exists at `cstrike/sound/papaj2137.wav`
- Check file format (must be WAV, not MP3)
- Ensure file is mono (1 channel) at 22050 Hz or 11025 Hz
- Ensure file is uncompressed PCM WAV, not compressed
- Try reconverting with: `ffmpeg -i input.mp3 -ar 22050 -ac 1 -sample_fmt s16 papaj2137.wav`

### Filter disappears on round restart
- This should not happen with the current version
- If it does, check that the `maintain_yellow_filter` task is running
- Check server logs for errors

### Effect doesn't trigger at 21:37
- Verify server time: type `amx_time` in server console
- Check server logs for: `"Papaj effect auto-triggered at 21:37"`
- Ensure plugin is loaded: type `amx_plugins` in console

### Players can't download the sound file
- The WAV is precached using `precache_sound()`
- Clients should automatically download it when connecting
- If not, check FastDL settings or add to resources.ini
- **Note:** WAV files are larger (1.3-2.7MB depending on quality), so download may take longer on slow connections

## Credits

- **Plugin:** Papaj 21:37
- **Version:** 1.0
- **Author:** bordeux
- **Platform:** AMX Mod X

## License

Free to use and modify.