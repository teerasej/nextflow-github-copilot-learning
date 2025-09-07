# Using Copilot Edits

Copilot Edit allows you to make changes across multiple files and larger code sections with AI assistance.

## Exercise 1: Enhance Calculator with Scientific Functions

1. Open your React TypeScript project in VS Code
2. If you haven't created it yet, create the `src/utils/Calculator.ts` file with basic operations:

    ```typescript
    export class Calculator {
      add(a: number, b: number): number {
        return a + b;
      }

      subtract(a: number, b: number): number {
        return a - b;
      }

      multiply(a: number, b: number): number {
        return a * b;
      }

      divide(a: number, b: number): number {
        if (b === 0) {
          throw new Error('Division by zero is not allowed');
        }
        return a / b;
      }
    }
    ```

3. Open the `Calculator.ts` file
4. Open GitHub Copilot Chat and click the `Copilot Edit` button to switch to Edit mode
5. Notice the **Working set** at the bottom showing `Calculator.ts`. You can add more files by clicking the `+` button
6. Type the following message:

    ```
    Add scientific calculator functions including square root, power, sine, cosine, and logarithm to the Calculator class.
    ```

7. Copilot will generate and apply the scientific functions directly to your file
8. Review the changes and accept or reject them as needed

## Exercise 2: Create a Photo Upload Component

1. Create a new file `src/components/PhotoUpload.tsx`
2. Add it to the Copilot Edit working set
3. Prompt:

    ```
    Create a React TypeScript component for photo upload with drag-and-drop functionality, file validation, and preview. Include proper error handling and loading states.
    ```

4. Copilot will create a comprehensive component with:
   - Drag and drop functionality
   - File type validation
   - Image preview
   - Loading and error states
   - TypeScript interfaces

## Exercise 3: Multi-file Update

1. Add multiple files to your working set:
   - `src/App.tsx`
   - `src/components/PhotoUpload.tsx`
   - `src/components/Calculator.tsx`

2. Prompt:

    ```
    Update the App.tsx to include both the Calculator and PhotoUpload components with proper routing using React Router. Add navigation between the two pages.
    ```

3. Copilot will:
   - Install necessary dependencies
   - Update App.tsx with routing
   - Create navigation components
   - Ensure proper TypeScript types

## Exercise 4: Add State Management

1. Add these files to the working set:
   - `src/App.tsx`
   - `src/hooks/usePhotos.ts` (create if not exists)
   - `src/types/Photo.ts`

2. Prompt:

    ```
    Implement global state management for photos using React Context and useReducer. Include actions for adding, deleting, and updating photos.
    ```

3. Review the generated changes across multiple files

## Best Practices for Copilot Edit

- **Clear Working Set**: Only include relevant files to avoid confusion
- **Specific Instructions**: Be clear about what you want to change
- **Review Changes**: Always review before accepting changes
- **Incremental Changes**: Make smaller changes for better accuracy

## Summary

Copilot Edit is powerful for:
- Adding features across multiple files
- Refactoring code architecture
- Implementing patterns consistently
- Making coordinated changes

## Next Steps

Continue to [Chat with Copilot](./chat-with-copilot.md) to learn about using the chat interface for component creation.
