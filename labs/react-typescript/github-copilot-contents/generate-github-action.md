# Generate GitHub Actions Workflow

Learn how to use GitHub Copilot to create CI/CD pipelines for your React TypeScript photo gallery application.

## Exercise 1: Basic CI/CD Pipeline

1. Open GitHub Copilot Chat
2. Ask Copilot to create a GitHub Actions workflow:

```
@workspace create a GitHub Actions workflow for a React TypeScript project that runs tests, builds the app, and deploys to GitHub Pages. Include linting, type checking, and security scanning.
```

3. Copilot will generate a comprehensive workflow file:

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run TypeScript type check
      run: npm run type-check
    
    - name: Run ESLint
      run: npm run lint
    
    - name: Run Prettier check
      run: npm run format:check
    
    - name: Run unit tests
      run: npm test -- --coverage --watchAll=false
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage/lcov.info
        flags: unittests
        name: codecov-umbrella
    
    - name: Run integration tests
      run: npm run test:integration
    
    - name: Build application
      run: npm run build
    
    - name: Upload build artifacts
      uses: actions/upload-artifact@v4
      with:
        name: build-files-${{ matrix.node-version }}
        path: build/

  security:
    runs-on: ubuntu-latest
    needs: test
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20.x'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run security audit
      run: npm audit --audit-level high
    
    - name: Run dependency vulnerability scan
      uses: snyk/actions/node@master
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
      with:
        args: --severity-threshold=high

  deploy:
    runs-on: ubuntu-latest
    needs: [test, security]
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20.x'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build for production
      run: npm run build
      env:
        CI: false
        GENERATE_SOURCEMAP: false
    
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./build
        cname: your-domain.com # Optional: replace with your domain
```

4. Create the workflow file using the "Create in new file" option

## Exercise 2: Advanced Workflow with Environment-specific Deployments

1. Ask Copilot for a more sophisticated workflow:

```
@workspace create a GitHub Actions workflow that deploys to different environments (staging and production) based on branch, includes environment protection rules, and performs automated testing against deployed applications
```

2. Copilot will generate an advanced workflow:

```yaml
# .github/workflows/deploy-environments.yml
name: Deploy to Environments

on:
  push:
    branches: [ main, develop ]
  release:
    types: [ published ]

env:
  NODE_VERSION: '20.x'

jobs:
  test:
    runs-on: ubuntu-latest
    outputs:
      coverage: ${{ steps.coverage.outputs.coverage }}
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run all tests
      run: npm run test:all -- --coverage
    
    - name: Extract coverage percentage
      id: coverage
      run: |
        COVERAGE=$(npx coverage-percentage ./coverage/lcov.info)
        echo "coverage=$COVERAGE" >> $GITHUB_OUTPUT
    
    - name: Comment PR with coverage
      if: github.event_name == 'pull_request'
      uses: actions/github-script@v7
      with:
        script: |
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: '📊 Test Coverage: ${{ steps.coverage.outputs.coverage }}%'
          })

  deploy-staging:
    runs-on: ubuntu-latest
    needs: test
    if: github.ref == 'refs/heads/develop'
    environment: staging
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build for staging
      run: npm run build
      env:
        REACT_APP_API_URL: ${{ secrets.STAGING_API_URL }}
        REACT_APP_ENVIRONMENT: staging
    
    - name: Deploy to Vercel (Staging)
      uses: amondnet/vercel-action@v25
      with:
        vercel-token: ${{ secrets.VERCEL_TOKEN }}
        vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
        vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
        working-directory: ./
        alias-domains: staging-photo-gallery.vercel.app

  deploy-production:
    runs-on: ubuntu-latest
    needs: test
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build for production
      run: npm run build
      env:
        REACT_APP_API_URL: ${{ secrets.PRODUCTION_API_URL }}
        REACT_APP_ENVIRONMENT: production
        GENERATE_SOURCEMAP: false
    
    - name: Deploy to Vercel (Production)
      uses: amondnet/vercel-action@v25
      with:
        vercel-token: ${{ secrets.VERCEL_TOKEN }}
        vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
        vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
        vercel-args: '--prod'
        working-directory: ./

  e2e-tests:
    runs-on: ubuntu-latest
    needs: [deploy-staging]
    if: github.ref == 'refs/heads/develop'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Install Playwright
      run: npx playwright install --with-deps
    
    - name: Run E2E tests against staging
      run: npm run test:e2e
      env:
        PLAYWRIGHT_TEST_BASE_URL: https://staging-photo-gallery.vercel.app
    
    - name: Upload E2E test results
      uses: actions/upload-artifact@v4
      if: failure()
      with:
        name: playwright-report
        path: playwright-report/
