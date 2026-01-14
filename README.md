# Browser Annotator

A Claude Code plugin that enables interactive visual communication with web pages through drawing and annotation. Annotate web pages directly in your browser, then ask Claude questions about the highlighted content.

## Overview

This plugin enables interactive visual communication with Claude by allowing you to:
- Draw and highlight directly on web pages
- Circle or mark specific elements you want to discuss
- Ask Claude questions about the annotated content
- Get contextual answers based on what you've highlighted

## Features

### Dual Browser Support

**annotate-ask** - For Claude in Chrome extension users
- Interactive annotation overlay on active Chrome tabs
- Real-time drawing with pen tool
- Instant screenshot capture with annotations

**annotate-ask-playwright** - For Playwright MCP server users
- Annotation support for automated browser sessions
- Compatible with browser testing workflows
- Same annotation interface as Chrome version

### Annotation Tools

- **Pen Tool**: Draw freehand to highlight areas of interest
- **Clear Button**: Reset annotations and start over
- **Done Button**: Capture annotated screenshot for Claude analysis

## Installation

Install the plugin using Claude Code's plugin manager:

```bash
claude /plugin https://github.com/coin2028/browser-annotator
```

This will install the browser annotator plugin and make the `/annotate` and `/annotate-playwright` commands available.

### Plugin Structure

The plugin follows the Claude Code plugin specification:
- `/.claude-plugin/marketplace.json` - Plugin metadata and configuration
- `/commands/` - Reusable slash command implementations
- `/skills/` - Skill definitions that reference the commands

## Requirements

- **Claude Code CLI** - The official Claude command-line interface
- **For /annotate:** Claude in Chrome browser extension installed and active
- **For /annotate-playwright:** Playwright MCP server configured in Claude Code

## Usage

### Basic Workflow

1. **Navigate to a web page** using Claude in Chrome or Playwright

2. **Invoke the annotation command**:
   ```
   /annotate
   ```
   or
   ```
   /annotate-playwright
   ```

3. **Annotation interface appears** - A toolbar with three buttons overlays the page:
   - **Pen** - Draw freehand to highlight areas (active by default)
   - **Clear** - Erase all annotations and start over
   - **Done** - Finish annotating and capture screenshot

4. **Draw on the page** - Click and drag to circle, underline, or mark specific elements

5. **Click Done** - Claude captures the annotated screenshot

6. **Ask your question** - After the screenshot is captured, ask Claude about the highlighted areas:
   ```
   What does this login form do?
   How does the navigation menu work?
   Why is this button disabled?
   ```

7. **Get answers** - Claude analyzes the annotated screenshot and responds based on the visual context

## Example Use Cases

- **UI/UX Review:** Highlight confusing interface elements and ask for explanations
- **Web Development:** Mark specific components and ask about their implementation
- **Bug Reports:** Circle problematic areas and describe the issue
- **Learning:** Highlight unfamiliar concepts on documentation pages for clarification
- **Accessibility:** Mark elements and ask about their accessibility features

## Technical Details

### Annotation Overlay

The commands inject a lightweight JavaScript overlay that:
- Uses HTML5 Canvas for drawing
- Applies high z-index (999999+) to appear above page content
- Captures mouse events for pen drawing
- Stores annotation paths for redrawing
- Cleans up automatically after completion

### Screenshot Capture

- Screenshots are captured directly into Claude Code's context
- Includes both page content and your red pen annotations
- Original page remains unmodified
- Overlay is removed after screenshot capture

### How It Works

1. JavaScript overlay is injected into the active browser tab
2. User draws annotations using HTML5 Canvas
3. Click "Done" sets a completion flag
4. Claude polls for completion, then captures screenshot
5. Screenshot with annotations is analyzed by Claude
6. Overlay is removed, page returns to normal

## License

MIT License - See LICENSE file for details

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Author

Quan Gan (coin2028@hotmail.com)

## Version

1.0.0

## Project Links

- **Repository:** https://github.com/coin2028/browser-annotator
- **Issues:** https://github.com/coin2028/browser-annotator/issues
- **Claude in Chrome:** https://claude.ai/chrome
- **Claude Code:** https://claude.com/code

---

*A Claude Code plugin for visual browser interaction*
