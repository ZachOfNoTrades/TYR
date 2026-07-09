# File Convention Standard

## Document Information

### Scope

This document covers the recommendations for file and directory organization for software development.

### History

| Date       | Author     | Description |
| ---------- | ---------- | ----------- |
| 2026-07-09 | Zach Smith | Draft       |

## Repositories

Repositories are a collection of core files required for a project.

1. All repository files should be in a `{project}/repo/` folder. This cleanly allows for other supporting files to be included in a project folder without unnecessary ignore rules.
1. All supporting files, such as supplementary documentation or company-specific testing files, should be organized within the `{project}/` folder alongside `repo/`.

## Naming Conventions

1. All files and directories related to a software development project should exclude uppercase letters and spaces.
1. Spaces can be supplemented with either a underscore (`_`) or dash (`-`).

Good: `project1/repo/python_files/test_script-v1.py`
Bad: `Project 1/repo/Python files/Test Script v1.py`

### Usage of Underscore versus Dash

1. Underscores should primarily be used to separate words/terms within a collective group.
   - `test_script`
1. Dashes should primarily be used to separate groups.
   - `test_script-v1`
