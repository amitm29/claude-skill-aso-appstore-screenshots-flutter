# ASO App Store Screenshots — Flutter

A Claude Code skill that generates high-converting **App Store** screenshots for your **Flutter iOS** app. It analyzes your Dart codebase, identifies core benefits, and creates professional screenshot images using AI.

> This is the Flutter variant. For native iOS apps (Swift/Objective-C, Xcode project), use the original [aso-appstore-screenshots](https://github.com/adamlyttleapps/claude-skill-aso-appstore-screenshots) skill instead.

## What It Does

1. **Benefit Discovery** — Reads `pubspec.yaml`, your `lib/` directory, and Flutter theme files to identify the 3-5 core benefits that drive downloads
2. **Screenshot Pairing** — Reviews your iOS Simulator screenshots (taken via Cmd+S or `flutter screenshot`), rates them, and pairs each with the best benefit
3. **Generation** — Creates polished App Store screenshots using a two-stage process: deterministic scaffolding (compose.py) + AI enhancement (Nano Banana Pro via Gemini MCP)
4. **Showcase** — Generates a preview image with all screenshots side-by-side

## Installation

### 1. Add the skill to Claude Code

```bash
claude install-skill github.com/amitm29/claude-skill-aso-appstore-screenshots-flutter
```

### 2. Install Python dependencies

```bash
pip install Pillow
```

### 3. Font requirement

The skill uses **SF Pro Display Black** for headline text. On macOS, install it from [Apple's developer fonts](https://developer.apple.com/fonts/). The expected path is:

```
/Library/Fonts/SF-Pro-Display-Black.otf
```

### 4. Set up Gemini MCP (for AI enhancement)

The generation phase requires [@houtini/gemini-mcp](https://www.npmjs.com/package/@houtini/gemini-mcp) to be configured as an MCP server in Claude Code:

```bash
npm install -g @houtini/gemini-mcp
```

Then add it to your Claude Code MCP config (`~/.claude/settings.json` or project `.mcp.json`).

### 5. Flutter SDK (for capturing simulator screenshots)

You'll need the [Flutter SDK](https://docs.flutter.dev/get-started/install) installed and the iOS Simulator working (`flutter run` should successfully launch your app on a simulator). Screenshots can be taken via Cmd+S in the Simulator window OR via `flutter screenshot --out path/to/file.png` while the app is running.

## Usage

From within your Flutter app's project directory (the one containing `pubspec.yaml`), run:

```
/aso-appstore-screenshots-flutter
```

The skill will guide you through each phase interactively. Progress is saved to Claude Code's memory system, so you can resume across conversations.

## How It Works

### Scaffold → Enhance Pipeline

Rather than generating screenshots from scratch (which produces inconsistent results), the skill uses a two-stage approach:

1. **compose.py** creates a deterministic scaffold with exact text positioning, device frame, and your simulator screenshot composited inside
2. **Nano Banana Pro** (via Gemini MCP) enhances the scaffold — adding a photorealistic device frame, breakout elements, and visual polish

This ensures consistent layout across all screenshots while letting AI handle the creative enhancement.

### Output

Screenshots are saved to a `screenshots/` directory in your project:

```
screenshots/
  01-benefit-slug/          ← working versions
    scaffold.png            ← deterministic compose.py output
    v1.png, v2.png, v3.png  ← AI-enhanced versions
    v1-resized.png, ...     ← cropped to App Store dimensions
  final/                    ← approved screenshots, ready to upload
    01-benefit-slug.png
    02-benefit-slug.png
  showcase.png              ← preview image with all screenshots
```

The `final/` folder contains App Store-ready screenshots at exact Apple dimensions (default: 1290×2796px for iPhone 6.7").

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | The skill prompt — defines the multi-phase workflow |
| `compose.py` | Deterministic scaffold generator (Pillow-based) |
| `generate_frame.py` | Generates the device frame template |
| `showcase.py` | Generates the side-by-side showcase image |
| `assets/device_frame.png` | Pre-rendered iPhone device frame template |

## License

MIT
