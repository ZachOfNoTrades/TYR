# Project Structure Standard

## Document Information

### Scope

This document covers the recommendations for file and directory organization for software development.

## Repositories

Repositories are a collection of core files required for a project.

1. All repository files should be in a `{project}/repo/` folder. This cleanly allows for other supporting files to be included in a project folder without unnecessary ignore rules.
1. All supporting files, such as supplementary documentation or company-specific testing files, should be organized within the `{project}/` folder alongside `repo/`.

## Documentation Structure

Below is the recommended base documentation and structure for a repository. Any directories or files not applicable may be omitted.

```
repo/
   devtools/   -- Directory of testing, development, and sample files
   docs/
      deployment/ -- Guides for setup, install, management, and updating the app
         CICD_DEPLOYMENT.md   -- Guide for CI/CD deployment
         MANUAL_DEPLOYMENT.md -- Guide for manual deployment
      development/
         DEVELOPMENT.md -- Guide for setting up a development environment
      resources/
         RESOURCES.md   -- Instructions for setting up required external resources
      testing/ -- Development and/or production testing procedures
   README.md
```

- **docs/deployment/**: At a minimum, a manual deployment guide must exist. Additional deployment guides (e.g. CICD, Vercel) should exist as project requires.

## Naming Conventions

1. All files and directories related to a software development project should exclude uppercase letters. Documentation files are exempt from this.
1. Documentation files should be all uppercase letters.
1. All files and directories (including documentation files) should exclude spaces. Spaces can be supplemented with either a underscore (`_`) or dash (`-`).

- Documentation
  - Good: `project1/repo/docs/testing/TEST_PROCEDURE.md`
  - Bad: `project1/repo/docs/testing/Test Procedure.md`
- Non-Documentation
  - Good: `project1/repo/python_files/test_script-v1.py`
  - Bad: `Project 1/repo/Python files/Test Script v1.py`

### Usage of Underscore versus Dash

1. Underscores should primarily be used to separate words/terms within a collective group.
   - `test_script`
1. Dashes should primarily be used to separate groups.
   - `test_script-v1`
