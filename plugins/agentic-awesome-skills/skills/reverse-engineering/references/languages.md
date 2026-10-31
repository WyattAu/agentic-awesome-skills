# Split on comma (input read): each segment handles one character
- Loop-based multiplication: `[->>+++<<]` multiplies by 3; count the inner operations
**Key insight:** BF programs that check input are structurally simple; each input byte is compared against a constant built by incrementing a cell. Extract the increment counts to recover the expected input without running the program.
**Key insight:** BF input validation programs are sequential; they read one character, check it, and only read the next if it matches. The character causing more reads is correct because the program advances past the validation gate to check the next position.
- `[-]`; clear cell (set to 0)
- `[->+<]`; move cell right
- `<[-<->] +<[>-<[-]]>[-<+>]`; equality comparison of two cells
**Key insight:** Code coverage is a powerful oracle; it tells you which conditional paths were taken. Any encryption with data-dependent branching leaks information through coverage.
**Key requirement:** Compile that exact Python version to disassemble bytecode; alpha/beta versions have different opcodes than stable releases.