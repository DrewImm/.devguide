# Dev Guide

## Git & Branching Guide

### Branch Naming Convention

Use the ticket number for branch names:

**Examples:**
- `PROJ-123`
- `PROJ-456`
- `PROJ-789`
- `PROJ-234`
- `PROJ-567`

**Benefits:**
- **Ultra-simple** - Just the ticket number, nothing else
- **Clean & Zero ambiguity** - No decisions about prefixes or formatting
- **Perfect for automation** - CI/CD tools can easily parse and link
- **Clean repository** - Branch list is very clean and easy to scan
- **Fast to type** - Developers can create branches quickly
- **Direct mapping** - Branch name = ticket number

**Benefits of ticket integration:**
- **Traceability** - Easy to track work back to requirements
- **Automation** - CI/CD can auto-link commits to tickets
- **Context** - Team members can quickly understand the purpose
- **Reporting** - Better project management and velocity tracking

### Workflow

#### Branching Strategy: Git Flow

This project uses **Git Flow** for structured release management and better quality control.

**Key Benefits:**
- **Release control** - Can prepare and polish releases before going live
- **Stability** - `main` stays clean and production-ready
- **Feature integration testing** - Can test multiple features together in `dev`
- **Scheduled releases** - Better for coordinated, planned releases
- **Hotfix isolation** - Clear path for emergency production fixes
- **Quality gates** - More checkpoints before production

#### Branch Types

**Forever Branches:**
- `main` - Production-ready code, contains only released versions
- `dev` - Integration branch where features are merged for testing

**Supporting Branches:**
- `v1.2.3` - Release Branch
- `PROJ-123` - Feature/fix/task branch

#### Feature Development

1. **Start a new feature** from `dev`:
   ```bash
   git checkout dev
   git pull
   git checkout -b PROJ-123
   ```

2. **Push your branch**:
   ```bash
   git push -u origin PROJ-123
   ```

3. **Work on your feature** and commit regularly:
   ```bash
   npm test
   git add .
   git commit -m "feat: add user authentication logic"
   git push
   ```

4. **Create Pull Request** to merge into `dev` when ready

5. **Merge feature** into `dev` after review. Use a squash commit!

#### Release Process

1. **Create release branch** from `dev`:
   ```bash
   git checkout dev
   git pull origin dev
   git checkout -b v1.2.3
   ```

2. **Bump Version Code & Update**
   Bump the version in `package.json`. Then install:

   ```bash
   npm i && npm update && npm audit fix
   npm test
   git add package.json package-lock.json
   git commit -m "v1.2.3"
   git push -u
   ```

3. **Create PR**
   Create and merge a PR for the release into the main branch. Use a merge commit, do not squash!

4. **Tag Release**
   Tag the release and generate release notes

5. **Publish Release**
   ```bash
   git checkout main
   git pull
   npm run script publish

   # If publish script fails because of npm OTP:
   cd dist/src && npm publish
   ```

   **Note** - *When publishing for the first time, you will need to use `npm publish --access=public` from `dist/src`*

6. **Announce Release**
   - Blog posts

#### Hotfix Process

1. **Create hotfix branch** from `main`:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b PROJ-789
   git push -u
   ```

2. **Fix the issue** and commit:
   ```bash
   git add .
   git commit -m "fix: patch security vulnerability"
   git push
   ```

3. **Create PR**
   Create and merge PR into main

4. **Sync Dev**
   ```bash
   git checkout dev
   git pull
   git merge origin/main
   git push
   ```

#### Commit Message Convention
Use conventional commits format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Formatting changes (no code change)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

Example: `feat: add password reset functionality`
