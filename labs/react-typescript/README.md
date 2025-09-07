# GitHub Copilot - Learning with React TypeScript

This tutorial demonstrates GitHub Copilot features using a React TypeScript project. You'll learn how to leverage AI-powered coding assistance to build a modern photo gallery application with event registration functionality.

## Prerequisites

- Node.js 18+ installed
- VS Code with GitHub Copilot extension
- Basic knowledge of React and TypeScript

## Project Setup

### Option 1: Use the Provided Starter Project (Recommended)

Navigate to the starter project directory:
```bash
cd photo-gallery
npm install
npm run dev
```

### Option 2: Create a New Project from Scratch

Create a new React TypeScript project using Vite:
```bash
npm create vite@latest photo-gallery -- --template react-ts
cd photo-gallery
npm install
npm run dev
```

## Tutorial Structure

> **Note:** Before you start following the steps, make sure you create a new branch so your **main** branch will survive.

## 1. Getting Started

1. [Fork and create your GitHub codespace](./github-copilot-contents/activate-codespace.md)
2. [Explore your workspace](./github-copilot-contents/explore-workspace.md)

## 2. Triggering GitHub Copilot in the code

1. [Ghost text (Inline Suggestions)](github-copilot-contents/inline-suggestion.md)
2. [Inline chat](github-copilot-contents/inline-chat.md)

## 3. Copilot Edits 

1. [Using Copilot Edit](github-copilot-contents/copilot-edit-1.md)

## 4. Chat with Copilot

1. [Create components with Chat](github-copilot-contents/chat-with-copilot.md)

## 5. Generate Testing 

1. [Generate Unit test for components](github-copilot-contents/generate-unit-test.md)
2. [Generate Integration tests](github-copilot-contents/generate-integration-test.md)

## 6. Documentation and Code Quality

1. [Document your code](github-copilot-contents/documenting-your-code.md)
2. [Explain code functionality](github-copilot-contents/explain-code.md)

## 7. DevOps Integration

1. [Generate GitHub Actions workflow](github-copilot-contents/generate-github-action.md)

## Challenge 

> **Note:** Before you start following the steps, make sure you create a new branch **v2**, then commit all changes to it.

1. [Create an event registration feature](github-copilot-contents/event-registration.md)
2. [Service and mocking challenge](github-copilot-contents/challenge-service-and-mocking.md)

## Final Project Structure

Your React TypeScript photo gallery will include:
- 📸 Photo upload and display functionality
- 🎯 Event registration with form validation
- 🧪 Comprehensive testing suite
- 📚 Well-documented code
- 🚀 CI/CD pipeline with GitHub Actions

## Reference 

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [React TypeScript Best Practices](https://react-typescript-cheatsheet.netlify.app/)
