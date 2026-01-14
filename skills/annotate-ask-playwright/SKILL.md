---
name: annotate-ask-playwright
description: INVOKE THIS SKILL when users ask questions about specific visual UI elements (e.g., "What is this button?", "What does this section do?", "Can you change this element's color?") or when they want to identify/discuss particular page components, and the agent lacks information to infer which part of the web page the user refers to. This skill lets users draw/circle elements instead of describing them.
license: MIT
compatibility: Designed for Claude Code with Playwright MCP server
metadata:
  author: Quan Gan
  version: "1.0.0"
  category: browser-tools
---

# Browser Annotation with Question (Playwright Version)

This skill helps users annotate the current browser page (using Playwright) and answer questions about what they annotated.

**User's question:** $ARGUMENTS

## Step 1: Capture Annotated Screenshot

Use the Skill tool to run the `/annotate-playwright` command. This will:
- Inject an annotation overlay on the current browser page
- Let the user draw/highlight areas of interest
- Capture a screenshot with the annotations
- Save the screenshot to the annotations directory
- Bring the screenshot into Claude Code's context

## Step 2: Answer the User's Question

After the `/annotate-playwright` command completes, examine the annotated screenshot now in context. Look at the red annotations the user drew to understand which parts of the page they're referring to.

Based on what you see in the annotated screenshot, answer the user's question: **$ARGUMENTS**

Focus your answer on:
- The areas the user highlighted/circled with red annotations
- What those elements are (buttons, text, images, etc.)
- Any specific details relevant to their question

Provide a clear, helpful response based on the visual annotations.
