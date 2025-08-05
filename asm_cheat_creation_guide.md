# ASM Cheat Creation Guide

This guide demonstrates how to create and edit assembly (ASM) cheats using a practical example: a "Max Gold on Change" cheat. We will cover two primary methods: creating a cheat from scratch and modifying a pre-existing cheat.

## The Goal: "Max Gold on Change" Cheat

Our objective is to create a cheat that sets your gold to 99,999,999 whenever the game tries to change its value.

Here is the final cheat in its two common forms:

**Assembly Code (The human-readable version):**
```asm
// Define a new location for our code cave
cave_start = 0x982DB0

// Load the max gold value (99,999,999) into w21.
ldr w21, max_gold 

// Execute the original instruction we overwrote.
str w21, [x19, #0x49c]

// Jump back to the original code flow.
b 0x8754F0

// Data label for our max gold value.
max_gold: .word 99999999

// The main hook that replaces the original game code.
[main]
0x8754EC: b 0x982DB0
```

**Raw Code (The machine-readable version):**
```
[Max gold on Change]
[Main+R10+0x00008754EC] = 0x14043631  b #0x982db0
[Main+R10+0x0000982DB0] = 0x18000075  ldr w21, #0x982dbc
[Main+R10+0x0000982DB4] = 0xB9049E75  str w21, [x19, #0x49c]
[Main+R10+0x0000982DB8] = 0x17FBC9CE  b #0x8754f0
[Main+R10+0x0000982DBC] = 0x5F5E0FF  .word 99999999
```

---

## Method 1: Creating the Cheat from Scratch

This method is for when you are the original creator of the cheat.

### Step 1: Identify the Target Game Instruction

First, you use a debugger to find the exact line of code responsible for changing the gold value. You discover it's this instruction:
```asm
[Main+R10+0x00008754EC] = 0xB9049E75  str w21, [x19, #0x49c]
```
This code stores the value from register `w21` into memory. Our goal is to change `w21` just before this happens.

### Step 2: Plan and Write the Cheat

Since we need to add new instructions, we must use a code cave.

> **Pro Tip: Start with the Opcode**
> A fast way to begin is by using the raw opcode of the instruction you found. This is efficient because it requires less typing, and the original opcode can be saved and used as a "restore code" to disable your cheat later.
>
> For our example, you could start with the raw "code that changes gold":
> ```
> [code that change gold]
> 040A0000 008754EC B9049E75
> ```
> The disassembler will convert this into the assembly instruction (`str w21, [x19, #0x49c]`), which you can then copy into the assembler window to begin building your code cave.

### Step 2: Plan and Write the Cheat

1.  **Open the Assembler:** Right-click the instruction at `0x8754EC` and select "Edit ASM". You will be presented with a template like this:

    ```asm
    cave_start = 0x8754EC
    m_8754ec: str w21, [x19, #0x49c]
    //
    ```
    This template is a good starting point to create your cheat with use of a code cave.

2.  **Modify the Template:** Now, you edit this template to build the full cheat.

    *   First, change the instruction at the hook address (`m_8754ec`) to a branch (`b`) that jumps to your chosen code cave location.
    *   Next, define a new `cave_start` at that chosen location (`0x982DB0`).
    *   Write your new logic inside this new code cave.
    *   Finally, ensure you return to the original code flow by jumping to the instruction *after* the hook. The `m_8754ec` label makes this easy—you can simply jump to `m_8754ec + 4`.

3.  **The Final Code:** Here is the fully edited and commented code you would write in the assembler panel:

    ```asm
    // This is the main section where we hook the game code.
    // We replace the original instruction with a jump to our code cave.
    // the cave_start line is the one that makes the label m_8754ec assume the address 0x8754EC
    cave_start = 0x8754EC
    m_8754ec: b 0x982DB0

    // This defines the start of our new code block in a free memory region.
    cave_start = 0x982DB0

    // --- Start of Code Cave Logic ---

    // Load our desired value (99,999,999) into the target register 'w21'.
    ldr w21, max_gold

    // Execute the original instruction we overwrote. This is critical.
    str w21, [x19, #0x49c]

    // Return to the normal game flow. The m_8754ec label gives us a
    // convenient way to calculate the return address.
    b m_8754ec + 4

    // Define the data for our 'max_gold' label. The assembler will
    // place this value in memory for the 'ldr' instruction to access.
    max_gold: .word 99999999
    ```

After writing this and assembling, you will have successfully created the "Max Gold" cheat from scratch.

---

## Method 2: Reusing and Modifying an Existing Cheat

This is a powerful workflow for when you find an existing cheat (often as a raw opcode) and want to adapt it or create a variation.

### Step 1: Start with the Raw Opcodes

You typically find cheats online in their raw, machine-readable format. For our example, you find the "Max Gold" cheat like this:
```
[Max gold on Change]
040A0000 008754EC 14043631
040A0000 00982DB0 18000075
040A0000 00982DB4 B9049E75
040A0000 00982DB8 17FBC9CE
040A0000 00982DBC 05F5E0FF

```

### Step 2: Disassemble to Make it Usable

This raw code isn't easy to edit. You need to convert it back into human-readable assembly.

1.  Paste the raw opcodes into opcode panel. (center or center right)
2.  The disassembler will convert it into the full assembly logic, which you can then copy into the assembler panel.

### Step 3: Edit the Code

Once the code is in the assembler panel, you have two ways to modify it:

**A) Simple In-Place Editing**

For minor changes, you can edit the assembly directly in the assembler window. This is perfect for changing a single value or instruction without altering the code's structure.

*   **Example:** To change the max gold amount to 500, you would simply find the line `[Main+R10+0x0000982DBC] = 0x5F5E0FF  .word 99999999` and change the last part to `.word 500`. The assembler will update the opcode for you.

**B) Advanced Editing with "Edit ASM"**

For more significant changes, such as adding new instructions, using labels, or expanding the code cave, you should use the full editor.

1.  Right-click the disassembled code in the assembler panel and select "Edit ASM".
2.  This opens the code in a dedicated editor. Crucially, this gives you the option to **create a new variant** of the cheat, leaving the original "Max Gold" cheat untouched.
3.  In the new editor window, you can easily:
    *   **Add new lines of code:** For instance, adding a conditional check before loading the max gold value.
    *   **Use labels:** As seen in Method 1, you can use labels like `max_gold:` to make your code more readable and maintainable, which is not possible with in-place editing.
    *   **Restructure the cheat:** Make significant changes to the logic to create your new version.

### Step 4: Expanding the Cheat with "Relocate All"

What if your new logic requires more space? The original code cave might not be large enough, or it might conflict with another cheat. This is where `Relocate All` becomes essential.

Let's say you need to add several new instructions to your "Max Gold" cheat, but the space after `0x982DBC` is already used by another cheat.

1.  **Add your new code:** Write the new instructions in the assembler panel, even if it overwrites the next cheat's memory space.
2.  **Use "Relocate All":** Click the "Relocate All" button. This powerful feature automatically performs the following actions:
    *   It relocate all code cave into sequential address within the free code space.
    *   It updates your cheat's `cave_start` and all its internal branches (`b`, `ldr`, etc.) to point to the correct new locations.
    *   After relocate assuming there is enough free code space to cater for all cheat you are done, if not you have to decide which one to keep or use less space so there is enough for all.

This feature allows you to expand and modify cheats without manually managing memory or worrying about breaking other cheats, making the process much more flexible and robust.
