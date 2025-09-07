# Triggering Inline Suggestions (Ghost Text)

GitHub Copilot provides real-time code suggestions as you type, appearing as "ghost text" that you can accept or ignore.

## Exercise 1: Creating a Photo Interface

1. Open your React TypeScript project in VS Code
2. Create a new file `src/types/Photo.ts`
3. Start typing the following code and watch for Copilot suggestions:

```typescript
export interface Photo {
  id: string;
  url: string;
  title: string;
```

4. GitHub Copilot should suggest completing the interface with additional properties like `description`, `uploadDate`, `tags`, etc.
5. Use `Tab` to accept suggestions or `Esc` to dismiss them
6. Try cycling through multiple suggestions using `Alt+]` and `Alt+[`

## Exercise 2: Creating a React Component

1. Create a new file `src/components/PhotoCard.tsx`
2. Start typing:

```typescript
import React from 'react';
import { Photo } from '../types/Photo';

interface PhotoCardProps {
  photo: Photo;
}

const PhotoCard: React.FC<PhotoCardProps> = ({ photo }) => {
```

3. Copilot should suggest the component implementation including JSX structure
4. Continue typing and let Copilot suggest the return statement and JSX

## Exercise 3: Creating a Custom Hook

1. Create a new file `src/hooks/usePhotos.ts`
2. Start typing:

```typescript
import { useState, useEffect } from 'react';
import { Photo } from '../types/Photo';

export const usePhotos = () => {
```

3. Copilot should suggest a complete custom hook implementation for managing photos state
4. Accept or modify the suggestions as needed

## Tips for Better Suggestions

- **Context Matters**: Copilot analyzes your entire file and project structure
- **Clear Naming**: Use descriptive variable and function names
- **Comments**: Add comments to guide Copilot's suggestions
- **Type Information**: In TypeScript, type annotations help Copilot provide better suggestions

## Example of Good Context

```typescript
// Custom hook for managing photo gallery state
export const usePhotoGallery = () => {
  // Copilot will now understand this is for photo management
```

## Next Steps

Continue to [Inline Chat](./inline-chat.md) to learn about interactive conversations with Copilot.