```

## Exercise 3: Performance and Accessibility Testing

1. Ask Copilot to add performance and accessibility testing:

```
@workspace add Lighthouse CI and accessibility testing to the GitHub Actions workflow for performance monitoring and a11y compliance
```

2. Copilot will add specialized testing jobs:

```yaml
  lighthouse:
    runs-on: ubuntu-latest
    needs: deploy-staging
    if: github.ref == 'refs/heads/develop'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
    
    - name: Install Lighthouse CI
      run: npm install -g @lhci/cli@0.12.x
    
    - name: Run Lighthouse CI
      run: lhci autorun
      env:
        LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
        LHCI_TARGET_URL: https://staging-photo-gallery.vercel.app
    
    - name: Upload Lighthouse results
      uses: actions/upload-artifact@v4
      with:
        name: lighthouse-report
        path: .lighthouseci/

  accessibility:
    runs-on: ubuntu-latest
    needs: deploy-staging
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Install axe-core CLI
      run: npm install -g @axe-core/cli
    
    - name: Run accessibility tests
      run: |
        axe https://staging-photo-gallery.vercel.app \
          --exit \
          --reporter=junit \
          --output=accessibility-results.xml
    
    - name: Upload accessibility results
      uses: actions/upload-artifact@v4
      with:
        name: accessibility-report
        path: accessibility-results.xml
```

## Exercise 4: Package.json Scripts

1. Ask Copilot to generate the necessary npm scripts:

```
@workspace create npm scripts in package.json for all the commands used in the GitHub Actions workflows including testing, linting, and building
```

2. Copilot will suggest adding these scripts:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "test": "vitest",
    "test:all": "npm run test:unit && npm run test:integration",
    "test:unit": "vitest run --coverage",
    "test:integration": "vitest run --testPathPattern=integration",
    "test:e2e": "playwright test",
    "preview": "vite preview",
    "lint": "eslint src/**/*.{ts,tsx} --max-warnings 0",
    "lint:fix": "eslint src/**/*.{ts,tsx} --fix",
    "format": "prettier --write src/**/*.{ts,tsx,css,md}",
    "format:check": "prettier --check src/**/*.{ts,tsx,css,md}",
    "type-check": "tsc --noEmit",
    "type-check:watch": "tsc --noEmit --watch",
    "analyze": "npm run build && npx bundle-analyzer build/static/js/*.js",
    "prepare": "husky install"
  }
}
```

## Exercise 5: Repository Configuration Files

1. Ask Copilot to create supporting configuration files:

```
@workspace create all necessary configuration files for the GitHub Actions workflow including ESLint, Prettier, TypeScript, and Lighthouse CI configurations
```

2. Copilot will generate multiple configuration files:

**`.eslintrc.json`:**
```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint", "react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

**`.prettierrc`:**
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

**`lighthouserc.js`:**
```javascript
module.exports = {
  ci: {
    collect: {
      url: ['http://localhost:5173'],
      startServerCommand: 'npm run dev',
      startServerReadyPattern: 'Local:',
      startServerReadyTimeout: 30000,
    },
    assert: {
      assertions: {
        'categories:performance': ['warn', { minScore: 0.9 }],
        'categories:accessibility': ['error', { minScore: 0.95 }],
        'categories:best-practices': ['warn', { minScore: 0.9 }],
        'categories:seo': ['warn', { minScore: 0.9 }],
      },
    },
    upload: {
      target: 'temporary-public-storage',
    },
  },
};
```

## Exercise 6: Security and Dependency Management

1. Ask for additional security workflows:

```
@workspace create a separate GitHub Actions workflow for dependency updates, security scanning, and automated dependency management using Dependabot and CodeQL
```

## Best Practices for CI/CD with Copilot

- **Environment Variables**: Use secrets for sensitive data
- **Caching**: Cache dependencies for faster builds
- **Parallel Jobs**: Run independent tasks in parallel
- **Environment Protection**: Use GitHub environments for deployment approval
- **Artifact Management**: Store build artifacts and test results

## Monitoring and Notifications

Ask Copilot to add monitoring:

```
@workspace add Slack notifications and monitoring alerts to the GitHub Actions workflow for deployment status and test failures
```

## Next Steps

Continue to [Event Registration Challenge](./event-registration.md) to implement a complete feature using all the techniques learned.
