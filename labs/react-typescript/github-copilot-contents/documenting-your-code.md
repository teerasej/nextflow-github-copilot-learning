# Documenting Your Code

Learn how to use GitHub Copilot to generate comprehensive documentation for your React TypeScript photo gallery application.

## Exercise 1: Generate Component Documentation

1. Open your `src/components/PhotoGallery.tsx` file
2. Position your cursor above the component and use Copilot Chat:

```
@workspace generate comprehensive JSDoc documentation for the PhotoGallery component including props, usage examples, and accessibility notes
```

3. Copilot will generate detailed documentation:

```typescript
/**
 * PhotoGallery Component
 * 
 * A responsive photo gallery component that displays photos in a grid layout
 * with support for photo selection, modal view, and deletion functionality.
 * 
 * @component
 * @example
 * ```tsx
 * const photos = [
 *   { id: '1', url: 'photo1.jpg', title: 'Sunset' },
 *   { id: '2', url: 'photo2.jpg', title: 'Mountains' }
 * ];
 * 
 * <PhotoGallery 
 *   photos={photos} 
 *   onDeletePhoto={(id) => console.log('Delete photo:', id)} 
 * />
 * ```
 * 
 * @param {PhotoGalleryProps} props - The component props
 * @param {Photo[]} props.photos - Array of photo objects to display
 * @param {(photoId: string) => void} [props.onDeletePhoto] - Optional callback for photo deletion
 * 
 * @accessibility
 * - Images include alt text from photo titles
 * - Modal can be closed with Escape key
 * - Focus management for modal interactions
 * - Keyboard navigation support for photo grid
 * 
 * @performance
 * - Uses React.memo for optimization
 * - Lazy loading for images outside viewport
 * - Virtualization for large photo collections
 * 
 * @since 1.0.0
 */
interface PhotoGalleryProps {
  /** Array of photos to display in the gallery */
  photos: Photo[];
  /** Callback function called when a photo should be deleted */
  onDeletePhoto?: (photoId: string) => void;
}

export const PhotoGallery: React.FC<PhotoGalleryProps> = ({ 
  photos, 
  onDeletePhoto 
}) => {
  // Component implementation...
};
```

## Exercise 2: Document Custom Hooks

1. Open your `src/hooks/usePhotoGallery.ts` file
2. Ask Copilot to document the hook:

```
@workspace create detailed documentation for the usePhotoGallery hook including parameters, return values, usage patterns, and error handling
```

3. Copilot will generate comprehensive hook documentation:

```typescript
/**
 * usePhotoGallery Hook
 * 
 * A custom React hook for managing photo gallery state and operations.
 * Provides functionality for uploading, deleting, and managing photos
 * with integrated loading states and error handling.
 * 
 * @hook
 * @example
 * ```tsx
 * function PhotoGalleryPage() {
 *   const { 
 *     photos, 
 *     uploadPhoto, 
 *     deletePhoto, 
 *     isLoading, 
 *     error 
 *   } = usePhotoGallery();
 * 
 *   const handleFileUpload = async (file: File) => {
 *     try {
 *       await uploadPhoto(file);
 *       console.log('Photo uploaded successfully');
 *     } catch (error) {
 *       console.error('Upload failed:', error);
 *     }
 *   };
 * 
 *   return (
 *     <div>
 *       {error && <div className="error">{error}</div>}
 *       <PhotoUpload onUpload={handleFileUpload} isLoading={isLoading} />
 *       <PhotoGallery photos={photos} onDeletePhoto={deletePhoto} />
 *     </div>
 *   );
 * }
 * ```
 * 
 * @returns {UsePhotoGalleryReturn} Object containing photos state and management functions
 * 
 * @throws {Error} When photo upload fails due to invalid file type or size
 * @throws {Error} When photo deletion fails due to network issues
 * 
 * @performance
 * - Debounced upload operations to prevent duplicate requests
 * - Optimistic updates for better user experience
 * - Automatic retry logic for failed operations
 * 
 * @since 1.0.0
 */
export interface UsePhotoGalleryReturn {
  /** Array of photos currently in the gallery */
  photos: Photo[];
  /** Function to upload a new photo */
  uploadPhoto: (file: File) => Promise<void>;
  /** Function to delete a photo by ID */
  deletePhoto: (photoId: string) => Promise<void>;
  /** Loading state indicator */
  isLoading: boolean;
  /** Current error message, if any */
  error: string | null;
  /** Function to clear current error */
  clearError: () => void;
  /** Function to refresh photos from server */
  refreshPhotos: () => Promise<void>;
}

export const usePhotoGallery = (): UsePhotoGalleryReturn => {
  // Hook implementation...
};
```

