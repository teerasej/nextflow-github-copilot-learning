# Explore Your React TypeScript Workspace

## Project Structure Overview

Let's familiarize ourselves with the React TypeScript project structure:

```
photo-gallery/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── components/         # Reusable React components
│   ├── pages/             # Page components
│   ├── hooks/             # Custom React hooks
│   ├── services/          # API services and utilities
│   ├── types/             # TypeScript type definitions
│   ├── utils/             # Utility functions
│   ├── App.tsx            # Main app component
│   ├── App.css            # Global styles
│   ├── index.tsx          # App entry point
│   └── index.css          # Base styles
├── package.json           # Dependencies and scripts
├── tsconfig.json         # TypeScript configuration
└── README.md             # Project documentation
```

## Key Files to Understand

### 1. `src/App.tsx`
The main application component where routing and global state management happens.

### 2. `src/index.tsx`
The entry point of the React application where the app is rendered to the DOM.

### 3. `package.json`
Contains project dependencies, scripts, and metadata. Key scripts include:
- `npm start` - Start development server
- `npm test` - Run tests
- `npm run build` - Build for production

### 4. `tsconfig.json`
TypeScript configuration file that defines compilation options and project structure.

## Exploring with VS Code

1. **File Explorer**: Use the left sidebar to navigate through files
2. **Terminal**: Access via `Terminal → New Terminal` or `` Ctrl+` ``
3. **Command Palette**: Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac) for quick actions
4. **Search**: Use `Ctrl+Shift+F` to search across all files

## GitHub Copilot Integration

Look for these indicators that Copilot is active:
- Copilot icon in the VS Code status bar
- Ghost text suggestions as you type
- Copilot Chat panel (accessible via sidebar)

## Development Server

Start the development server to see your application:

```bash
npm start
```

This will:
- Start the React development server
- Open your browser to `http://localhost:3000`
- Enable hot reloading for instant updates

## Next Steps

Now that you're familiar with the workspace, let's start using GitHub Copilot features:
- [Ghost text (Inline Suggestions)](./inline-suggestion.md)
