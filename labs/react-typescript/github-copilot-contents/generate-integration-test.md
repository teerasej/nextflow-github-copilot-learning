# Generate Integration Tests

Learn how to create comprehensive integration tests that validate the interaction between multiple components and services in your React TypeScript application.

## Prerequisites

Install additional testing dependencies:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event msw
```

## Exercise 1: Setup Mock Service Worker (MSW)

1. Ask Copilot to set up API mocking:

```
@workspace set up Mock Service Worker (MSW) for testing API interactions in a React TypeScript photo gallery application
```

2. Copilot will generate MSW setup files:

```typescript
// src/mocks/handlers.ts
import { rest } from 'msw';
import { Photo } from '../types/Photo';

const mockPhotos: Photo[] = [
  {
    id: '1',
    url: 'https://picsum.photos/300/200?random=1',
    title: 'Mountain Landscape',
    uploadDate: new Date('2023-01-01'),
    size: 1024000,
    type: 'image/jpeg'
  },
  {
    id: '2',
    url: 'https://picsum.photos/300/200?random=2',
    title: 'Ocean View',
    uploadDate: new Date('2023-01-02'),
    size: 2048000,
    type: 'image/png'
  }
];

export const handlers = [
  // Get all photos
  rest.get('/api/photos', (req, res, ctx) => {
    return res(ctx.json(mockPhotos));
  }),

  // Upload photo
  rest.post('/api/photos', (req, res, ctx) => {
    const newPhoto: Photo = {
      id: Date.now().toString(),
      url: 'https://picsum.photos/300/200?random=' + Date.now(),
      title: 'New Photo',
      uploadDate: new Date(),
      size: 1500000,
      type: 'image/jpeg'
    };

    return res(ctx.json(newPhoto));
  }),

  // Delete photo
  rest.delete('/api/photos/:id', (req, res, ctx) => {
    return res(ctx.status(204));
  })
];
```

```typescript
// src/mocks/server.ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

3. Update your test setup file:

```typescript
// src/setupTests.ts
import '@testing-library/jest-dom';
import { server } from './mocks/server';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

## Exercise 2: Full Application Integration Test

1. Ask Copilot to create comprehensive integration tests:

```
@workspace create integration tests for the complete photo gallery workflow: upload photo, display in gallery, view in modal, and delete photo
```

2. Copilot will generate comprehensive integration tests:

```typescript
// src/__tests__/PhotoGalleryApp.integration.test.tsx
import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom';
import { PhotoGalleryApp } from '../components/PhotoGalleryApp';
import { server } from '../mocks/server';
import { rest } from 'msw';

describe('Photo Gallery Integration Tests', () => {
  beforeEach(() => {
    // Reset any specific handlers before each test
    server.resetHandlers();
  });

  it('should complete full photo workflow: upload, view, and delete', async () => {
    const user = userEvent.setup();
    render(<PhotoGalleryApp />);

    // 1. Initial state - should show loading or empty state
    expect(screen.getByText(/loading/i)).toBeInTheDocument();

    // 2. Wait for photos to load
    await waitFor(() => {
      expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();
      expect(screen.getByText('Ocean View')).toBeInTheDocument();
    });

    // 3. Upload a new photo
    const fileInput = screen.getByLabelText(/upload photo/i);
    const mockFile = new File(['photo content'], 'test-photo.jpg', {
      type: 'image/jpeg'
    });

    await user.upload(fileInput, mockFile);

    // 4. Wait for upload to complete and new photo to appear
    await waitFor(() => {
      expect(screen.getByText('New Photo')).toBeInTheDocument();
    });

    // 5. Click on a photo to open modal
    const photoElement = screen.getByAltText('Mountain Landscape');
    await user.click(photoElement);

    // 6. Verify modal is open
    expect(screen.getByRole('dialog')).toBeInTheDocument();
    expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();

    // 7. Delete photo from modal
    const deleteButton = screen.getByText('Delete');
    await user.click(deleteButton);

    // 8. Confirm deletion
    const confirmButton = screen.getByText('Confirm Delete');
    await user.click(confirmButton);

    // 9. Verify photo is removed from gallery
    await waitFor(() => {
      expect(screen.queryByText('Mountain Landscape')).not.toBeInTheDocument();
    });

    // 10. Verify modal is closed
    expect(screen.queryByRole('dialog')).not.toBeInTheDocument();
  });

  it('should handle upload errors gracefully', async () => {
    // Override handler to simulate upload failure
    server.use(
      rest.post('/api/photos', (req, res, ctx) => {
        return res(ctx.status(500), ctx.json({ error: 'Upload failed' }));
      })
    );

    const user = userEvent.setup();
    render(<PhotoGalleryApp />);

    // Wait for initial load
    await waitFor(() => {
      expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();
    });

    // Attempt to upload a file
    const fileInput = screen.getByLabelText(/upload photo/i);
    const mockFile = new File(['photo content'], 'test-photo.jpg', {
      type: 'image/jpeg'
    });

    await user.upload(fileInput, mockFile);

    // Verify error message is displayed
    await waitFor(() => {
      expect(screen.getByText(/upload failed/i)).toBeInTheDocument();
    });

    // Verify gallery still shows original photos
    expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();
    expect(screen.getByText('Ocean View')).toBeInTheDocument();
  });

  it('should handle network failures', async () => {
    // Simulate network error
    server.use(
      rest.get('/api/photos', (req, res, ctx) => {
        return res.networkError('Network connection failed');
      })
    );

    render(<PhotoGalleryApp />);

    // Verify error state is displayed
    await waitFor(() => {
      expect(screen.getByText(/failed to load photos/i)).toBeInTheDocument();
    });

    // Verify retry functionality
    const retryButton = screen.getByText('Retry');
    expect(retryButton).toBeInTheDocument();
  });

  it('should persist photo state during navigation', async () => {
    const user = userEvent.setup();
    render(<PhotoGalleryApp />);

    // Wait for photos to load
    await waitFor(() => {
      expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();
    });

    // Navigate to different view (e.g., settings)
    const settingsLink = screen.getByText('Settings');
    await user.click(settingsLink);

    // Navigate back to gallery
    const galleryLink = screen.getByText('Gallery');
    await user.click(galleryLink);

    // Verify photos are still displayed
    expect(screen.getByText('Mountain Landscape')).toBeInTheDocument();
    expect(screen.getByText('Ocean View')).toBeInTheDocument();
  });
});
```

## Exercise 3: Component Integration Tests

1. Ask Copilot for component integration testing:

```
@workspace create integration tests that verify PhotoUpload component properly integrates with PhotoService and updates the PhotoGallery
```

2. Copilot will generate tests that validate component interactions:

```typescript
// src/__tests__/PhotoUploadGallery.integration.test.tsx
import React from 'react';
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom';
import { PhotoUpload } from '../components/PhotoUpload';
import { PhotoGallery } from '../components/PhotoGallery';
import { usePhotoGallery } from '../hooks/usePhotoGallery';

