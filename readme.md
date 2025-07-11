# Discord Music Bot

A powerful Discord music bot built with Python that can play music from various sources including YouTube, SoundCloud, Bandcamp, and more. Features include queue management, playlist support, and multi-platform audio extraction.

## Features

- 🎵 Play music from multiple sources (YouTube, SoundCloud, Bandcamp, etc.)
- 📋 Queue management with history tracking
- 🎶 Playlist creation and management
- 🔊 Volume control and audio quality settings
- ⏯️ Playback controls (play, pause, skip, stop)
- 🔀 Queue shuffling and looping
- 📱 Rich Discord embeds and user-friendly interface
- 🛡️ Rate limiting and security features
- 🌐 Multi-server support with isolated queues

## Ubuntu Installation Guide[automated]
```
bash <(curl -s https://raw.githubusercontent.com/TS-25/discord-music-bot/refs/heads/main/install)
```

### Prerequisites

Make sure your Ubuntu system is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 1: Install System Dependencies

Install Python 3.11+ and required system packages:

```bash
# Install Python 3.11 and pip
sudo apt install python3.11 python3.11-pip python3.11-dev -y

# Install FFmpeg for audio processing
sudo apt install ffmpeg -y

# Install libffi and libsodium for Discord voice support
sudo apt install libffi-dev libsodium-dev -y

# Install Opus codec for Discord voice
sudo apt install libopus0 libopus-dev -y

# Install build essentials (required for some Python packages)
sudo apt install build-essential -y
```

### Step 2: Install Python Dependencies

Install the required Python packages:

```bash
# Install core dependencies
pip3.11 install discord.py>=2.5.0
pip3.11 install yt-dlp>=2025.6.30
pip3.11 install PyNaCl>=1.5.0

# Install additional dependencies
pip3.11 install aiohttp>=3.8.0
pip3.11 install cffi>=1.15.0
pip3.11 install pycparser>=2.21
pip3.11 install opuslib>=3.0.0

# Or install all at once:
pip3.11 install discord.py yt-dlp PyNaCl aiohttp cffi pycparser opuslib
```

### Step 3: Clone or Download the Bot

Download the bot files to your Ubuntu system:

```bash
# Create a directory for the bot
mkdir ~/discord-music-bot
cd ~/discord-music-bot

# Copy all bot files to this directory
# (main.py, music_bot.py, config.py, etc.)
```

### Step 4: Set Up Discord Bot Token

1. **Create a Discord Application:**
   - Go to https://discord.com/developers/applications
   - Click "New Application" and give it a name
   - Go to the "Bot" section and click "Add Bot"
   - Copy the bot token

2. **Set Environment Variable:**
   ```bash
   # Option 1: Set temporarily (for current session)
   export DISCORD_BOT_TOKEN="your_bot_token_here"
   
   # Option 2: Set permanently (add to ~/.bashrc)
   echo 'export DISCORD_BOT_TOKEN="your_bot_token_here"' >> ~/.bashrc
   source ~/.bashrc
   ```

3. **Alternative: Create a .env file:**
   ```bash
   # Create .env file in bot directory
   echo "DISCORD_BOT_TOKEN=your_bot_token_here" > .env
   ```

### Step 5: Configure Bot Permissions

In the Discord Developer Portal:

1. Go to "OAuth2" → "URL Generator"
2. Select "bot" scope
3. Select these permissions:
   - Send Messages
   - Use Slash Commands
   - Connect
   - Speak
   - Use Voice Activity
   - Read Message History
4. Copy the generated URL and use it to invite the bot to your server

### Step 6: Run the Bot

Start the bot:

```bash
cd ~/discord-music-bot
python3.11 main.py
```

### Step 7: Test the Bot

In your Discord server, try these commands:

```
!help          # Show all available commands
!join          # Bot joins your voice channel
!play song     # Play a song
!queue         # Show current queue
!skip          # Skip current song
```

## Configuration Options

You can customize the bot by setting these environment variables:

```bash
# Bot Configuration
export BOT_PREFIX="!"                    # Command prefix
export MAX_QUEUE_SIZE="100"              # Maximum songs in queue
export DEFAULT_VOLUME="0.5"              # Default volume (0.0-1.0)
export VOICE_TIMEOUT="300"               # Voice channel timeout (seconds)

# Audio Quality
export AUDIO_QUALITY="medium"            # low, medium, high
export PREFER_OPUS="true"                # Use Opus encoding

# Features
export ALLOW_PLAYLISTS="true"            # Enable playlist support
export ENABLE_VOTE_SKIP="false"          # Enable vote-based skipping
export REQUIRE_SAME_CHANNEL="true"       # Require users to be in same voice channel

# Rate Limiting
export RATE_LIMIT_PER_USER="5"           # Commands per minute per user
export RATE_LIMIT_GLOBAL="50"            # Global commands per minute

# Logging
export LOG_LEVEL="INFO"                  # DEBUG, INFO, WARNING, ERROR
export LOG_COMMANDS="true"               # Log command usage
```

## Running as a Service (Optional)

To run the bot as a system service that starts automatically:

1. **Create a service file:**
   ```bash
   sudo nano /etc/systemd/system/discord-music-bot.service
   ```

2. **Add this content:**
   ```ini
   [Unit]
   Description=Discord Music Bot
   After=network.target

   [Service]
   Type=simple
   User=your_username
   WorkingDirectory=/home/your_username/discord-music-bot
   Environment=DISCORD_BOT_TOKEN=your_bot_token_here
   ExecStart=/usr/bin/python3.11 main.py
   Restart=always
   RestartSec=10

   [Install]
   WantedBy=multi-user.target
   ```

3. **Enable and start the service:**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable discord-music-bot
   sudo systemctl start discord-music-bot
   
   # Check status
   sudo systemctl status discord-music-bot
   
   # View logs
   sudo journalctl -u discord-music-bot -f
   ```

## Troubleshooting

### Common Issues

1. **"ModuleNotFoundError" for discord.py:**
   ```bash
   pip3.11 install --upgrade discord.py
   ```

2. **FFmpeg not found:**
   ```bash
   sudo apt install ffmpeg
   which ffmpeg  # Should show: /usr/bin/ffmpeg
   ```

3. **Voice connection issues:**
   ```bash
   sudo apt install libopus0 libopus-dev libffi-dev libsodium-dev
   pip3.11 install --upgrade PyNaCl
   ```

4. **YouTube extraction errors:**
   ```bash
   pip3.11 install --upgrade yt-dlp
   ```

5. **Permission denied errors:**
   ```bash
   chmod +x main.py
   # Or run with: python3.11 main.py
   ```

### Log Files

The bot creates a `bot.log` file in the same directory with detailed logs. Check this file for error details:

```bash
tail -f bot.log  # Follow log in real-time
```

## Supported Audio Sources

- ✅ YouTube (with fallback handling)
- ✅ SoundCloud
- ✅ Bandcamp
- ✅ Vimeo
- ✅ Twitch VODs
- ✅ Direct audio URLs
- ✅ Many other platforms supported by yt-dlp

## Commands

### Music Commands
- `!play <song>` - Play a song or add to queue
- `!skip` - Skip current song
- `!pause` - Pause playback
- `!resume` - Resume playback
- `!stop` - Stop playback and clear queue
- `!queue` - Show current queue
- `!nowplaying` - Show currently playing song
- `!volume <0-100>` - Set volume

### Playlist Commands
- `!playlist create <name>` - Create a new playlist
- `!playlist add <name> <song>` - Add song to playlist
- `!playlist play <name>` - Play entire playlist
- `!playlist list` - Show all playlists
- `!playlist show <name>` - Show playlist contents

### Utility Commands
- `!join` - Join your voice channel
- `!leave` - Leave voice channel
- `!shuffle` - Shuffle the queue
- `!clear` - Clear the queue
- `!help` - Show all commands

## Support

For issues and support:
1. Check the `bot.log` file for error details
2. Ensure all dependencies are installed correctly
3. Verify your Discord bot token is correct
4. Make sure the bot has proper permissions in your Discord server

