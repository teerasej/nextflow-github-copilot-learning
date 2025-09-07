# Generate Unit Tests

Learn how to use GitHub Copilot to generate comprehensive unit tests for your React TypeScript components and utilities.

## Prerequisites

First, install testing dependencies:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event jest-environment-jsdom
```

## Exercise 1: Generate Tests for Calculator Class

1. Open your `src/utils/Calculator.ts` file
2. Open Copilot Chat and type:

```
/test
```

3. Copilot will suggest creating unit tests for the Calculator class
4. You'll see a comprehensive test suite generated:

```typescript
// src/utils/__tests__/Calculator.test.ts
import { Calculator } from '../Calculator';

describe('Calculator', () => {
  let calculator: Calculator;

  beforeEach(() => {
    calculator = new Calculator();
  });

  describe('add', () => {
    it('should add two positive numbers correctly', () => {
      expect(calculator.add(2, 3)).toBe(5);
    });

    it('should add negative numbers correctly', () => {
      expect(calculator.add(-2, -3)).toBe(-5);
    });

    it('should add zero correctly', () => {
      expect(calculator.add(5, 0)).toBe(5);
    });
  });

  describe('subtract', () => {
    it('should subtract two numbers correctly', () => {
      expect(calculator.subtract(5, 3)).toBe(2);
    });

    it('should handle negative results', () => {
      expect(calculator.subtract(3, 5)).toBe(-2);
    });
  });

  describe('multiply', () => {
    it('should multiply two numbers correctly', () => {
      expect(calculator.multiply(4, 3)).toBe(12);
    });

    it('should handle multiplication by zero', () => {
      expect(calculator.multiply(5, 0)).toBe(0);
    });
  });

  describe('divide', () => {
    it('should divide two numbers correctly', () => {
      expect(calculator.divide(10, 2)).toBe(5);
    });

    it('should throw error when dividing by zero', () => {
      expect(() => calculator.divide(10, 0)).toThrow('Division by zero is not allowed');
    });

    it('should handle decimal results', () => {
      expect(calculator.divide(10, 3)).toBeCloseTo(3.333, 3);
    });
  });
});
```

5. Accept the suggestion and create the test file

## Exercise 2: Generate Tests for React Components

1. Open your `src/components/PhotoGallery.tsx` file
2. Use Copilot Chat with:

```
@workspace generate comprehensive unit tests for the PhotoGallery component including user interactions, prop handling, and modal functionality
```

3. Copilot will create detailed component tests:

```typescript
// src/components/__tests__/PhotoGallery.test.tsx
import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom';
import { PhotoGallery } from '../PhotoGallery';
import { Photo } from '../../types/Photo';

const mockPhotos: Photo[] = [
  {
    id: '1',
    url: 'https://example.com/photo1.jpg',
    title: 'Test Photo 1',
    uploadDate: new Date('2023-01-01'),
    size: 1024,
    type: 'image/jpeg'
  },
  {
    id: '2',
    url: 'https://example.com/photo2.jpg',
    title: 'Test Photo 2',
    uploadDate: new Date('2023-01-02'),
    size: 2048,
    type: 'image/png'
  }
];

