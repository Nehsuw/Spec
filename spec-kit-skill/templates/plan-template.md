# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]`
**Date**: [DATE]
**Spec**: [link to spec.md]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]

**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]

**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]

**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]

**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]

**Project Type**: [single/web/mobile - determines source structure]

**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]

**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]

**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

[Gates determined based on constitution file]

- ✅/❌ [Check Item 1]
- ✅/❌ [Check Item 2]
- ✅/❌ [Check Item 3]

## Architecture Decisions

### Decision 1: [Title]

**Decision**: [Specific solution]

**Rationale**:
- Reason 1
- Reason 2
- Reason 3

**Alternatives Considered**: [Rejected alternatives and reasons]

**Trade-offs**:
- Pros: [Advantages]
- Cons: [Disadvantages]

### Decision 2: [Title]

...

## API Design

### [API Name 1]

```
[METHOD] /api/v1/[endpoint]

Request:
{
  "field1": "value1",
  "field2": "value2"
}

Response:
{
  "success": true,
  "data": {}
}

Errors:
- 400: [Error Description]
- 401: [Error Description]
- 500: [Error Description]
```

### [API Name 2]

...

## Data Model

[Choose appropriate format based on technology]

### SQL Example

```sql
CREATE TABLE entity_name (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_name (name)
);
```

### ORM Example

```python
class Entity:
    id: int
    name: str
    created_at: datetime
```

## Project Structure

### Documentation (this feature)

```
specs/[###-feature]/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
└── contracts/           # Phase 1 output
```

### Source Code (repository root)

```
# Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# Option 2: Web application
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# Option 3: Mobile + API
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure]
```

**Structure Decision**: [Document the selected structure]

## Security Considerations

1. [Security consideration 1]
2. [Security consideration 2]
3. [Security consideration 3]

## Performance Considerations

1. [Performance optimization 1]
2. [Performance optimization 2]
3. [Performance optimization 3]

## Complexity Tracking

**Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|---|---|---|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
