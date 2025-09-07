# Inline Chat

Inline chat allows you to have a conversation with Copilot directly in your code editor, providing specific instructions for code generation.

## 1. Create a Calculator Utility Class

1. Open your React TypeScript project in VS Code
2. Create an empty file named `src/utils/Calculator.ts`
3. Open the file `Calculator.ts`
4. Use **Ctrl + I** (or **Cmd + I** on Mac) to trigger the inline chat
5. Type the following message in the chat:
   ```
   Create a calculator class with add, subtract, multiply, and divide methods. Use TypeScript and include proper error handling.
   ```
6. Copilot will generate code similar to this:
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
7. Review the generated code and click `Accept` to apply it

## 2. Create a React Component with State

1. Create a new file `src/components/Calculator.tsx`
2. Use **Ctrl + I** to trigger inline chat
3. Prompt the following message:
   ```
   Create a React functional component that uses the Calculator class. Include input fields for two numbers, buttons for each operation, and display the result. Use TypeScript and React hooks.
   ```
4. Copilot will generate a complete React component with:
   - State management using `useState`
   - Event handlers for button clicks
   - Input validation
   - Proper TypeScript types
5. Review and accept the generated code

## 3. Add Advanced Features

1. Position your cursor at the end of the Calculator component
2. Use **Ctrl + I** to trigger inline chat
3. Prompt:
   ```
   Add a history feature that keeps track of all calculations performed. Include a button to clear history and display the last 5 calculations.
   ```
4. Copilot will suggest additional state and JSX for the history feature
5. Accept the suggestions and integrate them into your component

## 4. Create a Custom Hook

1. Create a new file `src/hooks/useCalculator.ts`
2. Use **Ctrl + I** and prompt:
   ```
   Extract the calculator logic into a custom hook called useCalculator that manages state and operations.
   ```
3. Copilot will create a custom hook that you can use in your components

## Tips for Effective Inline Chat

- **Be Specific**: Provide clear requirements and context
- **Mention Technologies**: Specify "React", "TypeScript", "hooks", etc.
- **Ask for Best Practices**: Request error handling, accessibility, etc.
- **Iterate**: Use follow-up prompts to refine the code

## Example Follow-up Prompts

```
Add proper TypeScript types for all props
Include error handling for invalid inputs
Make this component accessible with proper ARIA labels
Add unit tests for this component
```

## Next Steps

Continue to [Using Copilot Edit](./copilot-edit-1.md) to learn about making larger code changes.
