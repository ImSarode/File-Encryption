# File Encryption Using AVL Trees and Huffman Coding

A **C++ text-processing and encoding project** that combines a Caesar cipher, hash maps, an AVL tree, and Huffman coding. It reads a text file, counts words, adds Caesar-shifted versions of the input tokens to the frequency data, organizes the results in a self-balancing tree, and generates variable-length Huffman codes.

> **Project status:** This is a demonstration of encryption-related and data-structure techniques, **not a secure file-encryption utility**. The current program prints frequency information and Huffman codes to the terminal; it does not write an encrypted output file or implement decryption.

## Features

- **Text file input:** Reads a user-specified text file.
- **Word-frequency analysis:** Converts original text to lowercase, removes non-alphanumeric characters from each token, and counts occurrences using `std::unordered_map`.
- **Caesar cipher transformation:** Applies a fixed shift of **7** to alphabetic characters in the file's original tokens, preserving letter case and non-letter characters.
- **Combined frequencies:** Merges counts for the original normalized words and Caesar-shifted tokens.
- **AVL tree:** Stores word/count pairs in a balanced binary search tree and prints them using inorder traversal.
- **Huffman coding:** Uses a min-priority queue to build a Huffman tree and generate binary codes based on the combined frequencies.

## Technologies

- **Language:** C++
- **Standard library:** `iostream`, `fstream`, `sstream`, `unordered_map`, `vector`, `queue`, and related headers
- **Concepts:** Caesar cipher, hash maps, AVL trees, priority queues, Huffman coding
- **Dependencies:** No third-party libraries

## Project Structure

```text
File-Encryption-main/
├── main.cpp               # File reading, frequency analysis, and program flow
├── avl_tree.hpp           # AVL tree insertion, rotations, and traversal
├── caesar_cipher.hpp      # Caesar cipher transformation
├── huffman.hpp            # Huffman tree construction and code generation
├── RANDOM_GENERATOR.hpp   # Caesar-shifted word frequency processing
├── test.txt               # Sample input text
└── README.md              # Project documentation
```

## How It Works

1. **Read the file:** The program asks for a filename and opens it.
2. **Count normalized words:** Original words are lowercased, stripped of non-alphanumeric characters, and counted in a hash map.
3. **Process shifted tokens:** The file is read a second time and each whitespace-separated token is transformed using a Caesar shift of 7. Counts for these tokens are collected separately.
4. **Merge frequencies:** The shifted-token counts are added to the original-word frequency map.
5. **Build an AVL tree:** The combined word/count pairs are inserted into a self-balancing tree.
6. **Generate Huffman codes:** The tree's entries become Huffman leaves. A priority queue combines the lowest-frequency nodes into a coding tree.
7. **Display results:** The terminal shows the AVL tree's inorder traversal and the binary Huffman codes.

```text
Input text file
      |
      +--> Normalize words --> Count original words --+
      |                                                |
      +--> Caesar shift (+7) --> Count shifted tokens --+ 
                                                       |
                                               Merge frequency maps
                                                       |
                                                   AVL Tree
                                                       |
                                                Huffman Coding
                                                       |
                                           Print frequencies and codes
```

## Getting Started

### Prerequisites

Install a C++ compiler such as **g++**. Because the code uses `std::random_shuffle` in a helper function, **C++14** is a suitable compilation target. (That helper is not called in the current main workflow.)

### 1. Get the source code

Download this repository as a ZIP and extract it, or clone it using your repository URL:

```bash
git clone <YOUR_REPOSITORY_URL>
cd File-Encryption-main
```

### 2. Fix the header filename mismatch

On case-sensitive systems (including most Linux systems), `main.cpp` includes `random_generator.hpp`, but the file in this repository is named `RANDOM_GENERATOR.hpp`.

**Either** rename the file:

```bash
mv RANDOM_GENERATOR.hpp random_generator.hpp
```

**Or** change the corresponding line in `main.cpp` to:

```cpp
#include "RANDOM_GENERATOR.hpp"
```

Only one of these changes is required.

### 3. Compile

From the project directory, run:

```bash
g++ -std=c++14 main.cpp -o file_encryption
```

### 4. Run

On Linux or macOS:

```bash
./file_encryption
```

On Windows (after compiling with g++):

```powershell
.\file_encryption.exe
```

When prompted, enter the path to an existing text file, for example:

```text
Enter the input file name: test.txt
```

The file path is interpreted relative to the directory where you launch the program unless you supply an absolute path.

## Example

Suppose the input file contains:

```text
hello hello world
```

The original-word counts are:

```text
hello: 2
world: 1
```

The Caesar-shifted tokens (shift = 7) are:

```text
hello -> olssv
world -> dvysk
```

The resulting combined frequency data includes:

```text
hello: 2
world: 1
olssv: 2
dvysk: 1
```

The program then prints these entries via AVL inorder traversal and generates a Huffman code for each word. **The exact Huffman bit strings may vary**, particularly when frequencies are tied.

## Implementation Notes and Limitations

- The Caesar shift is fixed at **7** in `processWords()`.
- The shifted-token pass preserves punctuation and capitalization, while the original-word pass normalizes both. These two passes therefore use different token formats.
- The file `RANDOM_GENERATOR.hpp` includes random-word and salting helper functions, but the current `processWords()` implementation **does not perform random salting**. It returns frequencies of Caesar-shifted words.
- Huffman codes are displayed, **not used to encode and save the input file**.
- There is **no decryption**, key management, ciphertext output, or secure cryptographic primitive.
- `HuffmanCoding::buildFromAVL()` assumes at least one word exists. An empty file (or a file producing no entries) may cause the program to fail.
- This project is intended for **learning and experimentation**, not protecting sensitive information.

## Possible Future Improvements

- Encode file contents using the generated Huffman codes and save the output.
- Implement decoding and round-trip verification.
- Handle empty input safely.
- Use consistent token normalization across processing stages.
- Replace deprecated `std::random_shuffle` with `std::shuffle` and a modern random engine.
- Add command-line arguments, unit tests, and error handling.
- For actual security, use an established authenticated-encryption library rather than a Caesar cipher.

## Contributing

Contributions and suggestions are welcome. Open an issue to discuss changes or submit a pull request with improvements.

## License

No license file is included in the provided project. Add a `LICENSE` file if you want to specify terms for reuse and distribution.
