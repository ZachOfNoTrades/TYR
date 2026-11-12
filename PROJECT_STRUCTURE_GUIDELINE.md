# Project Structure Guideline

## `1` Document Information

### `1.A` Scope

#### `1.A.1`
This document covers the recommendations for file and directory organization for software development.

## `2` Guidelines

### `2.A` Repositories

#### `2.A.1`
Repositories are a collection of core files required for a project.

#### `2.A.2`
All repository files should be in a `{project}/repo/` folder. This cleanly allows for other supporting files to be included in a project folder without unnecessary ignore rules.

#### `2.A.3`
All supporting files, such as supplementary documentation or company-specific testing files, should be organized within the `{project}/` folder alongside `repo/`.

### `2.B` Documentation Structure

#### `2.B.1`
Below is the recommended base documentation and structure for a repository. Any directories or files not applicable may be omitted.

##### `2.B.1(a)`
Structure:

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

#### `2.B.2`
**docs/deployment/**: At a minimum, a manual deployment guide must exist. Additional deployment guides (e.g. CICD, Vercel) should exist as the project requires.

### `2.C` Naming Conventions

#### `2.C.1`
All files and directories related to a software development project should exclude uppercase letters. Documentation files are exempt from this.

#### `2.C.2`
Documentation files should be all uppercase letters.

#### `2.C.3`
All files and directories (including documentation files) should exclude spaces. Spaces can be supplemented with either an underscore (`_`) or dash (`-`).

##### `2.C.3(a)`
Example:

- Documentation
  - Good: `project1/repo/docs/testing/TEST_PROCEDURE.md`
  - Bad: `project1/repo/docs/testing/Test Procedure.md`
- Non-Documentation
  - Good: `project1/repo/python_files/test_script-v1.py`
  - Bad: `Project 1/repo/Python files/Test Script v1.py`

### `2.D` Usage of Underscore versus Dash

#### `2.D.1`
Underscores should primarily be used to separate words/terms within a collective group.

- `2.D.1(a)`
`test_script`

#### `2.D.2`
Dashes should primarily be used to separate groups.

- `2.D.2(a)`
`test_script-v1`
