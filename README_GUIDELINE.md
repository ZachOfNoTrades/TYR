# README Guideline

## Document Information

### Scope

This document provides recommendations for the structure and section conventions for a repository's top-level `README.md`.

Documentation following this guideline should also inherit the [Language Guideline](./LANGUAGE_GUIDELINE.md).

### History

| Date       | Author     | Description                                                |
| ---------- | ---------- | ---------------------------------------------------------- |
| 2026-07-10 | Zach Smith | Release                                                    |
| 2026-07-13 | Zach Smith | Tweak prerequisite, change config props from list to table |
| 2026-07-23 | Zach Smith | Add attributes rules, rename key table section             |

# Guideline

A README can either serve as the full end-to-end document for a small repository, or a brief scope and router to more detailed documentation. For larger repos, further documentation should live under `docs/`, and the README will route to that.

## Headings

- The level 1 heading (`#`) is reserved for the README title only.
- Level 2 headings (`##`) are only used for the main sections as defined in [Structure](#structure).
- All level 3 headings (`###`) or lower may be freely used under the main sections.
- All headings are Title Case.

## Structure

READMEs should only use the the following main sections, and in the following order. Sections that do not apply or have dedicated documentation in `docs/` should be omitted.

1. Title and Definition
1. Features
1. Table of Contents
1. Installation
1. Configuration
1. Usage
1. Updating
1. Notes
1. Contact

## Title and Definition

- The title should just be the project's full name. For acronym'd projects, the acronym is acceptable.
- Provide a definition sentence or paragraph under the title: `<Name> is a <what it is> <for whom / purpose>.`

## Features

- List features as bold lead-in bullets: `- **Central Server Integration**: The Datapack network uses a centralized server...`.
- The lead-in is the feature name in Title Case; the description is one sentence after the colon.

## Table of Contents

Use this section when the repository has a `docs/` directory.

- Link to each guide with a relative path: `[Installation Guide](./docs/installation/INSTALLATION.md)`.
- For projects with documentation for different audiences, group links under level-3 subheadings — `Developers`, `SysAdmins`, `Users` — or name the audience in the link text: `[Release Guide for Developers](...)`.
- Order groups from most-general to most-specialized reader.

### Organizing by Audience

For projects with documentation for different audiences, the TOC can be organized by either grouping links under a level-3 subheading or by including the target audience in the link name.

**Audience Groups**

```md
### Developers

- [Installation Guide](docs/install/INSTALL.md)
- [Updating Guide](docs/update/UPDATE.md)

### Users

- [User Guide](docs/user_guide/USER_GUIDE.md)
```

**Audience in Link Name**

```md
- [Installation Guide for Developers](docs/install/INSTALL.md)
- [Updating Guide for Developers](docs/update/UPDATE.md)
- [Guide for Users](docs/user_guide/USER_GUIDE.md)
```

## Installation

This section provides precise instructions on how to get the software package working. This section should be omitted if there is a dedicated installation document.

### Structure

1. Instruction Overview
2. Prerequisite Subsection
3. Instructions

#### Instruction Overview

A brief sentence or paragraph explaining the scope of the installation procedure.

#### Prerequisite Subsection

A level-3 subheading containing all the required hardware, accounts, permissions, etc. that the instructions _cannot_ provision. For example, if an Azure administrator account is a requirement, it is considered a prerequisite.

- Bulleted list
- Terse terms

#### Instructions

Robust instructions to set up requirements and the project. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions).

Long procedures may be phased into multiple level-3 subsections.

## Configuration

This section provides instructions on how to set configurable values for the software. This section should be omitted if there is a dedicated configuration document, or if there is no configuration.

### Structure

The Config section should contain these elements. These do not need to be separated into subsections.

1. Sample Config Code Fence
1. Key Table
1. Configuration Instructions

### Sample Config Code Fence

Provide a sample configuration. If the project uses a binary config file such as `config.json`, it is recommended to structure the code fence like the file structure.

```json
"sample_config": {
  "prop_1": true,
  "prop_2": "hello world"
}
```

### Key Table

A table should be provided with the following columns:

- **Key**: the key name
- **Type**: The data type of the key
- **Default**: Either the default value, or any applicable properties
- **Description**: Description/use of the key

#### Attributes and footnotes

- Key values may have certain attributes such as "required".
- Attributes are denoted via a symbol in the Default column and a footnote under the table.
- Footnotes shall be formatted as the symbol followed by the property note italicized (e.g. `\* _required_`).
- If "required" is an attribute, it shall be the first attribute listed and the asterisk shall be reserved for it.
- Symbols should use the standard footnote symbol sequence of:
  - `*` Asterisk (Reserved for "required" if applicable)
  - `†` Dagger
  - `‡` Double dagger
  - `§` Section sign
  - `‖` Parallels
  - `¶` Pilcrow
  - Further attributes may use a double symbol (i.e. double asterisk `**`)
  - Multiple symbols may be used in the same table cell.
- The Default column may be omitted if there are no attributes or default values.

#### Sample table

```md
| Key           | Type    | Default      | Description                                      |
| ------------- | ------- | ------------ | ------------------------------------------------ |
| prop1         | boolean | \*           | Defines whether the config is turned on.         |
| prop2         | string  | "helloworld" | Text to show the user.                           |
| random_number | int     | †            | A randomly generated number when the app starts. |

\*_required_  
† _autogenerates_
```

### Configuration Instructions

Brief instructions how to set up the configuration. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions).

## Usage

Brief instructions on how to use the software. This section should be omitted if there is a dedicated usage document. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions).

## Updating

This section will provide instructions how to update from different versions of the project.

- Each version migration will have a dedicated subsection (`## v4.x > v5.x`)
- If an update re-uses steps found in the instructions, reference that section rather than repeating (`Follow steps in [Host Setup](#host-setup)`).
- This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions).

## Notes

Use this section for any miscellaneous details that did not fall into any other section.

## Contact

This section is optional. A credit tag can be added, crediting the company and author.

`Developed for [ALERT TECH SMT](https://alerttechsmt.com/) by [Zach Smith](https://github.com/ZachOfNoTrades).`
