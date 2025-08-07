# ASM Cheat Creation Guide

This guide provides a walkthrough for creating and editing assembly (ASM) cheats. We'll use a practical "Max Gold on Change" cheat as our example to cover two primary workflows: creating a cheat from scratch and modifying a pre-existing one.

For a comprehensive explanation of the underlying concepts, such as code caves and memory relocation, please refer to the [Code Cave and Relocation Workflow](readme.md#code-cave-and-relocation-workflow) section in our main documentation.

## The Goal: "Max Gold on Change"

Our objective is to create a cheat that sets your gold to **99,999,999** the moment the game attempts to modify its value.

Here is the final cheat in its two common forms:

**1. Human-Readable Assembly**
```asm
// This is the main section where we hook the game code.
// We replace the original instruction with a jump to our code cave.
[main]
0x8754EC: b 0x982DB0 // Jump to our new code

// This defines the start of our new code block in a free memory region.
cave_start = 0x982DB0

// --- Start of Code Cave Logic ---

// Load our desired value (99,999,999) into the target register 'w21'.
ldr w21, max_gold

// Execute the original instruction we overwrote. This is critical.
str w21, [x19, #0x49c]

// Return to the normal game flow.
b 0x8754F0

// Define the data for our 'max_gold' label. The assembler will
// place this value in memory for the 'ldr' instruction to access.
max_gold: .word 99999999
```

**2. Machine-Readable Raw Code**
```
[Max gold on Change]
[Main+R10+0x00008754EC] = 0x14043631  b #0x982db0
[Main+R10+0x0000982DB0] = 0x18000075  ldr w21, #0x982dbc
[Main+R10+0x0000982DB4] = 0xB9049E75  str w21, [x19, #0x49c]
[Main+R10+0x0000982DB8] = 0x17FBC9CE  b #0x8754f0
[Main+R10+0x0000982DBC] = 0x5F5E0FF  .word 99999999
```

---

## Method 1: Creating a Cheat from Scratch

This method is for when you are the original author of the cheat, starting from zero.

### Step 1: Identify the Target Game Instruction

First, use a debugger to pinpoint the exact line of code responsible for changing the gold value. In our case, you discover it's this instruction:

```asm
[Main+R10+0x00008754EC] = 0xB9049E75  str w21, [x19, #0x49c]
```
This instruction stores the value from register `w21` into a memory address. Our goal is to intercept this action and change the value of `w21` right before it's saved.

### Step 2: Assemble the Code Cave

Since we need to insert our own logic, we must create a **code cave**—a separate, unused area of memory where our custom instructions can reside.

1.  **Start with the Original Instruction:** Paste the raw opcode of the instruction you found into the center panel to get its assembly representation.
    ```
    [code that change gold]
    040A0000 008754EC B9049E75
    ```
    The disassembler output will give you:
    ```
    [code that change gold]
    [Main+R10+0x00008754EC] = 0xB9049E75  str w21, [x19, #0x49c]
    ```
    **Copy to Assembler input:** Copy the disassembler output to the Assembler input window. This recreates the editable cheat assembly source in the assembler. This is the starting point to create your target cheat.

2.  **Enter Edit Mode:** Right-click the code in the assembler input and select the context menu item **"Edit ASM"**.

3.  **Build the Hook:** In the assembler panel, create a hook that redirects the game's execution. You'll replace the original instruction at `0x8754EC` with a branch (`b`) to a free memory location (your future code cave).
    ```asm
    [main]
    0x8754EC: b 0x982DB0 // Jump to our new code
    ```

4.  **Write the Code Cave Logic:** Now, create the code cave itself.
    *   Define a new `cave_start` at your chosen location (`0x982DB0`).
    *   Load your desired value into the register (`ldr w21, max_gold`).
    *   Execute the original instruction you overwrote (`str w21, [x19, #0x49c]`).
    *   Jump back to the instruction *after* the hook to resume normal game flow (`b 0x8754F0`).
    *   Define your data (`max_gold: .word 99999999`).

5.  **The Final Code:** Here is the complete, manually-crafted code you would write in the assembler panel:
    ```asm
    // This is the main section where we hook the game code.
    // We replace the original instruction with a jump to our code cave.
    [main]
    0x8754EC: b 0x982DB0 // Jump to our new code

    // This defines the start of our new code block in a free memory region.
    cave_start = 0x982DB0

    // --- Start of Code Cave Logic ---

    // Load our desired value (99,999,999) into the target register 'w21'.
    ldr w21, max_gold

    // Execute the original instruction we overwrote. This is critical.
    str w21, [x19, #0x49c]

    // Return to the normal game flow.
    b 0x8754F0

    // Define the data for our 'max_gold' label. The assembler will
    // place this value in memory for the 'ldr' instruction to access.
    max_gold: .word 99999999
    ```

After assembling this code, you will have successfully created the "Max Gold" cheat from scratch.

> **🤖 Pro Tip: Automate with the "Easy Hack Template"**
>
> Once you are comfortable with the manual process, you can significantly speed up your workflow. The **Easy Hack Template** automates the entire code cave creation process.
>
> **How it Works:**
>
> 1.  In the ASM Editor, right-click the single instruction you want to hook (e.g., `str w21, [x19, #0x49c]`).
> 2.  Select **"Easy Hack Template"** from the context menu.
>
> The tool will automatically transform that single line into a complete, ready-to-edit code cave structure.
>
> **Before:**
> ```asm
> cave_start = 0x8754EC
> m_8754ec: str w21, [x19, #0x49c]
> ```
>
> **After:**
> ```asm
> cave_start = 0x8754EC
> m_8754ec: str w21, [x19, #0x49c]
> m_8754ec: b 0x11E28928 // Patched with a jump to the new cave
> //
>
> cave_start = 0x11E28928 // A new code cave is created
> ldr w21, a             // Template code to load a new value
> original: str w21, [x19, #0x49c] // The original instruction is preserved
> return:
> b m_8754ec + 4         // Jump back to the original code flow
> a: .word 100           // A placeholder value for you to change
> ```
>
> This automation handles the boilerplate—creating the jump, finding a free memory location, preserving the original instruction, and setting up the return jump—letting you focus immediately on the core cheat logic (like changing the `.word` value).

---

## Method 2: Reusing and Modifying an Existing Cheat

This is a powerful workflow for when you find an existing cheat (often as a raw opcode) and want to adapt it or create a variation.

### Step 1: Start with the Raw Opcodes

You'll often find cheats online in their raw, machine-readable format. For our example, you find the "Max Gold" cheat like this:
```
[Max gold on Change]
040A0000 008754EC 14043631
040A0000 00982DB0 18000075
040A0000 00982DB4 B9049E75
040A0000 00982DB8 17FBC9CE
040A0000 00982DBC 05F5E0FF
```

### Step 2: Disassemble and Prepare for Editing

1.  **Disassemble:** Paste the raw opcodes into the **opcode panel** (center). The disassembler on the right will automatically convert it into human-readable assembly.
2.  **Copy to Assembler:** Right-click the disassembled code in the right panel and select **"Copy to left panel"**. This recreates the editable cheat source in the assembler.
3.  **Enter Edit Mode:** Right-click the code in the assembler (left panel) and select the context menu item **"Edit ASM"**.

### Step 3: Modify the Logic

Once the code is in the dedicated editor, you can make your changes.
*   **For simple value changes,** you can edit the assembly directly in the assembler panel (e.g., changing `.word 99999999` to `.word 500`).
*   **For more complex changes,** you can add new instructions, use labels for clarity, and restructure the cheat's logic. You can also create a new variant to preserve the original cheat.

### Step 4: Expand the Cheat with "Relocate All"

What if your new logic requires more space than the original code cave provides?

1.  **Add Your Code:** Write your new instructions in the assembler, even if it seems to overwrite other memory.
2.  **Relocate:** Click the **"Relocate All"** button. This feature automatically moves all code caves to a new, sequential block of free memory and updates all internal branches (`b`, `ldr`, etc.) to point to the correct new locations.

This allows you to expand and modify cheats without manually managing memory addresses, making the process far more flexible and robust.