// Integration test component that combines upload and gallery
const PhotoUploadGalleryIntegration: React.FC = () => {
  const { photos, uploadPhoto, deletePhoto, isLoading, error } = usePhotoGallery();

  return (
    <div>
      <PhotoUpload 
        onUpload={uploadPhoto} 
        isLoading={isLoading} 
        error={error} 
      />
      <PhotoGallery 
        photos={photos} 
        onDeletePhoto={deletePhoto} 
      />
    </div>
  );
};

describe('PhotoUpload and PhotoGallery Integration', () => {
  it('should upload photo and immediately display in gallery', async () => {
    const user = userEvent.setup();
    render(<PhotoUploadGalleryIntegration />);

    // Create a mock file
    const mockFile = new File(['photo content'], 'beach.jpg', {
      type: 'image/jpeg'
    });

    // Upload the file
    const fileInput = screen.getByLabelText(/choose file|upload/i);
    await user.upload(fileInput, mockFile);

    // Verify upload button appears and click it
    const uploadButton = screen.getByText('Upload Photo');
    await user.click(uploadButton);

    // Wait for photo to appear in gallery
    await waitFor(() => {
      expect(screen.getByAltText('beach.jpg')).toBeInTheDocument();
    });

    // Verify photo count increased
    const photos = screen.getAllByRole('img');
    expect(photos.length).toBeGreaterThan(0);
  });

  it('should show loading state during upload', async () => {
    const user = userEvent.setup();
    render(<PhotoUploadGalleryIntegration />);

    const mockFile = new File(['photo content'], 'loading-test.jpg', {
      type: 'image/jpeg'
    });

    const fileInput = screen.getByLabelText(/choose file|upload/i);
    await user.upload(fileInput, mockFile);

    const uploadButton = screen.getByText('Upload Photo');
    await user.click(uploadButton);

    // Verify loading state appears
    expect(screen.getByText(/uploading/i)).toBeInTheDocument();

    // Wait for upload to complete
    await waitFor(() => {
      expect(screen.queryByText(/uploading/i)).not.toBeInTheDocument();
    });
  });
});
```

## Exercise 4: E2E-Style Integration Tests

1. Ask Copilot for end-to-end style tests:

```
@workspace create end-to-end style integration tests that simulate real user workflows including file validation, upload progress, and error recovery
```

2. Review and implement the comprehensive E2E tests

## Exercise 5: Performance Integration Tests

1. Generate performance-focused integration tests:

```
@workspace create integration tests that verify the photo gallery performs well with large numbers of photos and handles memory efficiently
```

## Running Integration Tests

1. Run specific integration tests:

```bash
npm test -- --testNamePattern="integration"
```

2. Run with coverage:

```bash
npm test -- --coverage --testNamePattern="integration"
```

3. Watch mode for development:

```bash
npm test -- --watch --testNamePattern="integration"
```

## Best Practices for Integration Testing

- **Test Real User Workflows**: Simulate actual user interactions
- **Mock External Services**: Use MSW for API calls
- **Test Error Scenarios**: Include network failures and edge cases
- **Verify State Persistence**: Ensure data consistency across components
- **Performance Considerations**: Test with realistic data volumes

## Next Steps

Continue to [Document Your Code](./documenting-your-code.md) to learn about generating documentation with Copilot.
