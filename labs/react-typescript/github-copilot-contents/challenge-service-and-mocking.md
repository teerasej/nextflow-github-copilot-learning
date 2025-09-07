# Challenge: Service and Mocking

Advanced challenge focusing on service architecture, API integration, and comprehensive testing with mocking strategies.

## Challenge Overview

This challenge tests advanced GitHub Copilot usage for:
- Creating service architectures
- API integration patterns
- Advanced mocking strategies
- Error handling and resilience
- Performance optimization
- Clean architecture principles

## Phase 1: Service Architecture Design

### 1.1 Plan the Architecture

1. Use Copilot Chat to design a comprehensive service layer:

```
@workspace design a clean service architecture for the photo gallery and event registration app that includes data repositories, API clients, caching layers, and error handling. Follow clean architecture principles.
```

### 1.2 Create Base Interfaces

1. Create `src/interfaces/` directory
2. Ask Copilot to generate base interfaces:

```
@workspace create TypeScript interfaces for a clean service architecture including Repository patterns, API clients, Cache interfaces, and Error handling types
```

Expected interfaces:
```typescript
// src/interfaces/Repository.ts
export interface Repository<T> {
  findAll(): Promise<T[]>;
  findById(id: string): Promise<T | null>;
  create(entity: Omit<T, 'id'>): Promise<T>;
  update(id: string, entity: Partial<T>): Promise<T>;
  delete(id: string): Promise<void>;
}

// src/interfaces/ApiClient.ts
export interface ApiClient {
  get<T>(url: string, config?: RequestConfig): Promise<ApiResponse<T>>;
  post<T>(url: string, data?: any, config?: RequestConfig): Promise<ApiResponse<T>>;
  put<T>(url: string, data?: any, config?: RequestConfig): Promise<ApiResponse<T>>;
  delete<T>(url: string, config?: RequestConfig): Promise<ApiResponse<T>>;
}
```

## Phase 2: Implement Service Layer

### 2.1 HTTP Client with Retry Logic

1. Create `src/services/HttpClient.ts`
2. Use Copilot to implement:

```
@workspace create a robust HTTP client with retry logic, request/response interceptors, timeout handling, and exponential backoff for network failures
```

### 2.2 Repository Implementations

1. Create repository implementations:

```
@workspace create PhotoRepository and EventRepository classes that implement the Repository interface with caching, optimistic updates, and conflict resolution
```

### 2.3 Cache Service

1. Create `src/services/CacheService.ts`:

```
@workspace implement a sophisticated caching service with TTL support, LRU eviction, and cache invalidation strategies for the photo gallery application
```

## Phase 3: Advanced Mocking Strategies

### 3.1 Service Mocks with MSW

1. Create comprehensive MSW handlers:

```
@workspace create Mock Service Worker handlers that simulate realistic API behavior including rate limiting, random failures, slow responses, and pagination for both photos and events APIs
```

Expected mock features:
- Realistic response times
- Random network failures (5% failure rate)
- Rate limiting simulation
- Pagination support
- Search functionality
- File upload simulation

### 3.2 Repository Test Doubles

1. Create test doubles for repositories:

```
@workspace create sophisticated mock implementations of PhotoRepository and EventRepository for testing that include configurable delays, failure modes, and state tracking
```

### 3.3 Dependency Injection Container

1. Create a simple DI container:

```
@workspace create a dependency injection container for managing service dependencies in tests and production, allowing easy swapping of implementations
```

## Phase 4: Error Handling and Resilience

### 4.1 Error Types and Handling

1. Create comprehensive error handling:

```
@workspace create a hierarchical error handling system with custom error types for network failures, validation errors, business logic errors, and system errors
```

### 4.2 Circuit Breaker Pattern

1. Implement circuit breaker:

```
@workspace implement a circuit breaker pattern for API calls to prevent cascading failures and provide fallback functionality
```

### 4.3 Retry Mechanisms

1. Create advanced retry logic:

```
@workspace create configurable retry mechanisms with exponential backoff, jitter, and different retry strategies for different types of failures
```

## Phase 5: Performance Optimization

### 5.1 Request Batching

1. Implement request batching:

```
@workspace create a request batching service that combines multiple API calls into single requests to improve performance and reduce server load
```

### 5.2 Background Sync

1. Create background synchronization:

```
@workspace implement background synchronization for offline support, conflict resolution, and eventual consistency between client and server
```

### 5.3 Virtual Scrolling for Large Lists

1. Implement virtual scrolling:

```
@workspace create a virtual scrolling component for the photo gallery that efficiently handles thousands of photos with lazy loading and memory management
```

## Phase 6: Advanced Testing Scenarios

### 6.1 Integration Tests with Real Network Conditions

