# Commit Guideline

## Document Information

### Scope

This document covers suggested scope and planning of Git repository commits, specifically to enhance repository conciseness, documentation, and understandability.

### History

| Date       | Author     | Description |
| ---------- | ---------- | ----------- |
| 2026-03-31 | Zach Smith | Release     |

## Guidelines

### General Guidelines

- Every commit is a testable chunk via either machine interface (e.g., API) or through the intended user interface. No commit should ever be a broken or untestable state.
- Commits should be written with conciseness in mind, as if every line of code is going to be manually reviewed.
- The following sections will use a animal data recording program with a presentation, interface, and business logic layer. This program also connects to a SQL database.

### Feature Plan

For the sample program, a feature will be added to add Genus classifications to animal species.

#### First Commit - Add Proof-of-Concept

- The first commit should be the absolute bare minimum for an end-to-end functional implementation of the given feature.
- For anything involving RESTful API or CRUD operations, only a get/read operation should be implemented.
- For projects with a frontend, the UI should add the absolute bare minimum UI functionality to fetch and display data for the new feature. It is acceptable for the UI implementation to follow the style of the project's theme.
- Depending on the project and feature, this initial fetch functionality may be fetching all records or just a a target record.
- Any database files and sample/test files should also be modified as appropriate.
- The code added in the first commit must be concise and as isolated from other components in the project as possible. If the feature is intended to work with other components but could theoretically be tested without wiring it, do not wire it until further commits.
- There should be no tweaks to presentation layer unrelated to implementation of this commit.

##### Example

For the first commit, the proof of concept will be the ability to read the the details of a Genus record.

1. Add Genus object to schema (e.g., create the Genus table for SQL. Note that at this point, the Genus ID column would not be added to the animal table yet).
1. Modify test files and inject Genus test data into database.
1. Create business logic and interface layers for fetching a target Genus.
1. Create a presentation layer named "Genus Details" with components to view fetched Genus record.

#### Second Commit - Add Core Functionality

- The second commit should focus on creating remaining RESTful API and/or CRUD operations from the first commit's changes.
- Update any frontend should implement UI elements for newly implemented operations.
- The second commit should NOT add enhancements or quality-of-life improvements for the feature, it is strictly to complete RESTful/CRUD operations.
- There should be no tweaks to presentation layer unrelated to implementation of this commit.

#### Example

For this commit, the remaining RESTful/CRUD operations will be created, which in this example will be the ability to create, edit, and delete.

1. In the business logic and interface layers, add functionality to create, edit, and delete Genus.
1. In the program's home page presentation layer, add components to create a Genus (since it would be impractical to add this functionality to the Genus Details page).
1. On the Genus Details presentation layer, add components to edit and delete the target Genus.

#### Third Commit - Wire In Proof-of-Concept

- If the core functionality is intended to work with other components and was testable without wiring it in, wire in (at a maximum, if applicable) read-only functionality to the components.

##### Example

In this commit, viewing the assigned Genus for target animals will be added.

1. In the schema, add the Genus ID to the Animals table.
1. Modify test files and inject Genus and Animal test data into database.
1. Modify animal's business logic and interface layers to read the Genus property of the target animal.
1. Modify the Animal Details presentation layer with components to display the Genus property.

- There should be no tweaks to presentation layer unrelated to implementation of this commit.

#### Fourth Commit - Wire In Core Functionality

- If any read-only functionality was wired in the last commit, wire in any other operations as applicable.
- There should be no tweaks to presentation layer unrelated to implementation of this commit.

##### Example

For the last core commit, the remaining RESTful/CRUD operations will be implemented for managing a target animal's Genus.

1. In the Animal's business logic and interface layers, add functionality to create, edit, and delete Genus.
1. In the Animal Details presentation layer, add components to link to a Genus, change Genus, or remove Genus entirely.

#### Further Commits - Quality of Life

- After the previous commits, enhancements and quality-of-life improvements may be made as necessary for the given feature.
- Each enhancement will be its own dedicated commit.
- Further commits may include refactoring of layers, including UI, for improvement outside of the core implementation.

##### Example

For the sample program, a page will be added to view all Genus, followed by implementing a Genus tag into the All Animals list view.

1. Add business logic, interface, and presentation layers to create a View All Genus page. This will be its own dedicated commit.
1. Add/modify business logic, interface, presentation layers, and classes to add the Genus to the View All Animals page, showing the respective Genus for each animal in the list. This will be its own dedicated commit.

### Enhancements

An enhancement is a small change or addition to improve an existing feature. Any changes to a schema will most likely constitute a feature, not an enhancement.

An enhancement can be compiled into a single commit, granted it is of small enough scope.

### Bug Fixes

A bug fix is a small change to an existing feature to fix an issue for an existing feature. Changes to schema for a bug fix may continue to follow this categorization of a bug fix.

A bug fix can be compiled into a single commit, granted it is of small enough scope. A narrow-scoped bug fix may span across a large amount of files - this is fine.
