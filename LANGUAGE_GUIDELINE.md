# Language Guideline

## Document Information

### Scope

This document covers the voice, grammar, and naming conventions used across these repositories.

### History

| Date       | Author     | Description |
| ---------- | ---------- | ----------- |
| 2026-07-10 | Zach Smith | Release     |

# Guideline

## Instructions

This section applies to instructions such as installation and usage procedures found in READMEs or other project documentation.

- A brief may be provided under the instructions section or subsections only if it is imperative information to assist with the procedure.
- All steps shall be a numbered list (`1.`). It is recommended to mark every step as `1.` rather than incrementing, as markdown viewers automatically render them in order `1. 1. 1. > 1. 2. 3.`, and it alleviates the potential for typos.
- Each step is a terse imperative.
- Sub-steps are allowed as either a numbered (`1.1.`) or unordered (`-`) list.
- For any step that has a sub-step picture, codeblock, or is otherwise applicable, it should end in a colon.

### Codeblocks

- User-supplied values are displayed as `{placeholder}` in braces.
- When a command carries placeholders, add a concrete `# Example` beneath it in the same fence:

  ```
  scp -r {local_app_file_path} {username}@{host_ip}:{deployment_directory}

  # Example
  scp -r ~/downloads/zest-v1.0.zip admin@10.0.0.2:/home/admin/zest/v1.0
  ```

- Annotate a command with a trailing comment when its purpose is not obvious: `sudo apt install python3-tk  # Tkinter for TCL communication`.