describe('PhotoGallery', () => {
  const mockOnDeletePhoto = jest.fn();

  beforeEach(() => {
    mockOnDeletePhoto.mockClear();
  });

  it('renders all photos in the gallery', () => {
    render(<PhotoGallery photos={mockPhotos} onDeletePhoto={mockOnDeletePhoto} />);
    
    expect(screen.getByAltText('Test Photo 1')).toBeInTheDocument();
    expect(screen.getByAltText('Test Photo 2')).toBeInTheDocument();
  });

  it('opens modal when photo is clicked', async () => {
    const user = userEvent.setup();
    render(<PhotoGallery photos={mockPhotos} onDeletePhoto={mockOnDeletePhoto} />);
    
    const photo = screen.getByAltText('Test Photo 1');
    await user.click(photo);
    
    expect(screen.getByRole('dialog')).toBeInTheDocument();
  });

  it('closes modal when close button is clicked', async () => {
    const user = userEvent.setup();
    render(<PhotoGallery photos={mockPhotos} onDeletePhoto={mockOnDeletePhoto} />);
    
    // Open modal
    const photo = screen.getByAltText('Test Photo 1');
    await user.click(photo);
    
    // Close modal
    const closeButton = screen.getByText('Close');
    await user.click(closeButton);
    
    expect(screen.queryByRole('dialog')).not.toBeInTheDocument();
  });

  it('calls onDeletePhoto when delete button is clicked', async () => {
    const user = userEvent.setup();
    render(<PhotoGallery photos={mockPhotos} onDeletePhoto={mockOnDeletePhoto} />);
    
    // Open modal
    const photo = screen.getByAltText('Test Photo 1');
    await user.click(photo);
    
    // Delete photo
    const deleteButton = screen.getByText('Delete');
    await user.click(deleteButton);
    
    expect(mockOnDeletePhoto).toHaveBeenCalledWith('1');
  });

  it('renders empty state when no photos provided', () => {
    render(<PhotoGallery photos={[]} onDeletePhoto={mockOnDeletePhoto} />);
    
    const photoGrid = screen.getByTestId('photo-grid');
    expect(photoGrid).toBeEmptyDOMElement();
  });
});
```

## Exercise 3: Generate Hook Tests

1. Open your custom hook file (e.g., `src/hooks/usePhotoGallery.ts`)
2. Ask Copilot:

```
@workspace create unit tests for the usePhotoGallery hook including state management, loading states, and error handling
```

3. Copilot will generate hook tests using React Testing Library's `renderHook`:

```typescript
// src/hooks/__tests__/usePhotoGallery.test.ts
import { renderHook, act } from '@testing-library/react';
import { usePhotoGallery } from '../usePhotoGallery';
import { PhotoService } from '../../services/PhotoService';

// Mock the PhotoService
jest.mock('../../services/PhotoService');
const mockedPhotoService = PhotoService as jest.Mocked<typeof PhotoService>;

describe('usePhotoGallery', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('should initialize with empty photos array and loading false', () => {
    const { result } = renderHook(() => usePhotoGallery());
    
    expect(result.current.photos).toEqual([]);
    expect(result.current.isLoading).toBe(false);
    expect(result.current.error).toBeNull();
  });

  it('should handle photo upload successfully', async () => {
    const mockPhoto = { id: '1', title: 'Test Photo', url: 'test.jpg' };
    mockedPhotoService.uploadPhoto.mockResolvedValue(mockPhoto);
    
    const { result } = renderHook(() => usePhotoGallery());
    const mockFile = new File(['test'], 'test.jpg', { type: 'image/jpeg' });
    
    await act(async () => {
      await result.current.uploadPhoto(mockFile);
    });
    
    expect(result.current.photos).toContain(mockPhoto);
    expect(mockedPhotoService.uploadPhoto).toHaveBeenCalledWith(mockFile);
  });

  it('should handle upload errors', async () => {
    const mockError = new Error('Upload failed');
    mockedPhotoService.uploadPhoto.mockRejectedValue(mockError);
    
    const { result } = renderHook(() => usePhotoGallery());
    const mockFile = new File(['test'], 'test.jpg', { type: 'image/jpeg' });
    
    await act(async () => {
      await result.current.uploadPhoto(mockFile);
    });
    
    expect(result.current.error).toBe('Upload failed');
    expect(result.current.photos).toEqual([]);
  });
});
```

## Exercise 4: Generate Integration Tests

1. Ask Copilot for integration tests:

```
@workspace create integration tests that test the entire photo upload flow from component interaction to service calls
```

2. Copilot will generate end-to-end component integration tests

## Running Tests

1. Add test scripts to your `package.json` if not already present:

```json
{
  "scripts": {
    "test": "react-scripts test",
    "test:coverage": "react-scripts test --coverage --watchAll=false"
  }
}
```

2. Run tests:

```bash
npm test
```

3. For coverage report:

```bash
npm run test:coverage
```

## Test Best Practices Suggested by Copilot

- **Arrange, Act, Assert**: Structure tests clearly
- **Mock External Dependencies**: Use Jest mocks for services
- **Test User Interactions**: Use user-event library
- **Cover Edge Cases**: Include error scenarios
- **Descriptive Test Names**: Use clear, descriptive test descriptions

## Challenge

Ask Copilot to:

```
Generate performance tests for the PhotoGallery component to ensure it handles large numbers of photos efficiently
```

## Next Steps

Continue to [Generate Integration Tests](./generate-integration-test.md) for broader testing strategies.
