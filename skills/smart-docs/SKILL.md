---
name: smart-docs
description: Generate comprehensive documentation from code. Analyzes exports, functions, classes, and types to produce README sections, API references, and module guides. Supports any language. Use when the user says /smart-docs, asks to document code, or wants to generate a README.
license: MIT
compatibility: Works with any language or framework
metadata:
  author: bmobot
  version: "1.0"
---

# Smart Docs Generator

Generate clear, comprehensive documentation from your codebase automatically.

## When to Activate

- User says `/smart-docs`, "document this", "generate docs", "write a README"
- User asks for API reference, module documentation, or function docs
- User wants to document a file, directory, or entire project

## Instructions

### Step 1: Determine Scope

Ask what needs documenting if unclear:

**Single file**: `/smart-docs src/auth.ts`
**Directory**: `/smart-docs src/services/`
**Full project**: `/smart-docs` (no arguments — documents the whole project)
**Specific focus**: `/smart-docs --api` or `/smart-docs --readme`

```bash
# Understand the project structure
find . -maxdepth 3 -type f \( -name "*.ts" -o -name "*.js" -o -name "*.py" -o -name "*.go" -o -name "*.rs" -o -name "*.java" \) | head -50

# Check for existing docs
ls -la README.md CHANGELOG.md docs/ 2>/dev/null

# Check package metadata
cat package.json 2>/dev/null | head -20
cat pyproject.toml 2>/dev/null | head -20
cat Cargo.toml 2>/dev/null | head -20
```

### Step 2: Analyze Code

Read the target files and extract:

