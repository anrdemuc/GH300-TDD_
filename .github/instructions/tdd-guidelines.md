# TDD Guidelines and Best Practices

## 📖 Overview

This document outlines the core principles and workflows for Test-Driven Development (TDD) in this repository.

## 🔄 The TDD Cycle

### 1. **RED** ❌ - Write a Failing Test
- Write a test that defines desired behavior
- The test should fail (no implementation exists)
- Focus on **what** the code should do, not **how**
- Tests serve as specifications

**Key Principles:**
- Test one behavior at a time
- Use clear, descriptive test names
- Keep tests focused and atomic
- Fail for the right reasons

**Example Red Phase:**
```javascript
// ❌ This test fails - no implementation yet
describe('Calculator', () => {
  test('should add two numbers', () => {
    const result = add(2, 3);
    expect(result).toBe(5);
  });
});
```

---

### 2. **GREEN** ✅ - Write Minimal Implementation
- Write the simplest code to pass the failing test
- Do NOT over-engineer or add extra features
- Do NOT optimize prematurely
- Prioritize passing the test, not perfection

**Key Principles:**
- Write only what's needed
- Make the test pass as quickly as possible
- Avoid adding untested functionality
- Keep implementation simple and direct

**Example Green Phase:**
```javascript
// ✅ Minimal implementation to pass the test
function add(a, b) {
  return a + b;
}
```

---

### 3. **REFACTOR** 🔧 - Improve Code Quality
- Improve code structure, readability, and maintainability
- Ensure all tests continue to pass
- Apply design patterns and principles
- Eliminate duplication

**Key Principles:**
- All tests must continue to pass
- Improve readability and clarity
- Remove code smells and duplication
- Maintain simplicity

**Example Refactor Phase:**
```javascript
// 🔧 Improved implementation while maintaining test passage
class Calculator {
  add(a, b) {
    this.validateNumbers(a, b);
    return a + b;
  }

  validateNumbers(...numbers) {
    numbers.forEach(num => {
      if (typeof num !== 'number') {
        throw new TypeError('Arguments must be numbers');
      }
    });
  }
}
```

---

## 📋 Workflow Rules

### ✅ DO:
- ✓ Write tests first
- ✓ Keep test cycles short (5-15 minutes)
- ✓ Write one test at a time
- ✓ Use descriptive test names
- ✓ Commit after each phase (Red → Green → Refactor → Commit)
- ✓ Maintain test coverage
- ✓ Refactor with confidence (tests catch regressions)

### ❌ DON'T:
- ✗ Write implementation before tests
- ✗ Write multiple tests at once
- ✗ Over-engineer during Green phase
- ✗ Skip the Refactor phase
- ✗ Ignore failing tests
- ✗ Write tests that pass immediately
- ✗ Leave tests broken

---

## 🎯 Test Structure

### Test Anatomy
Every test should follow this structure:

```javascript
describe('Feature/Component', () => {
  test('should [expected behavior] when [condition]', () => {
    // ARRANGE: Set up test data and preconditions
    const input = setupTestData();
    
    // ACT: Execute the code being tested
    const result = functionUnderTest(input);
    
    // ASSERT: Verify the expected outcome
    expect(result).toBe(expectedValue);
  });
});
```

### Naming Conventions
- Test files: `*.test.js` or `*.spec.js`
- Test suites: Describe the component/feature
- Test cases: Start with "should" - describe behavior

```javascript
describe('UserService', () => {
  describe('createUser', () => {
    test('should create a user with valid data', () => {});
    test('should throw error with invalid email', () => {});
    test('should hash password before storing', () => {});
  });
});
```

---

## 📊 Coverage Goals

- **Aim for 80%+ code coverage**
- Focus on critical paths and business logic
- Don't chase 100% coverage for utilities
- Prioritize meaningful tests over high numbers

---

## 🚀 Commit Strategy

Commit after completing each phase:

```
commit: RED - Add test for user authentication
commit: GREEN - Implement basic user authentication
commit: REFACTOR - Extract auth logic to service class
commit: main - User authentication feature complete
```

---

## 🔍 Common Patterns

### Testing State Changes
```javascript
test('should increment counter', () => {
  const counter = new Counter(0);
  counter.increment();
  expect(counter.value).toBe(1);
});
```

### Testing Errors
```javascript
test('should throw error for invalid input', () => {
  expect(() => {
    processData(null);
  }).toThrow(ValidationError);
});
```

### Testing Async Code
```javascript
test('should fetch user data', async () => {
  const user = await fetchUser(123);
  expect(user.id).toBe(123);
});
```

### Testing Side Effects
```javascript
test('should call logger on error', () => {
  const mockLogger = jest.fn();
  const service = new Service(mockLogger);
  service.handleError(new Error('test'));
  expect(mockLogger).toHaveBeenCalled();
});
```

---

## 🛠️ Tools & Setup

### Testing Framework
- Use Jest, Mocha, Vitest, or similar
- Configure in project root or dedicated config file
- Enable coverage reports

### Test Utilities
- Use fixtures for common test data
- Create helper functions for repeated setup
- Mock external dependencies

### CI/CD Integration
- Run tests on every commit
- Fail build if tests fail
- Require high coverage for merges

---

## 📚 Resources

### External Reading
- [Test Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [Clean Code: Chapter 9 - Unit Tests](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Jest Documentation](https://jestjs.io/)

### Internal Resources
- `.github/agents/TDD-red.agent.md` - Red phase details
- `.github/agents/TDD-green.agent.md` - Green phase details
- `.github/agents/TDD-refactor.agent.md` - Refactor phase details
- `.github/instructions/testing-standards.md` - Testing conventions

---

## ❓ FAQ

**Q: What if my test seems too simple?**
A: That's fine! Simple tests are good tests. They're faster and more maintainable. Add complexity only when needed.

**Q: Should I test private methods?**
A: No. Test the public interface. Private methods are implementation details and will be tested indirectly.

**Q: How many tests per feature?**
A: As many as needed to specify behavior. Typically 3-10 tests per function, depending on complexity.

**Q: What about performance?**
A: Write tests for performance-critical code, but don't optimize prematurely. Refactor phase is when you optimize.

**Q: Can I break a test temporarily?**
A: No. Always keep tests passing. If you need to experiment, create a branch.

---

**Version**: 1.0  
**Last Updated**: July 17, 2026
