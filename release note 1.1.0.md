### Key Additions:
- **Scan ASM for Address** This tool helps you prepare for the "Relocate All" function by mapping all memory addresses modified by your cheats. This allows you to identify the starting point of the code cave used by the cheat's author, which is essential for a successful relocation.
  - **Important Note:** This feature is most effective when the original author built the code cave from the top down (from a low address to a high one). If the code was built from the bottom up, a manual search for the true starting address may still be necessary.
- **Set Free Code Start from Address Map:** For added ease, you can right-click on your choice to set the "Free Code Start" address.
- **Recreate Cheat Source from Opcodes:** A new right-click context menu option in the disassembler (right panel) called **"Copy to left panel"** allows you to recreate an editable cheat source from raw opcodes. This is the primary method for modifying cheat files when you do not have the original source code.

### How These Features Improve Your Workflow:
- **Quick start to Relocations:** The Address Usage Mapper save you time in finding free code space start.
- **Source Code Recreation:** Making coping from disassembler quicker when you don't have any source for assembler yet and want to copy everything.