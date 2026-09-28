# Language Guideline

## Document Information

### Scope

This document covers the voice, grammar, and naming conventions used across codebases and technical documentation.

# Guideline

## General Language

**Mood**

- Impersonal language
- Direct imperative
- Terse
- Present-tense, as applicable

**Wording**

- Plain words
- Spell out acronyms the first time they are referenced, then the acronym may be used as desired

**Structure**

- Use `i.e.` and `e.g.` inline for examples

**State**

All writing should be stateless, including code comments, READMEs, and technical documentation.

- Never reference prior state, history, or transitions. Avoid works like "old", "new", "migrated from/to", etc.
- Historical context should be exclusively reserved for commit messages and changelogs, not in any artifact itself.
- Never make a assuming statement, such as "This app is deployed on an Azure VM".

An example would be saying "This is a fast C++ program" instead of "This program has been converted from Python to C++ to be faster".

## Code

### Names

- All names should be verbose except with repo-specific established terms e.g. `serial_number_per_customer` not `sn_cus`
- Each segment of names should be ordered from largest-scope to smallest-scope e.g. `name_first` not `first_name`
- Booleans should read as a yes/no question e.g. `is_primary`, `enabled`

Any industry-standard abbreviation that is not made clearer by its verbose form may be used, provided all source files utilizing the abbreviation contain a glossary listing the verbose name, abbreviation, and definition — e.g. `rts` → _Request to Send_ in radio communication.

All object-oriented programming (OOP) languages should adhere to the following casing conventions:

| Locals Fields | Constants         | Functions    |
| ------------- | ----------------- | ------------ |
| `snake_case`  | `SCREAMING_SNAKE` | `PascalCase` |

Listed non-OOP languages should adhere to the following casing conventions:

| Language | Type        | Casing       |
| -------- | ----------- | ------------ |
| SQL      | DB columns  | `snake_case` |
| SQL      | Table names | `snake_case` |

### Comments

#### Language

- The language of comments should continue to follow the [General Language](#general-language) rules, with a focus on plain, terse, and stateless wording.
- Comments do not need to be added to modified code unless the code necessitates a comment - reasons for modification should be captured in commit descriptions rather than code.

#### Categories

Comments have the following categories:

- Inline comments: Comments for a single line of code
- Block comments: Comments for a code block that performs a single action
- Section comments: Comments for a large code block with multiple actions
- Function comments: Comments describing the purpose of a function/method

The prefixes `Inline comment:`, `Block comment:`, `Section comment:`, and `Function comment:` shown in the examples below are for illustration only and are not part of the required format.

#### Inline Comments

Inline comments should be inlined with the code, not above.

```js
function FunctionOne() {
  const x = "abc"; // Inline comment: sets variable x to string
}
```

#### Block Comments

Block comments should be on the line above the code.

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

#### Section Comments

Section comments may be used to provide details for large blocks of code. The rule of thumb for section comments vs. block comments is scope: a block comment describes one contiguous chunk of code, while a section comment describes code that spans multiple chunks separated by blank lines.

A section comment should have a blank line between it and the code it describes.

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

#### Function Comments

Function comments provide the purpose of a function/method. These comments should be above the function, identical to a block comment.

```js
// Function comment: fetches user data from the API and returns a formatted record
function GetUser(userId) {
  const data = api.fetch(userId);
  return formatRecord(data);
}
```

## Documentation

### Instructions

This section applies to instructions such as installation and usage procedures found in READMEs or other project documentation.

- A brief may be provided under the instructions section or subsections only if it is imperative information to assist with the procedure.
- All steps shall be a numbered list (`1.`). It is recommended to mark every step as `1.` rather than incrementing, as markdown viewers automatically render them in order `1. 1. 1. > 1. 2. 3.`, and it alleviates the potential for typos.
- Each step is a terse imperative.
- Sub-steps are allowed as either a numbered (`1.1.`) or unordered (`-`) list.
- For any step that has a sub-step picture, codeblock, or is otherwise applicable, it should end in a colon.

#### Codeblocks

- User-supplied values are displayed as `{placeholder}` in braces.
- Any change directory command (`cd`) only lives in the scope of the section/subsection it appears in. A `cd` command must be given at the start of each section, even if the directory is unchanged from the previous section.
- When a command carries placeholders, add a concrete `# Example` beneath it in the same fence:

  ```
  scp -r {local_app_file_path} {username}@{host_ip}:{deployment_directory}

  # Example
  scp -r ~/downloads/zest-v1.0.zip admin@10.0.0.2:/home/admin/zest/v1.0
  ```

- Annotate a command with a trailing comment when its purpose is not obvious: `sudo apt install python3-tk  # Tkinter for TCL communication`.
