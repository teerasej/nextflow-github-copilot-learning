# Chat with Copilot

Use GitHub Copilot Chat to create components and features through conversational interactions.

## Exercise 1: Create a Photo Gallery Component

1. Open your React TypeScript project in VS Code
2. Open Copilot Chat from the sidebar (chat icon)
3. Type the following message:

```
@workspace create a PhotoGallery component that displays photos in a responsive grid layout. Include features for photo selection, zoom modal, and delete functionality. Use TypeScript and modern React patterns.
```

4. Press `Enter` to send the message
5. Copilot will generate a comprehensive component with explanations:

```typescript
// src/components/PhotoGallery.tsx
import React, { useState } from 'react';
import { Photo } from '../types/Photo';

interface PhotoGalleryProps {
  photos: Photo[];
  onDeletePhoto?: (photoId: string) => void;
}

export const PhotoGallery: React.FC<PhotoGalleryProps> = ({ 
  photos, 
  onDeletePhoto 
}) => {
  const [selectedPhoto, setSelectedPhoto] = useState<Photo | null>(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  const handlePhotoClick = (photo: Photo) => {
    setSelectedPhoto(photo);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
    setSelectedPhoto(null);
  };

  const handleDelete = (photoId: string) => {
    if (onDeletePhoto) {
      onDeletePhoto(photoId);
    }
    closeModal();
  };

  return (
    <div className="photo-gallery">
      <div className="photo-grid">
        {photos.map((photo) => (
          <div 
            key={photo.id} 
            className="photo-item"
            onClick={() => handlePhotoClick(photo)}
          >
            <img src={photo.url} alt={photo.title} />
            <div className="photo-overlay">
              <h3>{photo.title}</h3>
            </div>
          </div>
        ))}
      </div>

      {isModalOpen && selectedPhoto && (
        <div className="modal-overlay" onClick={closeModal}>
          <div className="modal-content" onClick={e => e.stopPropagation()}>
            <img src={selectedPhoto.url} alt={selectedPhoto.title} />
            <div className="modal-actions">
              <button onClick={() => handleDelete(selectedPhoto.id)}>
                Delete
              </button>
              <button onClick={closeModal}>Close</button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};
```

6. Use the **Create in new file** option to save this component

## Exercise 2: Create an API Service

1. In Copilot Chat, type:

```
@workspace create a PhotoService class for handling photo uploads, deletion, and fetching. Include error handling, TypeScript types, and simulate API calls with localStorage for now.
```

2. Copilot will generate a service class:

```typescript
// src/services/PhotoService.ts
import { Photo } from '../types/Photo';

export class PhotoService {
  private static STORAGE_KEY = 'photo-gallery-photos';

  static async uploadPhoto(file: File): Promise<Photo> {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      
      reader.onload = () => {
        const newPhoto: Photo = {
          id: Date.now().toString(),
          url: reader.result as string,
          title: file.name,
          uploadDate: new Date(),
          size: file.size,
          type: file.type
        };

        const existingPhotos = this.getAllPhotos();
        const updatedPhotos = [...existingPhotos, newPhoto];
        localStorage.setItem(this.STORAGE_KEY, JSON.stringify(updatedPhotos));
        
        resolve(newPhoto);
      };

      reader.onerror = () => reject(new Error('Failed to read file'));
      reader.readAsDataURL(file);
    });
  }

  static getAllPhotos(): Photo[] {
    const stored = localStorage.getItem(this.STORAGE_KEY);
    return stored ? JSON.parse(stored) : [];
  }

  static async deletePhoto(photoId: string): Promise<void> {
    const photos = this.getAllPhotos();
    const filteredPhotos = photos.filter(photo => photo.id !== photoId);
    localStorage.setItem(this.STORAGE_KEY, JSON.stringify(filteredPhotos));
  }
}
```

3. Save this as a new file using the suggested option

## Exercise 3: Generate a Custom Hook

1. Ask Copilot to create a custom hook:

```
@workspace create a custom hook usePhotoGallery that integrates with PhotoService and manages the photo gallery state including loading, error handling, and CRUD operations.
```

2. Review and save the generated custom hook

## Exercise 4: Use Terminal Commands

1. Ask Copilot for help with dependencies:

```
@workspace what dependencies do I need to install for a complete photo gallery with routing and styling? Provide the npm install command.
```

2. Use the **Run in terminal** option from the suggested commands
3. Common suggestions might include:
   ```bash
   npm install react-router-dom styled-components
   npm install --save-dev @types/react-router-dom
   ```

## Exercise 5: Test the Integration

1. Ask Copilot to help test the components:

```
Generate a command to test the photo upload functionality and ensure all components work together correctly.
```

2. Follow the suggested testing steps

## Tips for Effective Chat

- **Use @workspace**: This gives Copilot context about your entire project
- **Be Specific**: Include technology requirements (TypeScript, React, etc.)
- **Ask for Best Practices**: Request error handling, accessibility, performance considerations
- **Request Examples**: Ask for usage examples and integration code

## Advanced Prompts

Try these more sophisticated requests:

```
@workspace add pagination to the PhotoGallery component with infinite scroll

@workspace implement photo filtering and search functionality

@workspace add photo editing features like crop and rotate

@workspace create unit tests for the PhotoService class
```

## Next Steps

Continue to [Generate Unit Tests](./generate-unit-test.md) to learn about automated test generation.
