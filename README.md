# App Monorepo with Release Branches

A monorepo example containing multiple Go applications with separate long-living release branches for version management.

## 📁 Repository Structure

```text
.
├── apps/
│   ├── foo/                    # Foo Go application
│   │   ├── main.go
│   │   ├── go.mod
│   │   └── CHANGELOG.md
│   └── bar/                    # Bar Go application
│       ├── main.go
│       ├── go.mod
│       └── CHANGELOG.md
├── .github/
│   ├── workflows/
│   │   └── release-please.yml  # Release automation workflow
│   └── release-please-config.json
└── .release-please-manifest.json
```

## 🚀 Applications

### Foo Application

- **Location**: `apps/foo/`
- **Port**: 8080
- **Description**: Simple HTTP server serving Foo application

### Bar Application

- **Location**: `apps/bar/`
- **Port**: 8081
- **Description**: Simple HTTP server serving Bar application

## 🌿 Release Branch Strategy

This repository uses **separate release branches** for each application and major version:

### Branch Structure

- `main` - Main development branch
- `release/foo/v1.x` - Foo application version 1.x releases
- `release/foo/v2.x` - Foo application version 2.x releases
- `release/bar/v1.x` - Bar application version 1.x releases
- `release/bar/v2.x` - Bar application version 2.x releases

### Workflow

1. **Development**: All active development happens on `main` branch
2. **Version Branching**: When ready for a major version:
   - Create release branch (e.g., `release/foo/v1.x`, `release/bar/v2.x`)
   - Cherry-pick or merge specific commits
3. **Release Management**: Release Please monitors all branches and creates:
   - Release PRs with automatic changelog generation
   - GitHub releases with semantic versioning
   - Git tags (e.g., `foo-v1.0.0`, `bar-v2.1.0`)

## 🤖 Release Please Configuration

### Automated Release Management

Release Please is configured to:

- ✅ Monitor multiple branches (`main`, `release/foo/v1.x`, `release/foo/v2.x`, `release/bar/v1.x`, `release/bar/v2.x`)
- ✅ Generate separate PRs for each application
- ✅ Create changelogs using [Conventional Commits](https://www.conventionalcommits.org/)
- ✅ Update version numbers in `go.mod` files
- ✅ Create GitHub releases with tags

### Commit Convention

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```text
feat: add new feature (triggers minor version bump)
fix: bug fix (triggers patch version bump)
feat!: breaking change (triggers major version bump)
docs: documentation updates
chore: maintenance tasks
```

**Best Practice for Clean Changelogs:**

- **On `main` branch**: Use scoped commits for visibility across the monorepo

  ```bash
  git commit -m "feat(foo): add authentication middleware"
  git commit -m "fix(bar): resolve memory leak"
  ```

- **On release branches** (`release/foo/v1.x`, etc.): Omit the scope for cleaner changelogs

  ```bash
  # On release/foo/v1.x branch
  git commit -m "feat: add authentication middleware"
  git commit -m "fix: resolve memory leak"
  ```

Since each app has its own `CHANGELOG.md` in its directory, the scope prefix is redundant on release branches. This keeps your changelogs clean while maintaining visibility in the main branch history.

### Example Commits

**On `main` branch (use scopes):**

```bash
# Multi-app changes or cross-cutting concerns
git commit -m "feat(foo): add authentication middleware"
git commit -m "fix(bar): correct response headers"
git commit -m "chore(deps): update Go dependencies"
```

**On release branches (omit scopes for cleaner changelogs):**

```bash
# On release/foo/v1.x
git checkout release/foo/v1.x
git commit -m "feat: add new API endpoint"
git commit -m "fix: resolve memory leak in handler"

# On release/bar/v2.x
git checkout release/bar/v2.x
git commit -m "feat: add caching layer"
git commit -m "fix: correct response encoding"
```

## 🛠️ Development

### Running Applications

**Foo Application:**

```bash
cd apps/foo
go run main.go
# Visit http://localhost:8080
```

**Bar Application:**

```bash
cd apps/bar
go run main.go
# Visit http://localhost:8081
```

### Building Applications

```bash
# Build Foo
cd apps/foo && go build -o ../../bin/foo

# Build Bar
cd apps/bar && go build -o ../../bin/bar
```

## 📝 Release Process

### Automatic Releases (Recommended)

1. **Commit with Conventional Commits** to the appropriate branch:

   ```bash
   git checkout release/foo/v1.x
   git commit -m "feat: add new feature"
   git push origin release/foo/v1.x
   ```

2. **Release Please creates PR** automatically with:
   - Updated version numbers
   - Generated changelog
   - Version bumps based on commit types

3. **Merge the PR** to trigger:
   - GitHub release creation
   - Git tag creation (e.g., `foo-v1.2.0`)
   - Changelog update

### Manual Version Management

The version manifest is tracked in `.release-please-manifest.json`:

```json
{
  "apps/foo": "1.0.0",
  "apps/bar": "1.0.0"
}
```

## 📋 Changelog

Each application maintains its own `CHANGELOG.md` with:

- Version history
- Features, fixes, and breaking changes
- Commit references
- Release dates

## 🔧 Configuration Files

- **`.github/release-please-config.json`**: Release Please configuration
- **`.release-please-manifest.json`**: Current version tracking
- **`.github/workflows/release-please.yml`**: GitHub Actions workflow

## 📚 Resources

- [Release Please Documentation](https://github.com/googleapis/release-please)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
