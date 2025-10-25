# 🤝 Contributing to AlumniChain

Terima kasih atas minat Anda untuk berkontribusi pada AlumniChain! Panduan ini akan membantu Anda memulai.

## 📋 Table of Contents
1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Workflow](#development-workflow)
4. [Coding Standards](#coding-standards)
5. [Commit Guidelines](#commit-guidelines)
6. [Pull Request Process](#pull-request-process)
7. [Testing](#testing)
8. [Documentation](#documentation)

## 📜 Code of Conduct

### Our Pledge
We are committed to providing a welcoming and inspiring community for everyone.

### Our Standards
- ✅ Using welcoming and inclusive language
- ✅ Being respectful of differing viewpoints
- ✅ Gracefully accepting constructive criticism
- ✅ Focusing on what is best for the community

## 🚀 Getting Started

### 1. Fork the Repository
```bash
# Click "Fork" on GitHub
# Then clone your fork
git clone https://github.com/mrbrightsides/alumnichain.git
cd alumnichain
```

### 2. Set Up Development Environment
```bash
# Install dependencies
npm install

# Set up SpacetimeDB
curl --proto '=https' --tlsv1.2 -sSf https://install.spacetimedb.com | sh

# Start development server
npm run dev
```

### 3. Create a Branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

## 🔄 Development Workflow

### Branch Naming Convention
```
feature/  - New features
fix/      - Bug fixes
docs/     - Documentation updates
refactor/ - Code refactoring
test/     - Adding tests
chore/    - Maintenance tasks
```

### Examples
```bash
git checkout -b feature/add-alumni-search
git checkout -b fix/job-application-error
git checkout -b docs/update-readme
```

## 💻 Coding Standards

### TypeScript Guidelines

#### 1. Strict Typing
```typescript
// ✅ Good
function processAlumni(alumni: Alumni): ProcessedAlumni {
  return {
    id: alumni.id,
    name: alumni.name,
  };
}

// ❌ Bad
function processAlumni(alumni: any) {
  return {
    id: alumni.id,
    name: alumni.name,
  };
}
```

#### 2. Interface Definitions
```typescript
// ✅ Good - Define interfaces at top of file
interface Alumni {
  id: number;
  name: string;
  email: string;
}

// ❌ Bad - Inline types
function updateAlumni(data: { id: number; name: string }) {
  // ...
}
```

#### 3. Explicit Return Types
```typescript
// ✅ Good
function calculateXP(actions: Action[]): number {
  return actions.reduce((sum, action) => sum + action.xp, 0);
}

// ❌ Bad
function calculateXP(actions: Action[]) {
  return actions.reduce((sum, action) => sum + action.xp, 0);
}
```

### React Guidelines

#### 1. Component Structure
```typescript
'use client';

import type { FC } from 'react';
import { useState, useEffect } from 'react';

interface ComponentProps {
  title: string;
  onAction: () => void;
}

export const Component: FC<ComponentProps> = ({ title, onAction }) => {
  // Hooks
  const [state, setState] = useState<string>('');

  // Effects
  useEffect(() => {
    // ...
  }, []);

  // Handlers
  const handleClick = (): void => {
    onAction();
  };

  // Render
  return (
    <div>
      <h1>{title}</h1>
      <button onClick={handleClick}>Click</button>
    </div>
  );
};
```

#### 2. Custom Hooks
```typescript
// ✅ Good - Reusable hook
function useAlumniData(alumniId: number) {
  const [alumni, setAlumni] = useState<Alumni | null>(null);
  const [loading, setLoading] = useState<boolean>(true);

  useEffect(() => {
    // Fetch logic
  }, [alumniId]);

  return { alumni, loading };
}

// Usage
const { alumni, loading } = useAlumniData(123);
```

### CSS/Tailwind Guidelines

#### 1. Class Organization
```typescript
// ✅ Good - Organized by category
<div className="
  flex items-center justify-between
  px-4 py-2
  bg-slate-900 border border-purple-500/20
  rounded-lg shadow-lg
  hover:border-purple-500/50 transition-colors
">
```

#### 2. Custom Classes (if needed)
```css
/* globals.css */
@layer components {
  .glass-card {
    @apply bg-slate-900/50 backdrop-blur-md border border-white/10;
  }
}
```

### SpacetimeDB Guidelines

#### 1. Reducer Structure
```rust
#[spacetimedb::reducer]
pub fn create_entity(
    ctx: &ReducerContext,
    name: String,
    value: u64,
) -> Result<(), String> {
    // 1. Validate input
    if name.is_empty() {
        return Err("Name cannot be empty".into());
    }

    // 2. Check permissions
    let sender = ctx.sender;
    
    // 3. Perform operation
    Entity::insert(Entity { ... })?;

    // 4. Return success
    Ok(())
}
```

#### 2. Table Definitions
```rust
#[spacetimedb::table(name = entity)]
pub struct Entity {
    #[primarykey]
    #[autoinc]
    pub id: u64,
    pub name: String,
    pub created_at: u64,
    #[index]
    pub status: Status,
}
```

## 📝 Commit Guidelines

### Commit Message Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- **feat:** New feature
- **fix:** Bug fix
- **docs:** Documentation changes
- **style:** Code style changes (formatting)
- **refactor:** Code refactoring
- **test:** Adding tests
- **chore:** Maintenance tasks

### Examples
```bash
feat(jobs): add salary filter to job board

- Added min/max salary inputs
- Implemented filtering logic
- Updated job card display

Closes #123

---

fix(auth): resolve wallet connection timeout

The wallet connection was timing out due to 
async initialization issues.

- Added retry logic
- Improved error handling
- Updated timeout duration

Fixes #456
```

### Commit Best Practices
- ✅ Write clear, descriptive messages
- ✅ Use present tense ("add" not "added")
- ✅ Keep subject line under 50 characters
- ✅ Reference issue numbers when applicable
- ❌ Don't commit commented-out code
- ❌ Don't commit `console.log` statements

## 🔍 Pull Request Process

### 1. Before Submitting
```bash
# Ensure code compiles
npm run build

# Check for TypeScript errors
npx tsc --noEmit

# Format code (if using Prettier)
npm run format
```

### 2. PR Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Tested locally
- [ ] Added/updated tests
- [ ] All tests passing

## Screenshots (if applicable)
Add screenshots here

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings generated
```

### 3. Review Process
1. Maintainer reviews code
2. Feedback provided (if needed)
3. You make requested changes
4. Maintainer approves and merges

## 🧪 Testing

### Running Tests
```bash
# Type check
npm run build

# Future: Unit tests
npm test

# Future: E2E tests
npm run test:e2e
```

### Writing Tests
```typescript
// Example: Testing utility function
import { calculateXP } from '@/lib/utils';

describe('calculateXP', () => {
  it('should sum XP from multiple actions', () => {
    const actions = [
      { type: 'profile', xp: 50 },
      { type: 'apply', xp: 25 },
    ];
    
    expect(calculateXP(actions)).toBe(75);
  });
});
```

## 📚 Documentation

### Code Comments
```typescript
// ✅ Good - Explain WHY, not WHAT
// Retry connection because SpacetimeDB may not be ready on first attempt
await retryConnection();

// ❌ Bad - Obvious comment
// Call retry connection function
await retryConnection();
```

### Component Documentation
```typescript
/**
 * AlumniCard displays alumni profile information
 * 
 * @param alumni - Alumni data object
 * @param onConnect - Callback when connection is requested
 * @returns Rendered card component
 * 
 * @example
 * <AlumniCard
 *   alumni={alumniData}
 *   onConnect={handleConnect}
 * />
 */
export const AlumniCard: FC<AlumniCardProps> = ({ alumni, onConnect }) => {
  // ...
};
```

### README Updates
- Update README.md when adding new features
- Add examples for new functionality
- Update installation steps if dependencies change

## 🐛 Reporting Bugs

### Bug Report Template
```markdown
**Describe the bug**
A clear description of what the bug is.

**To Reproduce**
Steps to reproduce:
1. Go to '...'
2. Click on '...'
3. See error

**Expected behavior**
What you expected to happen.

**Screenshots**
If applicable, add screenshots.

**Environment:**
- OS: [e.g. Windows, macOS]
- Browser: [e.g. Chrome, Safari]
- Version: [e.g. 1.0.0]

**Additional context**
Add any other context about the problem.
```

## 💡 Feature Requests

### Feature Request Template
```markdown
**Is your feature request related to a problem?**
A clear description of the problem.

**Describe the solution you'd like**
A clear description of what you want to happen.

**Describe alternatives you've considered**
Other solutions you've thought about.

**Additional context**
Add any other context or screenshots.
```

## 🎉 Recognition

Contributors will be recognized in:
- README.md contributors section
- GitHub contributors graph
- Release notes (for significant contributions)

## 📞 Getting Help

- **Questions:** Open a GitHub Discussion
- **Bugs:** Open a GitHub Issue
- **Security:** Email support@elpeef.com

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for contributing to AlumniChain! 🚀**

Every contribution, no matter how small, helps make this project better for the Universitas Bina Darma alumni community.
