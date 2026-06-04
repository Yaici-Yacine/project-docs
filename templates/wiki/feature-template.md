# Feature: {{FEATURE_NAME}}
> Last updated: {{DATE}}
> Status: {{STATUS}} <!-- Active | Deprecated | In Progress -->

## Purpose
{{PURPOSE_DESCRIPTION}}

## Location in Codebase
```
{{MAIN_FOLDER_PATH}}/
├── {{KEY_FILE_1}}     ← {{FILE_1_ROLE}}
├── {{KEY_FILE_2}}     ← {{FILE_2_ROLE}}
└── {{KEY_FILE_3}}     ← {{FILE_3_ROLE}}
```

## How It Works

### Overview
{{HIGH_LEVEL_EXPLANATION}}

### Step-by-Step Flow
1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

## Key Files

| File | Role |
|------|------|
| `{{FILE_PATH}}` | {{FILE_ROLE}} |

## Dependencies

### Internal
- `{{INTERNAL_MODULE}}` — {{WHY_NEEDED}}

### External
- `{{PACKAGE_NAME}}` — {{WHY_NEEDED}}

## Entry Points
```typescript
// How other modules use this feature
import { {{MAIN_EXPORT}} } from '{{IMPORT_PATH}}'
```

## Configuration
{{CONFIGURATION_DESCRIPTION}}

## Known Issues / TODOs
- [ ] {{TODO_ITEM}}

## Related Features
- [{{RELATED_FEATURE}}](./{{related-slug}}.md)
