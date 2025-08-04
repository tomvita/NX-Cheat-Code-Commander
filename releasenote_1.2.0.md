# Release Notes: Version 1.2.0

## New Features

### 4-Panel Mode for Enhanced Comparison

A new "4-panel mode" has been introduced to provide a more powerful and flexible environment for comparing opcodes. This feature is designed to help you track changes and analyze differences between two sets of opcodes side-by-side.

#### Key Functionalities:

*   **Toggle 4-Panel Mode**: A new "4 Panel Mode" checkbox has been added to the main toolbar. When enabled, the application switches to a four-panel layout, displaying two opcode panels in the center.
*   **Independent Updates**:
    *   The **center-left panel** is updated whenever the assembly code in the far-left panel is modified.
    *   The **center-right panel** is updated when a new opcode file is opened, allowing you to load a separate set of opcodes for comparison.
*   **Synchronized Disassembly**:
    *   Changes in the **center-left panel** now trigger an update in the far-right disassembly panel, providing real-time feedback on your assembly modifications.
    *   Changes in the **center-right panel** also update the disassembly panel, allowing you to analyze the loaded opcodes.
*   **Copy to Right**: A new context menu option, "Copy to right," has been added to the center-left panel. This allows you to quickly copy the entire content from the center-left panel to the center-right panel, making it easy to set a new baseline for comparison. The far-right panel will update automatically after the copy.

This feature is ideal for situations where you need to see how your assembly changes affect the resulting opcodes while keeping a reference set of opcodes in view.

### Synchronized Scrolling

To improve navigation and streamline your workflow, synchronized scrolling has been implemented across all text panels.

*   **Click to Sync**: A left-click on a cheat name in any of the main text panels (Assembler, Opcode, or Disassembly) or the logs will now automatically scroll all other panels to display the corresponding cheat.
*   **Highlighting**: The clicked cheat name will be highlighted in yellow for two seconds, making it easy to spot.
*   **Seamless Navigation**: This feature ensures that you can quickly jump to the relevant section in all views, making it easier to track and debug your cheats.