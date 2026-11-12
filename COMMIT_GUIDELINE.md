# Commit Guideline

## `1` Document Information

### `1.A` Scope

#### `1.A.1`
This document covers suggested scope and planning of Git repository commits, specifically to enhance repository conciseness, documentation, and understandability.

## `2` Guidelines

### `2.A` General

#### `2.A.1`
Every commit is a testable chunk via either machine interface (e.g., API) or through the intended user interface. No commit should ever be a broken or untestable state.

#### `2.A.2`
Commits should be written with conciseness in mind, as if every line of code is going to be manually reviewed.

#### `2.A.3`
The following sections will use a animal data recording program with a presentation, interface, and business logic layer. This program also connects to a SQL database.

### `2.B` Feature Plan

#### `2.B.1`
For the sample program, a feature will be added to add Genus classifications to animal species.

### `2.C` Commit Zero - Documentation

#### `2.C.1`
Prior to the first commit of code changes, it is recommended to create documentation drafts so the commits can work towards a specific end goal.

### `2.D` First Commit - Add Proof-of-Concept

#### `2.D.1`
The first commit should be the absolute bare minimum for an end-to-end functional implementation of the given feature.

#### `2.D.2`
For anything involving RESTful API or CRUD operations, only a get/read operation should be implemented.

#### `2.D.3`
For projects with a frontend, the UI should add the absolute bare minimum UI functionality to fetch and display data for the new feature. It is acceptable for the UI implementation to follow the style of the project's theme.

#### `2.D.4`
Depending on the project and feature, this initial fetch functionality may be fetching all records or just a a target record.

#### `2.D.5`
Any database files and sample/test files should also be modified as appropriate.

#### `2.D.6`
The code added in the first commit must be concise and as isolated from other components in the project as possible. If the feature is intended to work with other components but could theoretically be tested without wiring it, do not wire it until further commits.

#### `2.D.7`
There should be no tweaks to presentation layer unrelated to implementation of this commit.

#### `2.D.8`
Example: For the first commit, the proof of concept will be the ability to read the the details of a Genus record.

1. Add Genus object to schema (e.g., create the Genus table for SQL. Note that at this point, the Genus ID column would not be added to the animal table yet).
1. Modify test files and inject Genus test data into database.
1. Create business logic and interface layers for fetching a target Genus.
1. Create a presentation layer named "Genus Details" with components to view fetched Genus record.

### `2.E` Second Commit - Add Core Functionality

#### `2.E.1`
The second commit should focus on creating remaining RESTful API and/or CRUD operations from the first commit's changes.

#### `2.E.2`
Update any frontend should implement UI elements for newly implemented operations.

#### `2.E.3`
The second commit should NOT add enhancements or quality-of-life improvements for the feature, it is strictly to complete RESTful/CRUD operations.

#### `2.E.4`
There should be no tweaks to presentation layer unrelated to implementation of this commit.

#### `2.E.5`
Example: For this commit, the remaining RESTful/CRUD operations will be created, which in this example will be the ability to create, edit, and delete.

1. In the business logic and interface layers, add functionality to create, edit, and delete Genus.
1. In the program's home page presentation layer, add components to create a Genus (since it would be impractical to add this functionality to the Genus Details page).
1. On the Genus Details presentation layer, add components to edit and delete the target Genus.

### `2.F` Third Commit - Wire In Proof-of-Concept

#### `2.F.1`
If the core functionality is intended to work with other components and was testable without wiring it in, wire in (at a maximum, if applicable) read-only functionality to the components.

#### `2.F.2`
Example: In this commit, viewing the assigned Genus for target animals will be added.

1. In the schema, add the Genus ID to the Animals table.
1. Modify test files and inject Genus and Animal test data into database.
1. Modify animal's business logic and interface layers to read the Genus property of the target animal.
1. Modify the Animal Details presentation layer with components to display the Genus property.

#### `2.F.3`
There should be no tweaks to presentation layer unrelated to implementation of this commit.

### `2.G` Fourth Commit - Wire In Core Functionality

#### `2.G.1`
If any read-only functionality was wired in the last commit, wire in any other operations as applicable.

#### `2.G.2`
There should be no tweaks to presentation layer unrelated to implementation of this commit.

#### `2.G.3`
Example: For the last core commit, the remaining RESTful/CRUD operations will be implemented for managing a target animal's Genus.

1. In the Animal's business logic and interface layers, add functionality to create, edit, and delete Genus.
1. In the Animal Details presentation layer, add components to link to a Genus, change Genus, or remove Genus entirely.

### `2.H` Further Commits - Quality of Life

#### `2.H.1`
After the previous commits, enhancements and quality-of-life improvements may be made as necessary for the given feature.

#### `2.H.2`
Each enhancement will be its own dedicated commit.

#### `2.H.3`
Further commits may include refactoring of layers, including UI, for improvement outside of the core implementation.

#### `2.H.4`
Example: For the sample program, a page will be added to view all Genus, followed by implementing a Genus tag into the All Animals list view.

1. Add business logic, interface, and presentation layers to create a View All Genus page. This will be its own dedicated commit.
1. Add/modify business logic, interface, presentation layers, and classes to add the Genus to the View All Animals page, showing the respective Genus for each animal in the list. This will be its own dedicated commit.

### `2.I` Enhancements

#### `2.I.1`
An enhancement is a small change or addition to improve an existing feature. Any changes to a schema will most likely constitute a feature, not an enhancement.

#### `2.I.2`
An enhancement can be compiled into a single commit, granted it is of small enough scope.

#### `2.I.3`
If the commit is refactoring or migrating existing code, the commit should be as small of a diff as possible to avoid a full, unreadable migration.

### `2.J` Bug Fixes

#### `2.J.1`
A bug fix is a small change to an existing feature to fix an issue for an existing feature. Changes to schema for a bug fix may continue to follow this categorization of a bug fix.

#### `2.J.2`
A bug fix can be compiled into a single commit, granted it is of small enough scope. A narrow-scoped bug fix may span across a large amount of files - this is fine.
