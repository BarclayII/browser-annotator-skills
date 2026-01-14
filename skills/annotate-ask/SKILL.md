---
name: annotate-ask
description: Use this skill if the user asks a question or make a request on specific parts of the current browser page, or if the user indicates that they would like to draw/highlight on the current browser page (using Claude in Chrome).
license: MIT
compatibility: Designed for Claude Code with Claude in Chrome extension
metadata:
  author: Quan Gan
  version: "1.0.0"
  category: browser-tools
---

# Browser Annotation with Question (Chrome Version)

You are helping the user annotate the current browser page and answer a question about what they annotated.

**User's question:** $ARGUMENTS

## Important: Get Tab Context First

Before starting, ALWAYS call `mcp__claude-in-chrome__tabs_context_mcp` to get the current tab information. You MUST have a valid tabId to proceed.

## Step 1: Inject the Annotation Overlay

Use `mcp__claude-in-chrome__javascript_tool` to inject the annotation interface:

**Parameters:**
- `action`: "javascript_exec"
- `tabId`: (from tabs_context_mcp)
- `text`: The JavaScript code below (as a string)

**JavaScript code:**
```javascript
(() => {
  // Reset the done flag for a new annotation session
  window.__annotationsDone = false;

  if (document.getElementById('annotation-overlay')) {
    return 'Annotation overlay already exists';
  }

  const overlay = document.createElement('div');
  overlay.id = 'annotation-overlay';
  overlay.innerHTML = `
    <style>
      #annotation-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        z-index: 999999;
        pointer-events: none;
      }
      #annotation-canvas {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        cursor: crosshair;
        pointer-events: auto;
      }
      #annotation-toolbar {
        position: fixed;
        top: 10px;
        right: 10px;
        background: #333;
        padding: 10px;
        border-radius: 8px;
        display: flex;
        gap: 8px;
        z-index: 1000000;
        pointer-events: auto;
        font-family: system-ui, sans-serif;
      }
      #annotation-toolbar button {
        padding: 8px 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;
        font-weight: 500;
      }
      .btn-tool {
        background: #007bff;
        color: white;
      }
      .btn-clear {
        background: #6c757d;
        color: white;
      }
      .btn-done {
        background: #28a745;
        color: white;
      }
      #annotation-status {
        position: fixed;
        bottom: 20px;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.8);
        color: white;
        padding: 12px 24px;
        border-radius: 8px;
        font-family: system-ui, sans-serif;
        font-size: 14px;
        z-index: 1000000;
        pointer-events: none;
      }
    </style>
    <canvas id="annotation-canvas"></canvas>
    <div id="annotation-toolbar">
      <button class="btn-tool">Pen</button>
      <button class="btn-clear">Clear</button>
      <button class="btn-done">Done</button>
    </div>
    <div id="annotation-status">Draw on the page, then click Done</div>
  `;
  document.body.appendChild(overlay);

  const canvas = document.getElementById('annotation-canvas');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  let isDrawing = false;
  let paths = [];
  let currentPath = [];

  ctx.strokeStyle = '#ff0000';
  ctx.lineWidth = 3;
  ctx.lineCap = 'round';
  ctx.lineJoin = 'round';

  function redraw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    paths.forEach(path => {
      if (path.length < 2) return;
      ctx.beginPath();
      ctx.moveTo(path[0].x, path[0].y);
      path.forEach(point => ctx.lineTo(point.x, point.y));
      ctx.stroke();
    });
  }

  canvas.onmousedown = (e) => {
    isDrawing = true;
    currentPath = [{x: e.clientX, y: e.clientY}];
  };

  canvas.onmousemove = (e) => {
    if (!isDrawing) return;
    currentPath.push({x: e.clientX, y: e.clientY});
    redraw();
    ctx.beginPath();
    ctx.moveTo(currentPath[0].x, currentPath[0].y);
    currentPath.forEach(point => ctx.lineTo(point.x, point.y));
    ctx.stroke();
  };

  canvas.onmouseup = () => {
    if (!isDrawing) return;
    isDrawing = false;
    if (currentPath.length > 1) {
      paths.push(currentPath);
    }
    currentPath = [];
    redraw();
  };

  document.querySelector('.btn-clear').onclick = () => {
    paths = [];
    currentPath = [];
    redraw();
  };

  document.querySelector('.btn-done').onclick = () => {
    document.getElementById('annotation-toolbar').style.display = 'none';
    document.getElementById('annotation-status').textContent = 'Annotations complete. Please switch back to Claude Code to move on.';
    canvas.style.pointerEvents = 'none';
    window.__annotationsDone = true;
  };

  return 'Annotation overlay injected. User can now draw.';
})()
```

## Step 2: Inform the User

Tell the user: "Annotation tools are ready. Draw on the page to highlight what you want me to analyze, then click **Done**."

## Step 3: Wait for User to Click Done

Poll the page to check if the user clicked Done. Use `mcp__claude-in-chrome__javascript_tool` repeatedly with short delays:

**Check if done (use this JavaScript):**
```javascript
window.__annotationsDone === true
```

If it returns `false` or `undefined`, wait 2-3 seconds and check again. Repeat until it returns `true` or the user tells you to proceed anyway.

You can use the `mcp__claude-in-chrome__computer` tool with `action: "wait"` and `duration: 2` to wait between checks.

## Step 4: Take Screenshot

Use `mcp__claude-in-chrome__computer` with:
- `action`: "screenshot"
- `tabId`: (same tabId from earlier)

The screenshot will be returned in the tool result and you'll be able to see it immediately.

## Step 5: Remove the Overlay

Use `mcp__claude-in-chrome__javascript_tool` to clean up:

```javascript
(() => {
  const overlay = document.getElementById('annotation-overlay');
  if (overlay) overlay.remove();
  return 'Overlay removed';
})()
```

## Step 6: Analyze the Screenshot

Look at the screenshot you just captured (the tool result will show the image). Examine the red annotations the user drew.

## Step 7: Answer the User's Question

Based on what you see in the annotated screenshot, answer the user's question: **$ARGUMENTS**

Focus your answer on:
- The areas the user highlighted/circled with red annotations
- What those elements are (buttons, text, images, etc.)
- Any specific details relevant to their question

Provide a clear, helpful response based on the visual annotations.
