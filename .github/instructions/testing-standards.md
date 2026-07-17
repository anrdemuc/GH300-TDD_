# Testing Standards & Conventions

## 📋 Overview

This document establishes the testing standards and conventions for this repository to ensure consistency, maintainability, and high-quality test suites.

---

## 📁 Test File Organization

### Directory Structure
```
tests/
├── unit/                    # Unit tests for individual functions/classes
├── integration/             # Integration tests for multiple components
├── fixtures/                # Test data, mocks, and fixtures
│   ├── data/               # JSON/object test data
│   ├── mocks/              # Mock functions and services
│   └── factories.js        # Test data factories
└── helpers/                # Test utilities and helpers
```

### File Naming Conventions
- Unit tests: `tests/unit/[featureName].test.js`
- Integration tests: `tests/integration/[feature].integration.test.js`
- Test fixtures: `tests/fixtures/[component].fixtures.js`
- Test helpers: `tests/helpers/[utilName].helper.js`

**Example:**
```
tests/
├── unit/
│   ├── calculator.test.js
│   ├── userService.test.js
│   └── validator.test.js
├── integration/
│   ├── auth.integration.test.js
│   └── database.integration.test.js
└── fixtures/
    ├── user.fixtures.js
    └── mocks.js
```

---

## ✍️ Test Writing Standards

### 1. Naming Conventions

**Test Suite Names:**
```javascript
describe('Calculator', () => {
  // ✓ Good: Describes the component/feature
  describe('add', () => {
    // ✓ Good: Describes specific functionality
  });
});
```

**Test Case Names:**
```javascript
// ✓ Good: Clear behavior description
test('should return sum of two positive numbers', () => {});

// ✓ Good: Includes error condition
test('should throw error when given non-numeric input', () => {});

// ✓ Good: Specifies edge case
test('should handle negative numbers correctly', () => {});

// ✗ Bad: Too vague
test('addition works', () => {});

// ✗ Bad: No "should"
test('returns result', () => {});
```

### 2. Test Structure (AAA Pattern)

Every test should follow Arrange-Act-Assert:

```javascript
test('should calculate total price with tax', () => {
  // ARRANGE: Set up test data
  const price = 100;
  const taxRate = 0.1;
  const calculator = new PriceCalculator();

  // ACT: Execute the function
  const total = calculator.addTax(price, taxRate);

  // ASSERT: Verify the result
  expect(total).toBe(110);
});
```

### 3. Test Size Guidelines

**Unit Tests:**
- 1-5 lines typically
- Test one behavior
- No external dependencies
- Fast execution (< 10ms)

**Integration Tests:**
- 5-20 lines typically
- May involve multiple components
- Can involve external services
- May be slower (< 1s)

### 4. Assertions

**Use Clear, Specific Assertions:**

```javascript
// ✓ Good: Specific matcher
expect(user.age).toBe(25);

// ✓ Good: Clear error message
expect(user.email).toMatch(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);

// ✓ Good: Semantic assertion
expect(isValid).toBe(true);

// ✗ Avoid: Implicit truthy checks
expect(user).toBeTruthy();

// ✗ Avoid: Multiple assertions in one test
expect(user.name).toBe('John');
expect(user.age).toBe(25);
expect(user.email).toBe('john@example.com');
// (Better: separate into three tests)
```

**Multiple Assertions - When Appropriate:**

```javascript
// ✓ Good: Related assertions for complex validation
test('should create user with all required fields', () => {
  const user = createUser({ name: 'John', email: 'john@example.com' });
  
  expect(user).toBeDefined();
  expect(user.id).toBeTruthy();
  expect(user.createdAt).toBeInstanceOf(Date);
  // All assertions verify one behavior: successful user creation
});
```

---

## 🔍 Unit Testing Standards

### Isolation & Mocking

```javascript
describe('UserService', () => {
  let userService;
  let mockDatabase;
  let mockLogger;

  beforeEach(() => {
    // ✓ Mock external dependencies
    mockDatabase = {
      save: jest.fn().mockResolvedValue({ id: 1 }),
      findById: jest.fn(),
    };
    mockLogger = { log: jest.fn() };

    // ✓ Create service with mocks
    userService = new UserService(mockDatabase, mockLogger);
  });

  test('should save user to database', async () => {
    const user = { name: 'John', email: 'john@example.com' };
    
    await userService.createUser(user);
    
    expect(mockDatabase.save).toHaveBeenCalledWith(user);
  });
});
```

### Coverage Guidelines

**Target Coverage by Type:**
- Statements: 80%+
- Branches: 75%+
- Functions: 80%+
- Lines: 80%+

**Exclude from Coverage:**
- Generated code
- Dependencies (node_modules)
- Test fixtures
- Configuration files

### Test Data

**Use Fixtures for Common Data:**

```javascript
// ✓ Good: Centralized test data
const validUser = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 25,
};

test('should create valid user', () => {
  const user = createUser(validUser);
  expect(user.email).toBe(validUser.email);
});
```

**Use Factories for Complex Objects:**