#### Exports & Public API
- Exported functions, classes, types, constants
- Their signatures, parameters, return types
- Existing JSDoc/docstring/doc comments (preserve and enhance, don't replace)

#### Architecture
- Module dependencies (imports/requires)
- Inheritance hierarchies
- Design patterns used (factory, singleton, observer, etc.)
- Entry points and main flows

#### Configuration
- Environment variables used
- Config files read
- CLI arguments accepted
- Default values

#### Examples
- Look for existing tests — they're the best documentation
- Look for example files or usage in README
- Check for `examples/` or `__tests__/` directories

```bash
# Find test files for usage examples
find . -path "*/test*" -name "*.test.*" -o -name "*.spec.*" | head -20

# Find example files
find . -name "example*" -o -name "demo*" -o -name "sample*" 2>/dev/null | head -10
```

### Step 3: Choose Output Format

Based on the scope and user's request:

| Request | Output |
|---------|--------|
| Full project | README.md with all sections |
| Directory/module | Module guide (overview + API reference) |
| Single file | Function/class reference |
| `--api` flag | API reference only (signatures + descriptions) |
| `--readme` flag | README.md generation |
| `--inline` flag | Add/update JSDoc/docstrings in the source files |

### Step 4: Generate Documentation

#### For README.md (full project)

Generate these sections, skipping any that don't apply:

```markdown
# Project Name

One-line description from package metadata or main module docstring.

## Overview

2-3 sentences explaining what this project does, who it's for, and the key value proposition.

## Features

- Feature 1 — brief description
- Feature 2 — brief description

## Quick Start

### Prerequisites

- Node.js >= 18 (or whatever the project needs)
- Any required system dependencies

### Installation

```bash
npm install project-name
```

### Basic Usage

```language
// Minimal working example derived from tests or examples
import { mainThing } from 'project-name';

const result = mainThing({ option: 'value' });
```

## API Reference

### `functionName(param1, param2)`

Description of what it does.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| `param1` | `string` | required | What this param does |
| `param2` | `Options` | `{}` | Configuration options |

**Returns:** `ReturnType` — description

**Example:**
```language
const result = functionName('hello', { verbose: true });
```

### `ClassName`

Description of the class.

#### `new ClassName(options)`

Constructor description.

#### `instance.method()`

Method description.

## Configuration

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `PORT` | number | `3000` | Server port |
| `DEBUG` | boolean | `false` | Enable debug logging |

## Architecture

Brief description of the project's architecture, key modules, and how they interact.
Include a simple diagram if the project has 3+ interconnected modules:

```
[Client] → [API Router] → [Service Layer] → [Database]
                        → [Auth Middleware]
```

## Contributing

Standard contributing section (only if the project doesn't already have CONTRIBUTING.md).

## License

License from package metadata.
```

#### For Module/Directory Documentation

```markdown
# Module Name

Overview of the module's purpose and responsibilities.

## Exports

### Functions

#### `exportedFunction(params)`
Description, params table, return type, example.

### Classes

#### `ExportedClass`
Description, constructor, methods, properties.

### Types

#### `TypeName`
```typescript
type TypeName = {
  field: string;
  optional?: number;
};
```

### Constants

#### `CONSTANT_NAME`
Description and value.

## Internal Architecture

How the module's internal parts work together.

## Dependencies

What this module imports and why.
```

#### For Inline Documentation (`--inline`)

Add or update docstrings/JSDoc in the source files directly:

**TypeScript/JavaScript:**
```typescript
/**
 * Validates and processes a user registration request.
 *
 * Checks for duplicate emails, hashes the password, creates the user
 * record, and sends a verification email.
 *
 * @param input - Registration form data
 * @param input.email - User's email address (must be unique)
 * @param input.password - Plain text password (min 8 chars)
 * @param input.name - Display name
 * @returns The created user object (without password hash)
 * @throws {ConflictError} If email is already registered
 * @throws {ValidationError} If input fails validation
 *
 * @example
 * const user = await registerUser({
 *   email: 'alice@example.com',
 *   password: 'securepass123',
 *   name: 'Alice'
 * });
 */
```

**Python:**
```python
def register_user(email: str, password: str, name: str) -> User:
    """Validate and process a user registration request.

    Checks for duplicate emails, hashes the password, creates the user
    record, and sends a verification email.

    Args:
        email: User's email address (must be unique).
        password: Plain text password (min 8 chars).
        name: Display name.

    Returns:
        The created User object (without password hash).

    Raises:
        ConflictError: If email is already registered.
        ValidationError: If input fails validation.

    Example:
        >>> user = register_user("alice@example.com", "securepass123", "Alice")
    """
```

### Step 5: Present and Apply

Show the generated documentation and offer options:

1. **Write to file**: Create/update README.md, docs/*.md, or inline comments
2. **Copy to clipboard**: For pasting elsewhere
3. **Just show it**: Display without modifying files

Before writing, check for existing content:
```bash
# Don't overwrite existing README without asking
if [ -f README.md ]; then
  echo "README.md already exists ($(wc -l < README.md) lines)"
fi
```

If a README exists, offer to:
- Replace it entirely
- Merge specific sections (preserve custom content, update API reference)
- Generate as a separate file (README.generated.md)

## Quality Guidelines

### Writing Style
- **Be specific**: "Parses CSV files into typed arrays" not "Handles data processing"
- **Lead with the verb**: "Creates a new user" not "This function is used to create a new user"
- **Skip obvious params**: Don't document `options: Options` as "The options object" — describe what the options control
- **Show, don't tell**: A code example is worth 50 words of description
- **Match the project's voice**: Formal project → formal docs. Casual project → casual docs.

### What NOT to Document
- Private/internal functions (unless documenting internals is the goal)
- Auto-generated code
- Trivial getters/setters
- Framework boilerplate that's documented elsewhere
- Implementation details that change frequently

### Derive Examples from Tests
Tests are the most reliable source of usage examples. When you find a test like:
```typescript
it('should create a user with valid input', () => {
  const result = createUser({ name: 'Alice', email: 'alice@test.com' });
  expect(result.id).toBeDefined();
});
```

Transform it into a documentation example:
```typescript
const user = createUser({ name: 'Alice', email: 'alice@test.com' });
// => { id: '...', name: 'Alice', email: 'alice@test.com' }
```

## Edge Cases

- **No exports found**: Document the main entry point and key internal modules
- **Massive project** (100+ files): Focus on public API and main modules, offer to document specific subdirectories
- **Multiple languages**: Document each language section separately
- **Generated code mixed with handwritten**: Skip generated files (look for `@generated`, `AUTO-GENERATED`, `.generated.` in names)
- **Existing partial docs**: Enhance what exists rather than replacing — preserve custom sections the author wrote
- **Monorepo**: Ask which package to document, or generate a top-level overview linking to per-package docs
