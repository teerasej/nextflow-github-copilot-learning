# Explore Your React TypeScript Workspace

## Project Structure Overview

Let's familiarize ourselves with the React TypeScript project structure:

```
photo-gallery/
├── public/
│   └── vite.svg
├── src/
│   ├── components/         # Reusable React components
│   ├── pages/             # Page components
│   ├── hooks/             # Custom React hooks
│   ├── services/          # API services and utilities
│   ├── types/             # TypeScript type definitions
│   ├── utils/             # Utility functions
│   ├── assets/            # Static assets (images, icons, etc.)
│   ├── App.tsx            # Main app component
│   ├── App.css            # App-specific styles
│   ├── main.tsx           # App entry point (Vite)
│   ├── index.css          # Global styles
│   └── vite-env.d.ts      # Vite environment types
├── index.html             # HTML template (Vite)
├── package.json           # Dependencies and scripts
├── tsconfig.json         # TypeScript configuration
├── tsconfig.app.json     # App-specific TypeScript config
├── tsconfig.node.json    # Node-specific TypeScript config
├── vite.config.ts        # Vite configuration
└── README.md             # Project documentation
```

## Key Files to Understand

### 1. `src/App.tsx`
The main application component where routing and global state management happens.

### 2. `src/main.tsx`
The entry point of the React application where the app is rendered to the DOM (Vite uses `main.tsx` instead of `index.tsx`).

### 3. `package.json`
Contains project dependencies, scripts, and metadata. Key scripts include:
- `npm run dev` - Start development server (Vite)
- `npm test` - Run tests
- `npm run build` - Build for production
- `npm run preview` - Preview production build locally

### 4. `vite.config.ts`
Vite configuration file that defines build settings, plugins, and development server options.

### 5. `tsconfig.json`
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
npm run dev
```

This will:
- Start the Vite development server
- Open your browser to `http://localhost:5173` (Vite's default port)
- Enable hot module replacement (HMR) for instant updates

## Next Steps

Now that you're familiar with the workspace, let's start using GitHub Copilot features:
- [Ghost text (Inline Suggestions)](./inline-suggestion.md)