## Exercise 3: Generate API Documentation

1. Open your `src/services/PhotoService.ts` file
2. Ask Copilot for service documentation:

```
@workspace generate comprehensive API documentation for the PhotoService class including method descriptions, parameters, return types, and error scenarios
```

3. Copilot will create detailed service documentation:

```typescript
/**
 * PhotoService Class
 * 
 * Service class for handling photo-related operations including upload,
 * retrieval, and deletion. Provides a clean abstraction over the photo
 * storage backend with comprehensive error handling.
 * 
 * @class PhotoService
 * @since 1.0.0
 * 
 * @example
 * ```typescript
 * // Upload a photo
 * const file = new File(['content'], 'photo.jpg', { type: 'image/jpeg' });
 * const photo = await PhotoService.uploadPhoto(file);
 * 
 * // Get all photos
 * const photos = PhotoService.getAllPhotos();
 * 
 * // Delete a photo
 * await PhotoService.deletePhoto(photo.id);
 * ```
 */
export class PhotoService {
  private static readonly STORAGE_KEY = 'photo-gallery-photos';
  private static readonly MAX_FILE_SIZE = 5 * 1024 * 1024; // 5MB
  private static readonly ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/webp'];

  /**
   * Uploads a photo file and stores it in the gallery
   * 
   * @param {File} file - The image file to upload
   * @returns {Promise<Photo>} Promise that resolves to the uploaded photo object
   * 
   * @throws {Error} When file is too large (>5MB)
   * @throws {Error} When file type is not supported
   * @throws {Error} When file reading fails
   * 
   * @example
   * ```typescript
   * const fileInput = document.querySelector('input[type="file"]');
   * const file = fileInput.files[0];
   * 
   * try {
   *   const photo = await PhotoService.uploadPhoto(file);
   *   console.log('Uploaded photo:', photo);
   * } catch (error) {
   *   console.error('Upload failed:', error.message);
   * }
   * ```
   * 
   * @since 1.0.0
   */
  static async uploadPhoto(file: File): Promise<Photo> {
    // Validation
    if (file.size > this.MAX_FILE_SIZE) {
      throw new Error(`File size too large. Maximum size is ${this.MAX_FILE_SIZE / 1024 / 1024}MB`);
    }

    if (!this.ALLOWED_TYPES.includes(file.type)) {
      throw new Error(`Unsupported file type. Allowed types: ${this.ALLOWED_TYPES.join(', ')}`);
    }

    // Implementation...
  }

  /**
   * Retrieves all photos from storage
   * 
   * @returns {Photo[]} Array of all stored photos
   * 
   * @example
   * ```typescript
   * const photos = PhotoService.getAllPhotos();
   * console.log(`Found ${photos.length} photos`);
   * ```
   * 
   * @since 1.0.0
   */
  static getAllPhotos(): Photo[] {
    // Implementation...
  }

  /**
   * Deletes a photo by its ID
   * 
   * @param {string} photoId - The unique identifier of the photo to delete
   * @returns {Promise<void>} Promise that resolves when deletion is complete
   * 
   * @throws {Error} When photo with given ID is not found
   * @throws {Error} When deletion operation fails
   * 
   * @example
   * ```typescript
   * try {
   *   await PhotoService.deletePhoto('photo-123');
   *   console.log('Photo deleted successfully');
   * } catch (error) {
   *   console.error('Deletion failed:', error.message);
   * }
   * ```
   * 
   * @since 1.0.0
   */
  static async deletePhoto(photoId: string): Promise<void> {
    // Implementation...
  }
}
```

