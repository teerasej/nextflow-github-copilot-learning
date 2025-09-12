# Exercise: Create a Simple Single Page Application with GitHub Copilot Custom Instructions

## Objective

Build a basic single page application (SPA) using your preferred frontend framework (e.g., React, Vue, Angular) and leverage GitHub Copilot's custom instructions to assist your development process.

## Steps

### 1. Set Up the Project
1. Initialize a new project directory or fork this [repo](https://github.com/teerasej/nextflow-website-github-instruction) https://github.com/teerasej/nextflow-website-github-instruction
2. Create the codespace to work on this repo


### 2. Configure GitHub Copilot Custom Instructions
1. Open your IDE.
2. At the root of your project, create a `.github/instructions` directory.
3. In this directory, create a `my-instructions.instructions.md` file. (It doesn't have to be named this way, but it should have a `.instructions.md` extension.)
4. At the start of the file, create a frontmatter block containing the applyTo keyword. Use glob syntax to specify what files or directories the instructions apply to. For example:
   ```markdown
   ---
   applyTo: "src/**/*.{js,jsx,ts,tsx}"
   ---
   ```
   in this example, we will use, to apply to all files in the project:
   ```markdown
    ---
    applyTo: "**"
    ---
   ```  
5. Add custom instructions to guide Copilot.
    Example instructions:
    ```markdown
    # Project Overview

    This project is a web application that allows users to manage their tasks and to-do lists. It is built using React and Node.js, and uses MongoDB for data storage.

    ## Folder Structure

    - `/src`: Contains the source code for the frontend.
    - `/server`: Contains the source code for the Node.js backend.
    - `/docs`: Contains documentation for the project, including API specifications and user guides.

    ## Libraries and Frameworks

    - React and Tailwind CSS for the frontend.
    - Node.js and Express for the backend.
    - MongoDB for data storage.

    ## Coding Standards

    - Use semicolons at the end of each statement.
    - Use single quotes for strings.
    - Use function based components in React.
    - Use arrow functions for callbacks.

    ## UI guidelines

    - A toggle is provided to switch between light and dark mode.
    - Application should have a modern and clean design.
    ```

6. Save the file.
7. Try to use GitHub Copilot to generate code snippets, components, or functions based on your custom instructions.

---  
**Tip:** Experiment with different custom instructions to see how Copilot adapts its suggestions!