# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Exiled Exchange 2 is a Path of Exile 2 overlay application built with Electron. It provides price checking and other features for the game. This is a fork of Awakened PoE Trade, adapted for PoE2.

## Architecture

The application consists of two main parts that depend on each other:

1. **Main Process** (Electron backend)
   - Handles keyboard shortcuts via uiohook-napi
   - Manages overlay window and game window detection
   - Provides IPC server for renderer communication
   - Handles auto-updates and system tray integration

2. **Renderer Process** (Vue.js frontend)
   - Vue 3 + TypeScript UI application
   - Uses Vite for development and building
   - TailwindCSS for styling
   - i18n support via vue-i18n

## Development Workflow

### Running in Development

Two terminals are required:

```bash
# Terminal 1 - Start renderer
cd renderer
npm install
npm run make-index-files
npm run dev

# Terminal 2 - Start main process
cd main
npm install
npm run dev
```

### Common Commands

**Renderer (Vue.js frontend):**
- `npm run dev` - Start development server with hot reload
- `npm run build` - Build for production
- `npm run lint` - Run ESLint
- `npm run lint-fix` - Auto-fix linting issues
- `npm run format` - Format code with Prettier
- `npm run make-index-files` - Generate static asset indexes
- `npm run test` - Run tests with Vitest

**Main (Electron backend):**
- `npm run dev` - Start in development mode
- `npm run build` - Build TypeScript to JavaScript
- `npm run package` - Package application with electron-builder
- `npm run lint` - Run ESLint
- `npm run fix` - Auto-fix linting issues
- `npm run format` - Format code with Prettier

### Pre-commit Hooks

The project uses pre-commit hooks that:
- Check version consistency across files
- Auto-lint and format code in both main and renderer directories
- Run standard checks (trailing whitespace, large files, etc.)

### Building and Packaging

```bash
# Build both parts
cd renderer && npm run build
cd ../main && npm run build

# Package the application
npm run package
```

For distribution builds, use:
```bash
CSC_NAME="Certificate name in Keychain" npm run package
```

## Key Dependencies

**Renderer:**
- Vue 3.2.37 with Composition API
- Vite for tooling
- ApexCharts for data visualization
- Tippy.js for tooltips
- Luxon for date handling

**Main:**
- Electron 33.2.1
- uiohook-napi for global keyboard shortcuts
- electron-overlay-window for overlay functionality
- electron-updater for auto-updates

## Important Notes

- The application requires accessibility permissions on macOS for overlay functionality
- Game detection relies on finding the Path of Exile 2 window
- Configuration is stored in `%APPDATA%\exiled-exchange-2` on Windows
- The app can migrate settings from Awakened PoE Trade by copying the `apt-data` directory

## Testing

- Renderer uses Vitest for unit testing
- Run tests with `npm run test` in the renderer directory
- Test files are located in `renderer/specs/`

## Release Process

Version is managed in `main/package.json`. The release process involves:
1. Bumping version in main/package.json
2. Updating dependencies in both renderer and main
3. Building both parts
4. Creating a git tag
5. Publishing through GitHub releases