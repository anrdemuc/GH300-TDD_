# GH300-TDD Repository

A structured repository implementing **Test-Driven Development (TDD)** best practices with AI agent guidance for each phase of the development cycle.

## 🎯 Purpose

This repository demonstrates and enforces the TDD methodology through:
- Clear separation of concerns across development phases
- AI agent instructions for Red, Green, and Refactor cycles
- Organized test-first development structure
- Best practices for sustainable software development

## 📁 Repository Structure

```
GH300-TDD_/
├── .github/
│   ├── agents/                    # AI Agent configurations for TDD phases
│   │   ├── TDD-red.agent.md      # Phase 1: Write failing tests
│   │   ├── TDD-green.agent.md    # Phase 2: Write minimal implementation
│   │   └── TDD-refactor.agent.md # Phase 3: Improve code quality
│   └── instructions/              # Agent behavior and guidelines
│       ├── tdd-guidelines.md      # TDD best practices
│       └── testing-standards.md   # Testing conventions
├── tests/                         # Test suite (test-first)
│   ├── unit/                      # Unit tests
│   ├── integration/               # Integration tests
│   └── fixtures/                  # Test data and fixtures
├── src/                           # Source code (implementation)
│   └── (organized by feature)
├── docs/                          # Documentation
└── scripts/                       # Development utilities
```

## 🔄 TDD Cycle

### Phase 1: Red (`.github/agents/TDD-red.agent.md`)
- Write tests that **fail** (no implementation yet)
- Focus on defining behavior and requirements
- Document expected outcomes

### Phase 2: Green (`.github/agents/TDD-green.agent.md`)
- Write minimal code to pass tests
- Prioritize making tests pass (not perfection)
- Avoid over-engineering

### Phase 3: Refactor (`.github/agents/TDD-refactor.agent.md`)
- Improve code quality, readability, and maintainability
- Ensure all tests still pass
- Apply design patterns and clean code principles

## 📋 Agent Instructions

All AI agents follow standardized guidelines defined in `.github/instructions/`:
- **tdd-guidelines.md**: Core TDD principles and workflow
- **testing-standards.md**: Testing conventions and best practices

## 🚀 Getting Started

1. **Read the TDD Guidelines**
   ```
   .github/instructions/tdd-guidelines.md
   ```

2. **Start with the Red Phase**
   - Consult `.github/agents/TDD-red.agent.md`
   - Write failing tests in `tests/`

3. **Move to Green Phase**
   - Consult `.github/agents/TDD-green.agent.md`
   - Implement minimal code in `src/`

4. **Refactor Phase**
   - Consult `.github/agents/TDD-refactor.agent.md`
   - Improve and optimize

## 🧪 Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run specific test suite
npm test -- tests/unit/

# Run with coverage
npm test -- --coverage
```

## 📚 Best Practices

### Testing First
- Write tests **before** implementation
- Tests serve as executable documentation
- Catch regressions early

### Red-Green-Refactor Cycle
- Keep cycles short (minutes, not hours)
- Commit frequently after each phase
- Maintain 100% test passage

### Code Quality
- Maintain high code coverage (aim for 80%+)
- Keep tests focused and readable
- Use descriptive test names

### Collaboration
- Each phase has dedicated agent instructions
- Follow conventions in `.github/instructions/`
- Communicate intent through tests

## 🔗 Resources

- [Test-Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530) - Kent Beck
- [TDD Best Practices](https://en.wikipedia.org/wiki/Test-driven_development)
- Agent Guidelines: See `.github/agents/` and `.github/instructions/`

## 📝 Contributing

1. Understand the TDD cycle by reading `.github/instructions/tdd-guidelines.md`
2. Choose your phase (Red, Green, or Refactor)
3. Consult the appropriate agent file
4. Follow testing standards in `.github/instructions/testing-standards.md`
5. Commit with clear messages indicating the phase

## 📄 License

[Add your license here]

## 🤝 Support

For questions about TDD workflow, refer to:
- `.github/instructions/tdd-guidelines.md`
- `.github/agents/TDD-*.agent.md` files
- Repository issues and discussions

---

**Last Updated**: July 17, 2026
