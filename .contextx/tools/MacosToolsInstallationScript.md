#!/bin/bash

# Claude Code Tools Installation Script for macOS
# This script installs essential command-line tools for Claude Code automation

set -e  # Exit on any error

echo "🍎 Claude Code Tools Installation for macOS"
echo "============================================="
echo ""

# Color codes for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Function to print colored output
print_status() {
    echo -e "${BLUE}[INFO]${NC} $1"
}

print_success() {
    echo -e "${GREEN}[SUCCESS]${NC} $1"
}

print_warning() {
    echo -e "${YELLOW}[WARNING]${NC} $1"
}

print_error() {
    echo -e "${RED}[ERROR]${NC} $1"
}

# Check if running on macOS
if [[ "$OSTYPE" != "darwin"* ]]; then
    print_error "This script is designed for macOS only."
    exit 1
fi

# Check for Homebrew installation
print_status "Checking for Homebrew..."
if ! command -v brew &> /dev/null; then
    print_status "Installing Homebrew..."
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    
    # Add Homebrew to PATH for Apple Silicon Macs
    if [[ $(uname -m) == "arm64" ]]; then
        echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
        eval "$(/opt/homebrew/bin/brew shellenv)"
    fi
    
    print_success "Homebrew installed successfully"
else
    print_success "Homebrew is already installed"
fi

# Update Homebrew
print_status "Updating Homebrew..."
brew update

# Function to install or check tool
install_tool() {
    local tool_name=$1
    local brew_name=${2:-$tool_name}
    
    if brew list "$brew_name" &>/dev/null; then
        print_success "$tool_name is already installed"
    else
        print_status "Installing $tool_name..."
        brew install "$brew_name"
        print_success "$tool_name installed successfully"
    fi
}

# Install Content Acquisition Tools
echo ""
print_status "Installing Content Acquisition Tools..."
echo "----------------------------------------"

# Check for Python3 first (required for yt-dlp)
if ! command -v python3 &> /dev/null; then
    print_status "Installing Python3..."
    brew install python
    print_success "Python3 installed"
else
    print_success "Python3 is already available"
fi

# Install yt-dlp via pip
print_status "Installing yt-dlp..."
python3 -m pip install -U "yt-dlp[default]"
print_success "yt-dlp installed"

# Install other acquisition tools
install_tool "aria2"
install_tool "s-search"

# Install Content Processing Tools
echo ""
print_status "Installing Content Processing Tools..."
echo "--------------------------------------"

install_tool "ImageMagick" "imagemagick"
install_tool "FFmpeg" "ffmpeg"
install_tool "Glow" "glow"

# Install Development & Collaboration Tools
echo ""
print_status "Installing Development & Collaboration Tools..."
echo "-----------------------------------------------"

install_tool "GitHub CLI" "gh"
install_tool "ffsend"

# Install System Enhancement Tools
echo ""
print_status "Installing System Enhancement Tools..."
echo "-------------------------------------"

install_tool "nnn"
install_tool "tmux"

# Install Oh My Zsh if not present
if [ ! -d "$HOME/.oh-my-zsh" ]; then
    print_status "Installing Oh My Zsh..."
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
    print_success "Oh My Zsh installed"
else
    print_success "Oh My Zsh is already installed"
fi

# Install Oh My Tmux configuration
if [ ! -f "$HOME/.tmux.conf" ]; then
    print_status "Installing Oh My Tmux..."
    cd ~
    git clone https://github.com/gpakosz/.tmux.git
    ln -s -f .tmux/.tmux.conf