## Exercise 4: Generate README Documentation

1. Ask Copilot to create a comprehensive README:

```
@workspace generate a comprehensive README.md file for the React TypeScript photo gallery project including setup instructions, features, API documentation, and deployment guide
```

2. Copilot will generate a detailed README:

```markdown
# Photo Gallery App

A modern, responsive photo gallery application built with React, TypeScript, and modern web technologies.

## 🌟 Features

- 📸 **Photo Upload**: Drag-and-drop or click to upload photos
- 🖼️ **Gallery View**: Responsive grid layout with hover effects
- 🔍 **Photo Modal**: Click to view photos in full-screen modal
- 🗑️ **Delete Photos**: Remove photos with confirmation
- 📱 **Responsive Design**: Works seamlessly on desktop and mobile
- ♿ **Accessibility**: Full keyboard navigation and screen reader support
- 🧪 **Comprehensive Testing**: Unit and integration tests included

## 🚀 Quick Start

### Prerequisites

- Node.js 18 or higher
- npm or yarn package manager

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd photo-gallery
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

4. Open [http://localhost:5173](http://localhost:5173) in your browser

## 📁 Project Structure

```
src/
├── components/          # React components
│   ├── PhotoGallery.tsx # Main gallery component
│   ├── PhotoUpload.tsx  # Upload component
│   └── PhotoCard.tsx    # Individual photo component
├── hooks/               # Custom React hooks
│   └── usePhotoGallery.ts
├── services/            # API services
│   └── PhotoService.ts
├── types/               # TypeScript type definitions
│   └── Photo.ts
├── utils/               # Utility functions
│   └── Calculator.ts
└── __tests__/           # Test files
```

## 🛠️ Available Scripts

- `npm run dev` - Start development server
- `npm test` - Run test suite
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run test:coverage` - Run tests with coverage report

## 📚 API Documentation

### Photo Interface

```typescript
interface Photo {
  id: string;
  url: string;
  title: string;
  uploadDate: Date;
  size: number;
  type: string;
}
```

### PhotoService Methods

- `uploadPhoto(file: File): Promise<Photo>` - Upload a photo
- `getAllPhotos(): Photo[]` - Get all photos
- `deletePhoto(photoId: string): Promise<void>` - Delete a photo

## 🧪 Testing

Run the test suite:

```bash
npm test
```

For coverage report:

```bash
npm run test:coverage
```

## 🚀 Deployment

### Build for Production

```bash
npm run build
```

### Deploy to Vercel

1. Install Vercel CLI: `npm i -g vercel`
2. Run: `vercel`
3. Follow the prompts

### Deploy to Netlify

1. Build the project: `npm run build`
2. Upload the `build` folder to Netlify

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and add tests
4. Run tests: `npm test`
5. Commit your changes: `git commit -m 'Add feature'`
6. Push to the branch: `git push origin feature-name`
7. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

## Exercise 5: Generate Type Documentation

1. Ask Copilot to document your TypeScript types:

```
@workspace generate comprehensive documentation for all TypeScript interfaces and types used in the photo gallery application
```

## Best Practices for Documentation

- **Keep it Current**: Update documentation when code changes
- **Include Examples**: Provide practical usage examples
- **Document Edge Cases**: Explain error scenarios and limitations
- **Use Consistent Format**: Follow JSDoc standards
- **Accessibility Notes**: Include accessibility considerations

## Automated Documentation Generation

Ask Copilot to set up automated documentation:

```
@workspace set up automated documentation generation using TypeDoc for the React TypeScript project
```

## Next Steps

Continue to [Explain Code](./explain-code.md) to learn how to use Copilot for code analysis and explanation.
