# README Guideline

## `1` Document Information

### `1.A` Scope

#### `1.A.1`
This document provides recommendations for the structure and section conventions for a repository's top-level `README.md`.

#### `1.A.2`
Documentation following this guideline should also inherit the [Language Guideline](./LANGUAGE_GUIDELINE.md).

## `2` Guidelines

### `2.A` General

#### `2.A.1`
A README can either serve as the full end-to-end document for a small repository, or a brief scope and router to more detailed documentation. For larger repos, further documentation should live under `docs/`, and the README will route to that.

### `2.B` Headings

#### `2.B.1`
The level 1 heading (`#`) is reserved for the README title only.

#### `2.B.2`
Level 2 headings (`##`) are only used for the main sections as defined in [Structure](#2c-structure).

#### `2.B.3`
All level 3 headings (`###`) or lower may be freely used under the main sections.

#### `2.B.4`
All headings are Title Case.

### `2.C` Structure

#### `2.C.1`
READMEs should only use the following main sections, and in the following order. Sections that do not apply or have dedicated documentation in `docs/` should be omitted.

- `2.C.1(a)`
Title and Definition

- `2.C.1(b)`
Features

- `2.C.1(c)`
Table of Contents

- `2.C.1(d)`
Installation

- `2.C.1(e)`
Configuration

- `2.C.1(f)`
Usage

- `2.C.1(g)`
Updating

- `2.C.1(h)`
Notes

- `2.C.1(i)`
Contact

### `2.D` Title and Definition

#### `2.D.1`
The title should just be the project's full name. For acronym'd projects, the acronym is acceptable.

#### `2.D.2`
Provide a definition sentence or paragraph under the title: `<Name> is a <what it is> <for whom / purpose>.`

### `2.E` Features

#### `2.E.1`
List features as bold lead-in bullets: `- **Offline Mode**: The app keeps working without a network connection...`.

#### `2.E.2`
The lead-in is the feature name in Title Case; the description is one sentence after the colon.

### `2.F` Table of Contents

#### `2.F.1`
Use this section when the repository has a `docs/` directory.

#### `2.F.2`
Link to each guide with a relative path: `[Installation Guide](./docs/installation/INSTALLATION.md)`.

#### `2.F.3`
For projects with documentation for different audiences, group links under level-3 subheadings — `Developers`, `SysAdmins`, `Users` — or name the audience in the link text: `[Release Guide for Developers](...)`.

#### `2.F.4`
Order groups from most-general to most-specialized reader.

#### `2.F.5`
For projects with documentation for different audiences, the TOC can be organized by either grouping links under a level-3 subheading or by including the target audience in the link name.

##### `2.F.5(a)`
Audience Groups:

```md
### Developers

- [Installation Guide](docs/install/INSTALL.md)
- [Updating Guide](docs/update/UPDATE.md)

### Users

- [User Guide](docs/user_guide/USER_GUIDE.md)
```

##### `2.F.5(b)`
Audience in Link Name:

```md
- [Installation Guide for Developers](docs/install/INSTALL.md)
- [Updating Guide for Developers](docs/update/UPDATE.md)
- [Guide for Users](docs/user_guide/USER_GUIDE.md)
```

### `2.G` Installation

#### `2.G.1`
This section provides precise instructions on how to get the software package working. This section should be omitted if there is a dedicated installation document.

#### `2.G.2`
Structure:

- `2.G.2(a)`
Instruction Overview

- `2.G.2(b)`
Prerequisite Subsection

- `2.G.2(c)`
Instructions

#### `2.G.3`
Instruction Overview: A brief sentence or paragraph explaining the scope of the installation procedure.

#### `2.G.4`
Prerequisite Subsection: A level-3 subheading containing all the required hardware, accounts, permissions, etc. that the instructions _cannot_ provision. For example, if an Azure administrator account is a requirement, it is considered a prerequisite.

- `2.G.4(a)`
Bulleted list

- `2.G.4(b)`
Terse terms

#### `2.G.5`
Instructions: Robust instructions to set up requirements and the project. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#2h-instructions).

#### `2.G.6`
Long procedures may be phased into multiple level-3 subsections.

### `2.H` Configuration

#### `2.H.1`
This section provides instructions on how to set configurable values for the software. This section should be omitted if there is a dedicated configuration document, or if there is no configuration.

#### `2.H.2`
The Config section should contain these elements. These do not need to be separated into subsections.

- `2.H.2(a)`
Sample Config Code Fence

- `2.H.2(b)`
Key Table

- `2.H.2(c)`
Configuration Instructions

#### `2.H.3`
Sample Config Code Fence: Provide a sample configuration. If the project uses a config file such as `config.json`, it is recommended to structure the code fence like the file structure.

##### `2.H.3(a)`
Example:

```json
"sample_config": {
  "prop_1": true,
  "prop_2": "hello world"
}
```

#### `2.H.4`
Key Table: A table should be provided with the following columns:

- `2.H.4(a)`
**Key**: The key name

- `2.H.4(b)`
**Type**: The data type of the key

- `2.H.4(c)`
**Default**: Either the default value, or any applicable properties

- `2.H.4(d)`
**Description**: Description/use of the key

#### `2.H.5`
Key values may have certain attributes such as "required".

#### `2.H.6`
Attributes are denoted via a symbol in the Default column and a footnote under the table.

#### `2.H.7`
Footnotes shall be formatted as the symbol followed by the property note italicized (e.g. `\* _required_`).

#### `2.H.8`
If "required" is an attribute, it shall be the first attribute listed and the asterisk shall be reserved for it.

#### `2.H.9`
Symbols should use the standard footnote symbol sequence of:

- `2.H.9(a)`
`*` Asterisk (Reserved for "required" if applicable)

- `2.H.9(b)`
`†` Dagger

- `2.H.9(c)`
`‡` Double dagger

- `2.H.9(d)`
`§` Section sign

- `2.H.9(e)`
`‖` Parallels

- `2.H.9(f)`
`¶` Pilcrow

- `2.H.9(g)`
Further attributes may use a double symbol (i.e. double asterisk `**`)

- `2.H.9(h)`
Multiple symbols may be used in the same table cell.

#### `2.H.10`
The Default column may be omitted if there are no attributes or default values.

#### `2.H.11`
Sample table:

```md
| Key           | Type    | Default      | Description                                      |
| ------------- | ------- | ------------ | ------------------------------------------------ |
| prop1         | boolean | \*           | Defines whether the config is turned on.         |
| prop2         | string  | "helloworld" | Text to show the user.                           |
| random_number | int     | †            | A randomly generated number when the app starts. |

\*_required_  
† _autogenerates_
```

#### `2.H.12`
Configuration Instructions: Brief instructions on how to set up the configuration. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#2h-instructions).

### `2.I` Usage

#### `2.I.1`
Brief instructions on how to use the software. This section should be omitted if there is a dedicated usage document. This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#2h-instructions).

### `2.J` Updating

#### `2.J.1`
This section will provide instructions on how to update from different versions of the project.

#### `2.J.2`
Each version migration will have a dedicated subsection (`## v4.x > v5.x`)

#### `2.J.3`
If an update re-uses steps found in the instructions, reference that section rather than repeating (`Follow steps in [Host Setup](#host-setup)`).

#### `2.J.4`
This section should follow the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#2h-instructions).

### `2.K` Notes

#### `2.K.1`
Use this section for any miscellaneous details that did not fall into any other section.

### `2.L` Contact

#### `2.L.1`
This section is optional. A credit tag can be added, crediting the company and author.

##### `2.L.1(a)`
Example:

`Developed for [ZSMITH.IO](https://zsmith.io/) by [Zach Smith](https://github.com/ZachOfNoTrades).`
