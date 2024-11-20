'# Git Branching Standard Operating Procedure (SOP)

## Introduction
This document outlines the standard operating procedures for using Git in our development workflow. It covers branch naming conventions, branch flow, commit guidelines, merging strategies, and best practices to ensure consistency, collaboration, and efficiency across the team.

---

## 1. Branch Naming Conventions
Consistent branch naming helps clarify the purpose of each branch and improves collaboration. Below are the standard naming conventions for different types of branches:

- **Feature Branches**: For new features or enhancements.
  - Naming convention: `feature/<issue-number>-<short-description>`
  - Example: `feature/123-add-login-page`
  
- **Bugfix Branches**: For fixing bugs.
  - Naming convention: `bugfix/<issue-number>-<short-description>`
  - Example: `bugfix/456-fix-header-overlap`
  
- **Hotfix Branches**: For urgent fixes in production.
  - Naming convention: `hotfix/<issue-number>-<short-description>`
  - Example: `hotfix/789-fix-critical-error`
  
- **Release Branches**: For preparing a release.
  - Naming convention: `release/<version-number>`
  - Example: `release/v1.0.0`

- **Development Branch**: For ongoing development and integration.
  - Naming convention: `develop`
  
- **Staging Branch**: For pre-production testing.
  - Naming convention: `staging`

- **Main Branch** (or master): The stable production-ready code.
  - Naming convention: `main` (or `master`, depending on preference)

---

## 2. Branch Flow
The flow between branches is designed to ensure smooth progression from development to production. Below is the recommended branch flow:

### Development Branch (`develop`)
- All feature development starts here.
- Developers create feature or bugfix branches off of the `develop` branch.
- Once a feature or bugfix is complete, it is merged back into the `develop` branch via a pull request (PR).

### Staging Branch (`staging`)
- After merging all features and bug fixes into `develop`, create a pull request to merge into the `staging` branch for testing.
- The staging environment mirrors production and is used for final testing before release.

### Main Branch (`main`)
- Once all tests pass in the staging environment, create a release branch from `staging` or merge directly into the `main` branch.
- The main branch should always contain production-ready code.

### Release Branches (`release/x.x.x`)
- If you need to prepare for a formal release, create a release branch from `develop`.
- Perform any final bug fixes or adjustments here before merging into both `main` and `develop`.

### Hotfix Branches (`hotfix/x.x.x`)
- In case of critical issues in production, create a hotfix branch from the `main` branch.
- After fixing the issue, merge the hotfix back into both `main` and `develop` to ensure the fix is included in future releases.

---

## 3. Commit Guidelines
To maintain a clean and understandable history, follow these commit guidelines:

### Commit Message Structure
Use clear, descriptive commit messages following the [Conventional Commits](https://www.conventionalcommits.org/) standard:
```
<type>: <short description>

[Optional body]
```
Example:
```
feat: add login functionality
fix: resolve header overlap issue
```

### Atomic Commits
Each commit should represent one logical change. Commit frequently with small, atomic changes to make it easier to track progress and roll back if necessary.

---

## 4. Merging Strategy

### Pull Requests (PRs)
All changes between branches should be merged via pull requests (PRs). This ensures that every change is reviewed before being integrated.

### Merge Strategies
- Use **squash merges** or **rebase merges** when merging feature/bugfix branches into develop to keep commit history clean.
- Use regular merges when merging release or hotfix branches into main.

### Conflict Resolution
Resolve conflicts locally before pushing changes or opening a pull request to avoid blocking other team members.

---

## 5. Testing and CI/CD Integration

### Automated Testing
Integrate Continuous Integration (CI) pipelines that automatically run tests on every PR. Ensure that no code is merged into staging or main without passing all automated tests.

### Continuous Deployment (CD)
Set up Continuous Deployment (CD) pipelines to automatically deploy code from the main branch to production after successful tests.

---

## 6. Best Practices

1. **Pull Before Push**:
   Always pull the latest changes from the remote repository before pushing your own changes to avoid conflicts.

2. **Keep Main Stable**:
   The main branch should always be in a deployable state with fully tested code.

3. **Use Feature Flags**:
   If you're working on long-running features, consider using feature flags so you can merge incomplete features without affecting production.

4. **Avoid Long-Lived Branches**:
   Keep feature and bugfix branches short-lived by merging them back into develop as soon as possible to avoid large merge conflicts.

5. **Regularly Sync with Develop/Staging/Main**:
   Keep your feature or bugfix branch up-to-date by regularly merging changes from develop or staging to avoid large conflicts later on.

6. **Document Your Workflow**:
   Ensure that your team has clear documentation on how branching works, including naming conventions, PR review processes, and CI/CD integration steps.
