# macOS Tools Manual for Claude Code

## Content Acquisition Tools

### 1. yt-dlp
**Purpose**: Video downloader supporting 1000+ websites
**Command**: `yt-dlp`
**Common Usage**:
- Download video: `yt-dlp [URL]`
- Audio only: `yt-dlp -x --audio-format mp3 [URL]`
- Best quality: `yt-dlp -f best [URL]`
- Batch download: `yt-dlp -a urls.txt`
- Custom output: `yt-dlp -o "%(title)s.%(ext)s" [URL]`

### 2. aria2
**Purpose**: Multi-threaded download manager
**Command**: `aria2c`
**Common Usage**:
- Basic download: `aria2c [URL]`
- Multi-connection: `aria2c -x 16 -s 16 [URL]`
- Continue download: `aria2c -c [URL]`
- RPC mode: `aria2c --enable-rpc --rpc-listen-all=true --rpc-allow-origin-all`
- Download to directory: `aria2c -d /path/to/directory [URL]`

### 3. s-search
**Purpose**: Command-line search tool
**Command**: `s`
**Usage**:
- Google search: `s -p google "search term"`
- Baidu search: `s -p baidu "search term"`
- DuckDuckGo: `s -p duckduckgo "search term"`

## Content Processing Tools

### 4. ImageMagick
**Purpose**: Image processing and manipulation
**Commands**: `convert`, `magick`, `identify`
**Common Operations**:
- Resize: `convert input.jpg -resize 50% output.jpg`
- Format conversion: `convert input.png output.jpg`
- Batch resize: `convert *.jpg -resize 800x600 resized/`
- Add watermark: `convert input.jpg -pointsize 30 -fill white -annotate +10+50 'Watermark' output.jpg`
- Rotate: `convert input.jpg -rotate 90 output.jpg`
- Crop: `convert input.jpg -crop 800x600+100+50 output.jpg`

### 5. FFmpeg
**Purpose**: Video and audio processing
**Command**: `ffmpeg`
**Common Operations**:
- Format conversion: `ffmpeg -i input.mov -c:v libx264 output.mp4`
- Compress video: `ffmpeg -i input.mp4 -crf 23 output.mp4`
- Extract audio: `ffmpeg -i input.mp4 -vn -acodec copy output.aac`
- Video trimming: `ffmpeg -i input.mp4 -ss 00:01:00 -t 00:02:00 output.mp4`
- Add subtitles: `ffmpeg -i video.mp4 -i subtitles.srt -c copy output.mp4`
- Change resolution: `ffmpeg -i input.mp4 -vf scale=1280:720 output.mp4`

### 6. Glow
**Purpose**: Markdown renderer for terminal
**Command**: `glow`
**Usage**:
- Render file: `glow README.md`
- Pipe input: `echo "# Title" | glow`
- Pager mode: `glow -p README.md`

## Development & Collaboration Tools

### 7. GitHub CLI
**Purpose**: GitHub command-line interface
**Command**: `gh`
**Common Operations**:
- Create PR: `gh pr create --title "Title" --body "Description"`
- List issues: `gh issue list`
- Check PR status: `gh pr status`
- Clone repo: `gh repo clone [OWNER/REPO]`
- Create repo: `gh repo create [NAME] --public`
- View repo: `gh repo view [OWNER/REPO]`

### 8. ffsend
**Purpose**: Secure file sharing
**Command**: `ffsend`
**Usage**:
- Upload file: `ffsend upload document.pdf`
- Set expiration: `ffsend upload -e 1d file.txt`
- Password protect: `ffsend upload -p file.txt`

## System Enhancement Tools

### 9. nnn
**Purpose**: Terminal file manager
**Command**: `nnn`
**Key Bindings**:
- `h/j/k/l`: Navigation
- `o`: Open file
- `d`: Delete
- `r`: Rename
- `Space`: Select file
- `q`: Quit

### 10. tmux (with oh-my-tmux)
**Purpose**: Terminal multiplexer
**Command**: `tmux`
**Basic Usage**:
- New session: `tmux new -s session_name`
- Attach session: `tmux attach -t session_name`
- List sessions: `tmux list-sessions`
- Split horizontal: `Ctrl-b %`
- Split vertical: `Ctrl-b "`

### 11. macOS Application Launcher
**Purpose**: Launch GUI applications from terminal
**Methods**:
- Using open command: `open -a "Application Name"`
- Direct path: `/Applications/AppName.app/Contents/MacOS/AppName`
**Common Applications**:
- Firefox: `open -a Firefox`
- Safari: `open -a Safari`
- Chrome: `open -a "Google Chrome"`
- VS Code: `open -a "Visual Studio Code"`

## Usage Guidelines

### Security First
- Always confirm user intent before executing commands
- Validate file paths and URLs
- Use specific parameters to avoid unintended operations

### Error Handling
- Provide clear error messages
- Suggest alternative solutions
- Check command availability before execution

### Efficiency Optimization
- Choose the most appropriate tool for each task
- Use batch operations when possible
- Combine tools for complex workflows

## Common Workflow Examples

### Download and Process Video
```bash
# 1. Download video from Bilibili
yt-dlp "https://www.bilibili.com/video/BV1234567890"

# 2. Convert format
ffmpeg -i *.webm -c:v libx264 output.mp4

# 3. Compress video
ffmpeg -i output.mp4 -crf 23 compressed.mp4
```

### Batch Image Processing
```bash
# 1. Resize all images
convert *.jpg -resize 800x600 resized/

# 2. Convert format
for file in *.png; do convert "$file" "${file%.png}.jpg"; done

# 3. Add watermark
for file in *.jpg; do
    convert "$file" -pointsize 30 -fill white -annotate +10+50 'Copyright' "watermarked_$file"
done
```

### Project Development Workflow
```bash
# 1. Create project directory
mkdir new-project && cd new-project

# 2. Initialize Git
git init

# 3. Create GitHub repository
gh repo create new-project --public

# 4. Create and push initial commit
echo "# New Project" > README.md
git add . && git commit -m "Initial commit"
git push -u origin main

# 5. Create pull request
gh pr create --title "Initial setup" --body "Project initialization"
```