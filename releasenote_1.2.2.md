# Release Notes: Version 1.2.2

## New Features and Enhancements
*   **Context Menu in Assembly Editor:** A new context menu has been added to the assembly editor, providing quick access to common templates to create a cheat from scratch, starting with a cheat code that writes the recovery code for a hook (i.e., the original instruction in the game that the soon-to-be-created cheat will hook).
    *   **Easy hack template:** This template is designed for simple value modifications. It works with `ldr` and `str` instructions and allows you to easily change the value being loaded or stored.
    *   **Use branch to code cave template:** This template replaces the selected instruction with a branch to a new code cave. This allows you to write more complex assembly code without having to worry about overwriting existing game code.
    *   **Use branch to code cave template with push to stack:** This template is similar to the "branch to code cave" template, but it also includes instructions to save and restore registers `X25` and `X26`. This is useful when you need to use these registers in your code cave without interfering with the game's normal execution.
*   **Double-Click to Highlight:** You can now double-click on text in the assembly editor to highlight all instances of that text, making it easier to track references and usages.

