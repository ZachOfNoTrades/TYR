# Language Guideline

## `1` Document Information

### `1.A` Scope

#### `1.A.1`
This document covers the voice, grammar, and naming conventions used across codebases and technical documentation.

## `2` Guidelines

### `2.A` General Language

#### `2.A.1`
Mood:

- `2.A.1(a)`
Impersonal language

- `2.A.1(b)`
Direct imperative

- `2.A.1(c)`
Terse

- `2.A.1(d)`
Present-tense, as applicable

#### `2.A.2`
Wording:

- `2.A.2(a)`
Plain words

- `2.A.2(b)`
Spell out acronyms the first time they are referenced, then the acronym may be used as desired

#### `2.A.3`
Structure:

- `2.A.3(a)`
Use `i.e.` and `e.g.` inline for examples

#### `2.A.4`
All writing should be stateless, including code comments, READMEs, and technical documentation.

- `2.A.4(a)`
Never reference prior state, history, or transitions. Avoid words like "old", "new", "migrated from/to", etc.

- `2.A.4(b)`
Historical context should be exclusively reserved for commit messages and changelogs, not in any artifact itself.

- `2.A.4(c)`
Never make an assuming statement, such as "This app is deployed on an Azure VM".

- `2.A.4(d)`
An example would be saying "This is a fast C++ program" instead of "This program has been converted from Python to C++ to be faster".

### `2.B` Names

#### `2.B.1`
All names should be verbose except with repo-specific established terms e.g. `serial_number_per_customer` not `sn_cus`

#### `2.B.2`
Each segment of names should be ordered from largest-scope to smallest-scope e.g. `name_first` not `first_name`

#### `2.B.3`
Booleans should read as a yes/no question e.g. `is_primary`, `enabled`

#### `2.B.4`
Any industry-standard abbreviation that is not made clearer by its verbose form may be used, provided all source files utilizing the abbreviation contain a glossary listing the verbose name, abbreviation, and definition — e.g. `rts` → _Request to Send_ in radio communication.

#### `2.B.5`
All object-oriented programming (OOP) languages should adhere to the following casing conventions:

| Locals Fields | Constants         | Functions    |
| ------------- | ----------------- | ------------ |
| `snake_case`  | `SCREAMING_SNAKE` | `PascalCase` |

#### `2.B.6`
Listed non-OOP languages should adhere to the following casing conventions:

| Language | Type        | Casing       |
| -------- | ----------- | ------------ |
| SQL      | DB columns  | `snake_case` |
| SQL      | Table names | `snake_case` |

### `2.C` Comments

#### `2.C.1`
The language of comments should continue to follow the [General Language](#2a-general-language) rules, with a focus on plain, terse, and stateless wording.

#### `2.C.2`
Comments do not need to be added to modified code unless the code necessitates a comment - reasons for modification should be captured in commit descriptions rather than code.

#### `2.C.3`
Comments have the following categories:

- `2.C.3(a)`
Inline comments: Comments for a single line of code

- `2.C.3(b)`
Block comments: Comments for a code block that performs a single action

- `2.C.3(c)`
Section comments: Comments for a large code block with multiple actions

- `2.C.3(d)`
Function comments: Comments describing the purpose of a function/method

#### `2.C.4`
The prefixes `Inline comment:`, `Block comment:`, `Section comment:`, and `Function comment:` shown in the examples below are for illustration only and are not part of the required format.

### `2.D` Inline Comments

#### `2.D.1`
Inline comments should be inlined with the code, not above.

##### `2.D.1(a)`
Example:

```js
function FunctionOne() {
  const x = "abc"; // Inline comment: sets variable x to string
}
```

### `2.E` Block Comments

#### `2.E.1`
Block comments should be on the line above the code.

##### `2.E.1(a)`
Example:

```js
function FunctionOne(x) {
  // Block comment: checks if variable x equals the expected value
  if (x === "abc") {
    return true; // Inline comment: x is the expected value
  } else {
    return false;
  }
}
```

### `2.F` Section Comments

#### `2.F.1`
Section comments may be used to provide details for large blocks of code. The rule of thumb for section comments vs. block comments is scope: a block comment describes one contiguous chunk of code, while a section comment describes code that spans multiple chunks separated by blank lines.

#### `2.F.2`
A section comment should have a blank line between it and the code it describes.

##### `2.F.2(a)`
Example:

```js
function FunctionOne(x) {
  // Section comment: attempts to change the value of x if it does not match the expected value

  try {
    if (x === "abc") {
      return;
    }

    // Block comment: change the value of x
    else {
      x = "abc";
    }
  } catch {
    // Block comment: error handling if the try block failed
    throw new Error("value could not be changed"); // Inline comment: error goes to the terminal
  }
}
```

### `2.G` Function Comments

#### `2.G.1`
Function comments provide the purpose of a function/method. These comments should be above the function, identical to a block comment.

##### `2.G.1(a)`
Example:

```js
// Function comment: fetches user data from the API and returns a formatted record
function GetUser(userId) {
  const data = api.fetch(userId);
  return formatRecord(data);
}
```

### `2.H` Instructions

#### `2.H.1`
This section applies to instructions such as installation and usage procedures found in READMEs or other project documentation.

#### `2.H.2`
A brief may be provided under the instructions section or subsections only if it is imperative information to assist with the procedure.

#### `2.H.3`
All steps shall be a numbered list (`1.`). It is recommended to mark every step as `1.` rather than incrementing, as markdown viewers automatically render them in order `1. 1. 1. > 1. 2. 3.`, and it alleviates the potential for typos.

#### `2.H.4`
Each step is a terse imperative.

#### `2.H.5`
Sub-steps are allowed as either a numbered (`1.1.`) or unordered (`-`) list.

#### `2.H.6`
For any step that has a sub-step picture, codeblock, or is otherwise applicable, it should end in a colon.

### `2.I` Codeblocks

#### `2.I.1`
User-supplied values are displayed as `{placeholder}` in braces.

#### `2.I.2`
Any change directory command (`cd`) only lives in the scope of the section/subsection it appears in. A `cd` command must be given at the start of each section, even if the directory is unchanged from the previous section.

#### `2.I.3`
When a command carries placeholders, add a concrete `# Example` beneath it in the same fence:

```
scp -r {local_app_file_path} {username}@{host_ip}:{deployment_directory}

# Example
scp -r ~/downloads/zest-v1.0.zip admin@10.0.0.2:/home/admin/zest/v1.0
```

#### `2.I.4`
Annotate a command with a trailing comment when its purpose is not obvious: `sudo apt install python3-tk  # Tkinter for TCL communication`.