```javascript
// Factory pattern for flexible test data
function createUserFactory(overrides = {}) {
  return {
    id: Math.random(),
    name: 'Default Name',
    email: 'test@example.com',
    role: 'user',
    ...overrides,
  };
}

test('should create admin user', () => {
  const admin = createUserFactory({ role: 'admin' });
  expect(admin.role).toBe('admin');
});
```

---

## 🔌 Integration Testing Standards

### Testing Multiple Components

```javascript
describe('User Creation Flow', () => {
  let database;
  let emailService;
  let userService;

  beforeEach(async () => {
    // ✓ Set up real or test database
    database = new TestDatabase();
    await database.initialize();

    // ✓ Create service instances
    emailService = new MockEmailService();
    userService = new UserService(database, emailService);
  });

  afterEach(async () => {
    // ✓ Clean up after tests
    await database.clear();
    await database.disconnect();
  });

  test('should create user and send welcome email', async () => {
    const result = await userService.createUser({
      name: 'John',
      email: 'john@example.com',
    });

    expect(result.id).toBeTruthy();
    expect(emailService.sendWelcomeEmail).toHaveBeenCalledWith('john@example.com');
  });
});
```

### Test Data Cleanup

```javascript
// ✓ Always clean up after integration tests
afterEach(async () => {
  await database.deleteUser(testUserId);
  await cache.clear();
});

afterAll(async () => {
  await database.disconnect();
  await server.close();
});
```

---

## 🧪 Edge Cases & Error Testing

### Testing Error Conditions

```javascript
describe('Validation', () => {
  test('should throw error for null input', () => {
    expect(() => validate(null)).toThrow(ValidationError);
  });

  test('should throw specific error message', () => {
    expect(() => validate(null)).toThrow('Input cannot be null');
  });

  test('should handle empty strings', () => {
    expect(() => validate('')).toThrow();
  });

  test('should handle undefined', () => {
    expect(() => validate(undefined)).toThrow();
  });
});
```

### Testing Boundary Conditions

```javascript
test('should handle edge case: zero', () => {
  expect(divide(10, 0)).toThrow();
});

test('should handle maximum safe integer', () => {
  const result = add(Number.MAX_SAFE_INTEGER, 1);
  expect(result).toBeDefined();
});

test('should handle very large arrays', () => {
  const largeArray = Array(10000).fill(1);
  const result = processArray(largeArray);
  expect(result).toBeDefined();
});
```

---

## ⏱️ Async Testing

### Promises

```javascript
describe('Async Operations', () => {
  test('should resolve with user data', () => {
    return fetchUser(123).then(user => {
      expect(user.id).toBe(123);
    });
  });

  // Or using async/await
  test('should fetch user data', async () => {
    const user = await fetchUser(123);
    expect(user.id).toBe(123);
  });
});
```

### Timeouts

```javascript
// ✓ Set appropriate timeout for long operations
test('should complete data processing', async () => {
  const result = await processLargeDataset();
  expect(result).toBeDefined();
}, 10000); // 10 second timeout

// ✓ Handle promises correctly
test('should handle multiple async calls', async () => {
  await Promise.all([
    fetchUser(1),
    fetchUser(2),
    fetchUser(3),
  ]);
  // assertions...
});
```

---

## 📸 Snapshot Testing

**Use Sparingly:**

```javascript
// ✓ Good for complex, stable structures
test('should render user profile correctly', () => {
  const component = render(<UserProfile user={user} />);
  expect(component).toMatchSnapshot();
});

// ✗ Avoid: Snapshots of frequently changing data
test('should display current date', () => {
  expect(getCurrentDate()).toMatchSnapshot(); // Bad idea!
});
```

---

## 🚀 Performance Testing

### Marking Performance Tests

```javascript
describe.skip('Performance Tests', () => {
  // Only run during performance profiling
  test('should process 10k items in < 1s', () => {
    const start = performance.now();
    const result = processItems(largeDataset);
    const duration = performance.now() - start;
    expect(duration).toBeLessThan(1000);
  });
});
```

---

## ✅ Pre-Commit Testing Checklist

Before committing tests:

- [ ] All tests pass locally
- [ ] No `.only` or `.skip` left in test files
- [ ] Test coverage meets standards
- [ ] Test names clearly describe behavior
- [ ] Tests are properly organized
- [ ] No hardcoded file paths or secrets
- [ ] Mock data cleaned up after tests
- [ ] Async operations properly handled
- [ ] Edge cases covered
- [ ] No flaky or timing-dependent tests

---

## 🔧 Tools & Configuration

### Recommended Tools
- **Testing Framework**: Jest, Mocha, Vitest
- **Assertion Library**: Chai, Jest assertions
- **Mocking**: Jest mocks, Sinon
- **Coverage**: nyc, Jest coverage
- **Snapshot**: Built into most frameworks

### Configuration Example (Jest)

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'node',
  collectCoverageFrom: ['src/**/*.js'],
  coverageThreshold: {
    global: {
      branches: 75,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
  testMatch: ['**/tests/**/*.test.js'],
};
```

---

## 📚 Resources

- Jest: https://jestjs.io/
- Testing Best Practices: `.github/instructions/tdd-guidelines.md`
- TDD Phases: `.github/agents/`

---

**Version**: 1.0  
**Last Updated**: July 17, 2026
