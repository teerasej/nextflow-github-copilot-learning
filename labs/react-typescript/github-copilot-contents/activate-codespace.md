# Activating GitHub Codespace for React TypeScript

## Prerequisites

1. GitHub account with Copilot access
2. Access to GitHub Codespaces

## Steps

1. **Fork the Repository**
   - Go to the React TypeScript photo gallery repository
   - Click on the "Fork" button in the top right corner
   - Select your GitHub account as the destination

2. **Create a Codespace**
   - In your forked repository, click on the green "Code" button
   - Select the "Codespaces" tab
   - Click "Create codespace on main"

3. **Wait for Setup**
   - GitHub will create a cloud-based development environment
   - This includes VS Code in the browser with all necessary extensions
   - Node.js, npm, and other dependencies will be pre-installed

4. **Verify Setup**
   - Once the codespace loads, open the terminal
   - Run `node --version` to verify Node.js installation
   - Run `npm --version` to verify npm installation
   - Check that GitHub Copilot extension is installed and active

5. **Install Project Dependencies**
   ```bash
   cd photo-gallery
   npm install
   ```

6. **Start Development Server**
   ```bash
   npm run dev
   ```

## Alternative: Local Development

If you prefer to work locally:

1. Clone the forked repository
2. Install Node.js 18+ from [nodejs.org](https://nodejs.org)
3. Install VS Code with GitHub Copilot extension
4. Navigate to the photo-gallery directory: `cd photo-gallery`
5. Run `npm install` in the project directory
6. Run `npm run dev` to start the development server

## Next Steps

Once your environment is ready, proceed to [Explore your workspace](./explore-workspace.md).
