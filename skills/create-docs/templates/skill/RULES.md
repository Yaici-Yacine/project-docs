# Project Rules
> Last updated: {{DATE}}
> Project: {{PROJECT_NAME}}

## Tech Stack
- **Language**: {{LANGUAGE}}
- **Framework**: {{FRAMEWORK}}
- **Runtime**: {{RUNTIME}}

## Coding Rules

### General
- [ ] All functions must have explicit return types
- [ ] No `any` types (TypeScript projects)
- [ ] No commented-out code in production
- [ ] All async functions must have error handling

### Architecture
- [ ] {{ARCHITECTURE_PATTERN}} pattern enforced
- [ ] Feature modules are self-contained
- [ ] No circular dependencies between modules

### Testing
- [ ] Unit tests required for all business logic
- [ ] Test files colocated with source files
- [ ] Minimum coverage: {{COVERAGE}}%

## Forbidden Patterns
- ❌ Direct DOM manipulation outside of designated layers
- ❌ Business logic in UI components
- ❌ Hardcoded environment-specific values (use env vars)

## Performance Rules
- [ ] No synchronous blocking calls in hot paths
- [ ] Images optimized before commit
- [ ] Bundle size monitored

## Security Rules
- [ ] No secrets in source code
- [ ] All user input validated server-side
- [ ] Dependencies audited regularly (`npm audit`)
