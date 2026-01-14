# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Workflow

1. **Develop one small piece at a time** - Each feature is implemented as a small, testable unit
2. **Write unit tests alongside code** - Every component gets tests before moving on
3. **Log comprehensively** - Logging is required for complex functions to facilitate debugging and troubleshooting
4. **Code review after each feature** - Request review feedback by launching reviewer subagents, and iterate before proceeding
5. **Integrate incrementally** - Components are integrated only after individual testing passes
6. **Consult the web when stuck** - When fixing an issue, if the failure persists (e.g. after 3 attempts), consider searching the web for additional information.

## Build and Test Commands

*(To be added as components are developed)*

## Project Structure

- `/.kb/` - Knowledge base with research findings
