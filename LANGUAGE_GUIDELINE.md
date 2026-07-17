# Language Guideline

## Document Information

### Scope

This document covers the voice, grammar, and naming conventions used across codebases and technical documentation.

### History

| Date       | Author     | Description                         |
| ---------- | ---------- | ----------------------------------- |
| 2026-07-10 | Zach Smith | Release                             |
| 2026-07-17 | Zach Smith | Add General Language, Code sections |

# Guideline

## General Language

- Impersonal language should be used. Avoid using direct addressing such as "I", "you", etc.
- A direct imperative mood is recommended
- Writing should be terse

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

## Code

### Names

- All names should be verbose except with repo-specific established terms e.g. `serial_number_per_customer` not `sn_cus`
- Each segment of names should be ordered from largest-scope to smallest-scope e.g. `name_first` not `first_name`
- Booleans should read as a yes/no question e.g. `is_primary`, `enabled`

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

Comments should always be current state and never reference the state of previous code or mention migrations.

Comments do not need to be added to modified code unless the code necessitates a comment - reasons for modification should be captured in commit descriptions rather than code.

#### Categories

Comments have 3 categories:

- Inline comments: Comments for a single line of code
- Block comments: Comments for a code block that performs a single action
- Section comments: Comments for a large code block with multiple actions

The prefixes (`Inline comment:`, `Block comment:`, `Section comment:`) shown in the examples below are for illustration only and are not part of the required format.

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

## Documentation

## Instructions
### Instructions

This section applies to instructions such as installation and usage procedures found in READMEs or other project documentation.

- A brief may be provided under the instructions section or subsections only if it is imperative information to assist with the procedure.
- All steps shall be a numbered list (`1.`). It is recommended to mark every step as `1.` rather than incrementing, as markdown viewers automatically render them in order `1. 1. 1. > 1. 2. 3.`, and it alleviates the potential for typos.
- Each step is a terse imperative.
- Sub-steps are allowed as either a numbered (`1.1.`) or unordered (`-`) list.
- For any step that has a sub-step picture, codeblock, or is otherwise applicable, it should end in a colon.

#### Codeblocks

- User-supplied values are displayed as `{placeholder}` in braces.
- When a command carries placeholders, add a concrete `# Example` beneath it in the same fence:

  ```
  scp -r {local_app_file_path} {username}@{host_ip}:{deployment_directory}

  # Example
  scp -r ~/downloads/zest-v1.0.zip admin@10.0.0.2:/home/admin/zest/v1.0
  ```

- Annotate a command with a trailing comment when its purpose is not obvious: `sudo apt install python3-tk  # Tkinter for TCL communication`.
