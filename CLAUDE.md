# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a decentralized todo application built on Nostr protocol. It features:

- **Frontend**: Vanilla JavaScript with Preact components loaded from CDN
- **Storage**: Decentralized storage via nosdav with DID document discovery
- **Authentication**: Nostr extension or private key login
- **Architecture**: Client-side only application with no backend

## Development Environment

Since this is a client-side only application, development is straightforward:

- Open `index.html` in a web browser to run the application
- No build process or bundling required
- All dependencies are loaded from CDN

## Core Architecture

### Storage System

The application uses a sophisticated storage layer with multiple fallback mechanisms:

1. **DID Discovery**: Fetches DID documents from `nostr.social` to find custom storage endpoints
2. **TypeRegistrations**: Supports Solid-style TypeRegistrations for multiple todo lists
3. **Fallback**: Defaults to `nosdav.net` if DID discovery fails
4. **Local Storage**: Falls back to browser localStorage if cloud storage fails

Key files:

- `storage-config.js`: Centralized storage configuration with DID discovery
- `nosdav-shim.js`: Intercepts fetch requests to add Nostr authentication headers

### Authentication Flow

The application supports two authentication methods:

1. **Nostr Extension**: Uses browser extensions like nos2x or Alby
2. **Private Key**: Direct input of 64-character hex private key
3. **URL Hash Login**: Accepts private key in URL fragment for quick access

Authentication state is managed in `navbar.js` and persisted in localStorage.

### Component Structure

- `index.js`: Main TodoApp component with state management and business logic
- `navbar.js`: Reusable navigation component with authentication
- `index.html`: Entry point with styling and app container

### Data Model

The application follows a hybrid approach combining Todo and Tracker patterns:

- **Task items**: Individual todo items with `@type: 'Task'`
- **Tracker item**: Metadata container with `@type: 'Tracker'` and `@id: '#this'`
- **JSON-LD**: Uses `@context`, `@type`, and `@id` for semantic structure

## Key Features

### Multi-List Support

- Supports multiple todo lists via TypeRegistrations
- Dynamic URI selection from dropdown
- Custom storage locations via query parameters (`?uri=...`)

### Calendar Export

- Generates ICS files for calendar import
- Converts todos to VEVENT entries with due dates

### Mind Mapping Integration

- Links to external Mindstr application for visual task management
- Generates dynamic URLs with task and relay information

### Error Handling

- Graceful degradation when cloud storage fails
- User-friendly error messages for authentication issues
- Automatic fallback to localStorage

## Common Tasks

### Testing Authentication

Use the URL hash method for quick testing:

```
index.html#<64-character-hex-private-key>
```

### Storage Debugging

Check browser console for detailed storage operation logs including:

- DID document fetching
- TypeRegistration discovery
- Storage provider selection
- Save/load operations

### Adding New Features

Follow the existing patterns:

- Use Preact with HTM for components
- Add new state to the main TodoApp component
- Include proper error handling and fallbacks
- Update the Tracker metadata when appropriate

## Dependencies

All dependencies are loaded from CDN:

- Preact 10.13.1
- HTM 3.1.1
- SweetAlert2 11
- Noble secp256k1 1.7.1
- TailwindCSS (latest)

## File Structure

- `index.html`: Main entry point and styling
- `index.js`: Core application logic and TodoApp component
- `navbar.js`: Navigation and authentication component
- `storage-config.js`: Storage discovery and URL building
- `nosdav-shim.js`: Fetch interceptor for Nostr authentication