1. Create realistic integration tests:

```
@workspace create integration tests that simulate various network conditions including slow connections, intermittent failures, and offline scenarios using MSW and custom network simulation
```

### 6.2 Property-Based Testing

1. Implement property-based tests:

```
@workspace create property-based tests using fast-check library to test the photo and event services with generated data and edge cases
```

### 6.3 Performance Testing

1. Create performance benchmarks:

```
@workspace create performance tests that measure API response times, memory usage, and render performance for the photo gallery under various load conditions
```

## Phase 7: Monitoring and Observability

### 7.1 Metrics Collection

1. Implement metrics:

```
@workspace create a metrics collection service that tracks API response times, error rates, cache hit ratios, and user interaction metrics
```

### 7.2 Logging Service

1. Create structured logging:

```
@workspace implement a structured logging service with different log levels, context propagation, and integration with external logging services
```

### 7.3 Health Checks

1. Implement health monitoring:

```
@workspace create health check endpoints and services that monitor API availability, database connectivity, and system resource usage
```

## Phase 8: Advanced Mock Scenarios

### 8.1 Chaos Engineering Tests

1. Create chaos tests:

```
@workspace create chaos engineering tests that randomly inject failures, delays, and data corruption to test system resilience
```

### 8.2 Time-based Testing

1. Implement time manipulation:

```
@workspace create tests that manipulate time to test time-dependent features like event registration deadlines, cache expiration, and rate limiting
```

### 8.3 Load Testing

1. Create load test scenarios:

```
@workspace create load testing scenarios that simulate high user concurrency, bulk operations, and stress testing for the photo upload and event registration systems
```

## Requirements Checklist

### ✅ Service Architecture
- [ ] Clean repository pattern implementation
- [ ] Dependency injection container
- [ ] Service layer abstraction
- [ ] Interface segregation

### ✅ HTTP Client Features
- [ ] Retry logic with exponential backoff
- [ ] Request/response interceptors
- [ ] Timeout handling
- [ ] Connection pooling

### ✅ Caching Strategy
- [ ] Multi-level caching (memory, localStorage, IndexedDB)
- [ ] Cache invalidation strategies
- [ ] TTL and LRU eviction
- [ ] Cache warming

### ✅ Error Handling
- [ ] Custom error hierarchy
- [ ] Circuit breaker pattern
- [ ] Graceful degradation
- [ ] Error reporting and tracking

### ✅ Testing Strategy
- [ ] Unit tests with mocks
- [ ] Integration tests with MSW
- [ ] Property-based testing
- [ ] Performance benchmarks
- [ ] Chaos engineering tests

### ✅ Performance Optimization
- [ ] Request batching
- [ ] Virtual scrolling
- [ ] Lazy loading
- [ ] Background synchronization
- [ ] Memory leak prevention

### ✅ Observability
- [ ] Structured logging
- [ ] Metrics collection
- [ ] Health monitoring
- [ ] Performance tracking

## Example Implementation Structure

```typescript
// src/services/PhotoService.ts
export class PhotoService {
  constructor(
    private repository: PhotoRepository,
    private httpClient: HttpClient,
    private cache: CacheService,
    private metrics: MetricsService
  ) {}

  async uploadPhoto(file: File): Promise<Photo> {
    const startTime = Date.now();
    
    try {
      // Validate file
      await this.validateFile(file);
      
      // Upload with retry logic
      const photo = await this.repository.create({
        file,
        metadata: this.extractMetadata(file)
      });
      
      // Update cache
      await this.cache.invalidate('photos');
      
      // Track metrics
      this.metrics.track('photo.upload.success', {
        duration: Date.now() - startTime,
        fileSize: file.size
      });
      
      return photo;
    } catch (error) {
      this.metrics.track('photo.upload.error', {
        duration: Date.now() - startTime,
        error: error.message
      });
      throw error;
    }
  }
}
```

## Evaluation Criteria

- **Architecture Quality**: Clean separation of concerns and SOLID principles
- **Error Resilience**: Comprehensive error handling and recovery
- **Testing Coverage**: Extensive testing with realistic scenarios
- **Performance**: Efficient implementation with optimization strategies
- **Observability**: Proper monitoring and debugging capabilities
- **Copilot Usage**: Effective use of Copilot for complex architectural decisions

## Bonus Challenges

1. **Event Sourcing**: Implement event sourcing for audit trails
2. **CQRS Pattern**: Separate read and write models
3. **Microservices Communication**: Simulate service-to-service communication
4. **Real-time Updates**: WebSocket integration for live updates
5. **Offline-first Architecture**: Complete offline functionality

This challenge will test your ability to use GitHub Copilot for complex architectural decisions and advanced development patterns.
