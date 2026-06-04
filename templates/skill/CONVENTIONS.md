# Project Conventions
> Last updated: {{DATE}}
> Project: {{PROJECT_NAME}}

## Naming Conventions

### Files & Folders
| Type | Convention | Example |
|------|-----------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase with `use` prefix | `useAuthState.ts` |
| Utils | camelCase | `formatDate.ts` |
| Types | PascalCase | `UserType.ts` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Test files | `[name].test.[ext]` | `auth.test.ts` |

### Variables & Functions
| Type | Convention | Example |
|------|-----------|---------|
| Variables | camelCase | `userName` |
| Functions | camelCase | `getUserById()` |
| Classes | PascalCase | `AuthService` |
| Interfaces | PascalCase with `I` prefix (optional) | `IUserRepository` |
| Enums | PascalCase | `UserRole` |

## Folder Structure
```
src/
├── components/     ← Reusable UI components
├── features/       ← Feature modules (self-contained)
│   └── [feature]/
│       ├── index.ts
│       ├── [feature].service.ts
│       └── [feature].test.ts
├── hooks/          ← Shared custom hooks
├── lib/            ← Third-party integrations
├── types/          ← Shared TypeScript types
└── utils/          ← Pure utility functions
```

## Import Order
1. Node built-ins
2. External packages
3. Internal absolute imports (`@/`)
4. Relative imports

## Git Conventions

### Branch Names
- Feature: `feat/[description]`
- Bug fix: `fix/[description]`
- Docs: `docs/[description]`

### Commit Messages
Format: `[type]([scope]): [message]`
Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Example: `feat(auth): add OAuth2 login support`
