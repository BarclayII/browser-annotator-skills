# Browser Annotator Skills

A collection of browser annotation skills for Claude Code that allow users to draw, highlight, and annotate web pages, then ask Claude questions about the annotated content.

## Overview

These skills enable interactive visual communication with Claude by allowing you to:
- Draw and highlight directly on web pages
- Circle or mark specific elements you want to discuss
- Ask Claude questions about the annotated content
- Get contextual answers based on what you've highlighted

## Skills Included

### 1. annotate-ask (Claude in Chrome)

**Description:** Interactive browser annotation for Claude in Chrome extension.

**Compatibility:** Requires Claude Code with Claude in Chrome extension

**Usage:**
```
/annotate-ask What is this button for?
```

This skill:
- Injects an annotation overlay on the current browser tab
- Provides drawing tools (pen, clear, done buttons)
- Captures screenshots with your annotations
- Analyzes the annotated content to answer your questions

### 2. annotate-ask-playwright (Playwright)

**Description:** Interactive browser annotation using Playwright MCP server.

**Compatibility:** Requires Claude Code with Playwright MCP server configured

**Usage:**
```
/annotate-ask-playwright How does this form work?
```

This skill:
- Works with Playwright-controlled browser sessions
- Provides the same annotation interface as the Chrome version
- Supports automated browser testing workflows with visual feedback

## Installation

### Option 1: Install from GitHub

1. Clone this repository or add it to your Claude Code skills directory:
   ```bash
   git clone https://github.com/coin2028/browser-annotator-skills.git
   ```

2. The skills will be automatically available in Claude Code

### Option 2: Manual Installation

1. Copy the `skills/` directory to your `.claude/skills/` folder
2. Or reference this repository in your Claude Code configuration

## Requirements

- **Claude Code CLI** - The official Claude command-line interface
- **For annotate-ask:** Claude in Chrome browser extension
- **For annotate-ask-playwright:** Playwright MCP server configured in Claude Code

## How It Works

1. **Invoke the skill** with your question:
   ```
   /annotate-ask What does this section do?
   ```

2. **Draw on the page** - An annotation toolbar appears with:
   - Pen tool (active by default)
   - Clear button (to reset drawings)
   - Done button (when finished)

3. **Annotate freely** - Draw circles, arrows, or highlights around the elements you want to discuss

4. **Click Done** - Claude captures the annotated page and analyzes it

5. **Get answers** - Claude responds based on the specific areas you highlighted

## Example Use Cases

- **UI/UX Review:** Highlight confusing interface elements and ask for explanations
- **Web Development:** Mark specific components and ask about their implementation
- **Bug Reports:** Circle problematic areas and describe the issue
- **Learning:** Highlight unfamiliar concepts on documentation pages for clarification
- **Accessibility:** Mark elements and ask about their accessibility features

## Technical Details

### Annotation Overlay

The skills inject a lightweight JavaScript overlay that:
- Uses HTML5 Canvas for drawing
- Applies high z-index to appear above page content
- Captures mouse events for pen drawing
- Stores annotation paths for redrawing
- Cleans up after completion

### Screenshot Capture

- Annotations are saved to `./annotations/` directory
- Filenames use timestamp format: `annotated-YYYYMMDD-HHMMSS.png`
- Screenshots include both page content and annotations
- Original page remains unmodified

## License

MIT License - See LICENSE file for details

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Author

Quan Gan (coin2028@hotmail.com)

## Version

1.0.0

## Related Projects

- [Anthropic Skills Repository](https://github.com/anthropics/skills)
- [Claude in Chrome Extension](https://claude.ai/chrome)
- [Agent Skills Specification](https://agentskills.io/specification)

---

*Built with the Agent Skills framework for Claude Code*
