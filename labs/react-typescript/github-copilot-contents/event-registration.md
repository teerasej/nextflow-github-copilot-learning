# Create Event Registration Feature

This challenge combines all the GitHub Copilot techniques you've learned to build a complete event registration system for your photo gallery application.

> **Note:** Before you start, create a new branch `v2` and commit all changes to it.

## Challenge Overview

Create an event registration feature that allows users to register for photography events and workshops. This challenge will test your ability to use GitHub Copilot effectively for:

- Component creation
- State management
- Form validation
- API integration
- Testing
- Documentation

## Phase 1: Planning and Setup

1. **Create a new branch:**
   ```bash
   git checkout -b v2
   ```

2. **Use Copilot Chat to plan the feature:**
   ```
   @workspace I want to add an event registration feature to the photo gallery app. Users should be able to view upcoming photography events, register with their details, and manage their registrations. What components and structure would you recommend?
   ```

3. **Let Copilot suggest the project structure and components needed**

## Phase 2: Create Type Definitions

1. Create a new file `src/types/Event.ts`
2. Use Copilot Chat to generate comprehensive types:

```
@workspace create TypeScript interfaces for an event registration system including Event, Registration, and User types with proper validation requirements
```

**Expected types should include:**
- Event details (id, title, description, date, location, capacity, price)
- Registration information (user details, event selection, payment status)
- Form validation schemas

## Phase 3: Build Core Components

### 3.1 Event List Component

1. Create `src/components/EventList.tsx`
2. Use Copilot Edit to generate:

```
Create a React TypeScript component that displays a list of photography events in a card layout. Include event details, pricing, available spots, and registration buttons. Make it responsive and accessible.
```

### 3.2 Event Registration Form

1. Create `src/components/EventRegistrationForm.tsx`
2. Use inline chat (Ctrl+I) to create:

```
Create a comprehensive event registration form with fields for personal information, event selection, dietary requirements, and emergency contact. Include form validation, error handling, and success states.
```

### 3.3 Registration Management

1. Create `src/components/MyRegistrations.tsx`
2. Use Copilot Chat:

```
@workspace create a component for users to view and manage their event registrations, including the ability to cancel registrations and view event details
```

## Phase 4: State Management and Services

### 4.1 Custom Hook

1. Create `src/hooks/useEventRegistration.ts`
2. Ask Copilot to create:

```
@workspace create a custom hook for managing event registration state including fetching events, submitting registrations, and handling loading/error states
```

### 4.2 API Service

1. Create `src/services/EventService.ts`
2. Use Copilot to generate:

```
@workspace create an EventService class with methods for fetching events, submitting registrations, canceling registrations, and managing user registration data using localStorage for now
```

## Phase 5: Routing and Navigation

1. Update `src/App.tsx` to include routing
2. Use Copilot Edit with multiple files in working set:

```
Add React Router to the application with routes for the photo gallery, event list, event registration form, and user registration management. Include proper navigation with active states.
```

## Phase 6: Form Validation and UX

### 6.1 Form Validation

1. Ask Copilot to add robust form validation:

```
@workspace implement comprehensive form validation for the event registration form using a validation library like Formik or react-hook-form, including real-time validation and user-friendly error messages
```

### 6.2 Loading and Error States

1. Enhance components with proper UX states:

```
@workspace add loading spinners, error boundaries, and empty states to all event registration components with proper accessibility support
```

## Phase 7: Testing

### 7.1 Unit Tests

1. Use Copilot to generate comprehensive tests:

```
/test
```

For each component and service, ask for:
- Component rendering tests
- User interaction tests
- Form validation tests
- API service tests
- Custom hook tests

### 7.2 Integration Tests

1. Create integration tests:

```
@workspace create integration tests for the complete event registration flow from viewing events to successful registration submission
```

## Phase 8: Styling and Responsive Design

1. Ask Copilot for CSS/styling:

```
@workspace create responsive CSS modules for the event registration components with a modern, professional design that matches the photo gallery theme
```

## Phase 9: Documentation

1. Generate comprehensive documentation:

```
@workspace create detailed documentation for the event registration feature including component usage, API endpoints, and user workflows
```

## Final Implementation Requirements

Your completed feature should include:

### ✅ Core Functionality
- [ ] Display list of available events
- [ ] Event registration form with validation
- [ ] User registration management
- [ ] Registration cancellation
- [ ] Capacity management (prevent over-booking)

### ✅ Technical Requirements
- [ ] TypeScript interfaces for all data structures
- [ ] Custom hooks for state management
- [ ] Service layer for API interactions
- [ ] Form validation with error handling
- [ ] Loading and error states
- [ ] Responsive design

### ✅ Quality Assurance
- [ ] Unit tests for all components
- [ ] Integration tests for user workflows
- [ ] Accessibility compliance
- [ ] Error boundary implementation
- [ ] Performance optimization

### ✅ Documentation
- [ ] Component documentation with examples
- [ ] API documentation
- [ ] User guide for the registration process
- [ ] Developer setup instructions

## Example Final Structure

```typescript
// src/types/Event.ts
export interface Event {
  id: string;
  title: string;
  description: string;
  date: Date;
  location: string;
  capacity: number;
  registered: number;
  price: number;
  imageUrl: string;
  category: 'workshop' | 'exhibition' | 'competition';
}

export interface Registration {
  id: string;
  eventId: string;
  userId: string;
  registrationDate: Date;
  status: 'confirmed' | 'pending' | 'cancelled';
  personalInfo: PersonalInfo;
  specialRequirements?: string;
}

export interface PersonalInfo {
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
  emergencyContact: {
    name: string;
    phone: string;
    relationship: string;
  };
  dietaryRequirements?: string[];
}
```

## Bonus Challenges

If you complete the main requirements, try these advanced features:

1. **Payment Integration**: Add Stripe integration for event payments
2. **Email Notifications**: Implement email confirmations
3. **Calendar Integration**: Add calendar export functionality
4. **Waitlist Management**: Handle event waitlists when capacity is full
5. **Admin Panel**: Create an admin interface for event management

## Evaluation Criteria

Your implementation will be evaluated on:

- **Code Quality**: Clean, readable, and well-structured code
- **Copilot Usage**: Effective use of various Copilot features
- **Testing Coverage**: Comprehensive test suite
- **User Experience**: Intuitive and accessible interface
- **Documentation**: Clear and comprehensive documentation
- **Error Handling**: Robust error handling and edge cases

## Submission

1. Commit all changes to the `v2` branch
2. Create a pull request with a detailed description
3. Include screenshots of the working application
4. Document any challenges faced and how you solved them

## Tips for Success

- **Use Copilot Chat liberally** for planning and architecture decisions
- **Leverage Copilot Edit** for multi-file changes
- **Don't forget to use inline suggestions** for faster coding
- **Ask for explanations** when you don't understand generated code
- **Iterate and refine** based on Copilot's suggestions
- **Test frequently** and ask Copilot to help with debugging

Good luck! This challenge will demonstrate your mastery of GitHub Copilot and modern React TypeScript development.
