# 📝 Decentralized Todo App

A modern, decentralized todo application built on the **Nostr protocol** with advanced cloud storage capabilities. Manage your tasks with complete data ownership and privacy, featuring seamless synchronization across devices through decentralized storage providers.

![Todo App Preview](https://img.shields.io/badge/Built%20with-Nostr-purple?style=for-the-badge) ![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge) ![Status](https://img.shields.io/badge/Status-Production%20Ready-green?style=for-the-badge)

## ✨ Key Features

### 🔐 **Decentralized Authentication**

- **Nostr Extension Support**: Compatible with browser extensions like nos2x and Alby
- **Private Key Login**: Direct private key authentication for advanced users
- **URL Hash Login**: Quick access via URL fragment for testing and development

### ☁️ **Advanced Storage Architecture**

- **DID Document Discovery**: Automatically discovers custom storage endpoints from nostr.social
- **TypeRegistrations**: Supports Solid-style TypeRegistrations for multiple todo lists
- **Smart Fallbacks**: Gracefully degrades from custom storage → nosdav.net → localStorage
- **Multiple Storage Locations**: Support for multiple todo lists with different storage providers

### 📋 **Rich Task Management**

- **Priority System**: Mark tasks as high priority with visual indicators
- **Task Completion**: Toggle task completion with persistent state
- **Dynamic List Titles**: Editable list titles with auto-save functionality
- **JSON-LD Schema**: Semantic task structure with `@type: 'Task'` and tracker metadata

### 📅 **Calendar Integration**

- **ICS Export**: Generate calendar files for import into any calendar application
- **Due Date Support**: Convert todos to calendar events with proper scheduling
- **VEVENT Format**: Standards-compliant calendar export

### 🧠 **Mind Mapping Integration**

- **Mindstr Integration**: Links to external mind mapping application
- **Visual Task Management**: Convert linear todo lists to visual mind maps
- **Dynamic URL Generation**: Automatic relay and task information passing

## 🚀 Quick Start

### Option 1: Direct Browser Usage

1. Download or clone this repository
2. Open `index.html` in any modern web browser
3. Login with your Nostr extension or private key
4. Start managing your decentralized todos!

### Option 2: GitHub Pages (Live Demo)

Visit the live demo: [Todo App Demo](https://your-username.github.io/todo)

### Option 3: Custom Storage Location

Access with specific storage URI:

```
todo.html?uri=https://your-storage-provider.com/path/to/todos.json
```

## 🔧 Technology Stack

- **Frontend Framework**: Vanilla JavaScript with Preact components
- **UI Library**: Preact 10.13.1 with HTM for JSX-like syntax
- **Styling**: TailwindCSS with custom CSS variables
- **Storage Protocol**: Nosdav (Nostr-based WebDAV)
- **Authentication**: Nostr protocol with secp256k1 cryptography
- **Calendar Export**: Custom ICS generation
- **Dependencies**: All loaded from CDN (no build process required)

## 🏗️ Architecture Overview

### Storage Layer

```
DID Discovery → TypeRegistrations → Custom Storage → nosdav.net → localStorage
```

1. **DID Document Fetching**: Queries nostr.social for custom storage endpoints
2. **TypeRegistration Discovery**: Finds registered todo lists via publicTypeIndex.json
3. **Smart URL Building**: Constructs storage URLs with proper authentication
4. **Graceful Fallbacks**: Multiple fallback levels ensure data availability

### Authentication Flow

```
URL Hash → Nostr Extension → Private Key Input → Storage Access
```

### Component Structure

- `index.js`: Main TodoApp component with state management
- `navbar.js`: Authentication and navigation component
- `storage-config.js`: Centralized storage configuration with DID discovery
- `nosdav-shim.js`: Fetch interceptor for Nostr authentication headers

## 📁 File Structure

```
todo/
├── index.html             # Main entry point with styling
├── index.js               # Core application logic (1275 lines)
├── navbar.js              # Navigation and authentication (447 lines)
├── storage-config.js      # Storage discovery and URL building (128 lines)
├── nosdav-shim.js         # Fetch interceptor for Nostr auth (71 lines)
├── package.json           # Package configuration
├── CLAUDE.md              # Development documentation
├── LICENSE                # MIT License
└── README.md              # This file
```

## 🔐 Authentication Methods

### 1. Nostr Browser Extension

The recommended method for regular users:

- Install a Nostr extension (nos2x, Alby, etc.)
- Click "Sign in with Nostr extension"
- Grant permission for the app to access your identity

### 2. Private Key Login

For advanced users who manage their own keys:

- Enter your 64-character hex private key
- Key is stored locally and used for authentication
- **Warning**: Only use on trusted devices

### 3. URL Hash Login (Testing)

Quick access for development and testing:

```
todo.html#your-64-character-private-key-here
```

The hash is automatically removed from the URL for privacy.

## 🗄️ Storage Configuration

### DID-Based Storage Discovery

The app automatically discovers your custom storage provider:

1. Fetches your DID document from `nostr.social/.well-known/did/nostr/{pubkey}.json`
2. Looks for `Storage` service type in the service array
3. Uses the `serviceEndpoint` as your storage root
4. Falls back to `nosdav.net` if no custom storage is found

### Multiple Todo Lists

Support for multiple todo lists via TypeRegistrations:

1. Create `publicTypeIndex.json` in your storage root
2. Register Tracker instances with different storage locations
3. App automatically discovers and provides dropdown selection
4. Each list can use different storage providers

Example TypeRegistration:

```json
[
  {
    "type": "TypeRegistration",
    "forClass": "Tracker",
    "instance": "../work/todos.json"
  }
]
```

## 📊 Data Format

### Task Structure

```json
{
  "@context": {
    "Task": "https://schema.org/Task",
    "name": "https://schema.org/name",
    "completed": "https://schema.org/completed"
  },
  "@type": "Task",
  "@id": "#task-1",
  "name": "Complete project documentation",
  "completed": false,
  "priority": true,
  "created": "2025-01-27T10:30:00Z"
}
```

### Tracker Metadata

```json
{
  "@context": {
    "Tracker": "https://schema.org/ItemList",
    "name": "https://schema.org/name"
  },
  "@type": "Tracker",
  "@id": "#this",
  "name": "My Todo List"
}
```

## 🔄 Development Workflow

### No Build Process Required

- Open `index.html` directly in browser
- All dependencies loaded from CDN
- Changes are immediately visible on refresh

### Storage Debugging

Check browser console for detailed logs:

- DID document fetching
- TypeRegistration discovery
- Storage provider selection
- Save/load operations

### Adding New Features

Follow the existing patterns:

- Use Preact with HTM for components
- Add state to the main TodoApp component
- Include proper error handling and fallbacks
- Update Tracker metadata when appropriate

## 📅 Calendar Export

Export your todos to any calendar application:

1. Click the "Export to Calendar" button
2. Save the generated `.ics` file
3. Import into your preferred calendar app
4. Todos appear as events with proper dates

The export includes:

- Task names as event titles
- Creation dates as event dates
- Priority tasks marked with special formatting
- All-day events for flexible scheduling

## 🧠 Mind Mapping Integration

Convert your linear todo list to a visual mind map:

1. Click the "Open in Mind Map" link
2. Launches external Mindstr application
3. Automatically passes task and relay information
4. Provides visual task organization and relationships

## 🛠️ Troubleshooting

### Storage Issues

- **Error saving**: Check your Nostr extension connection
- **Data not syncing**: Verify your storage provider is accessible
- **Multiple lists not appearing**: Check your `publicTypeIndex.json` format

### Authentication Problems

- **Extension not detected**: Install a compatible Nostr extension
- **Private key errors**: Ensure key is exactly 64 hex characters
- **Login fails**: Clear browser cache and try again

### Performance Optimization

- **Slow loading**: DID documents are cached for 5 days
- **Storage timeouts**: Falls back to localStorage automatically
- **Memory usage**: Large todo lists are handled efficiently

## 🤝 Contributing

This project welcomes contributions! Areas for improvement:

- Additional storage provider integrations
- Enhanced UI/UX features
- Mobile responsiveness improvements
- Additional export formats
- Integration with other Nostr applications

## 📜 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🔗 Links

- [Nostr Protocol](https://nostr.com)
- [Nostr Extensions](https://nostrapps.github.io/extensions/)
- [nosdav Storage](https://nosdav.net)
- [Preact Framework](https://preactjs.com)

---

**Built with ❤️ on the decentralized web using Nostr protocol**
