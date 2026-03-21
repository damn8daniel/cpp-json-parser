# C++ JSON Parser

A high-performance JSON parser written in modern C++17. Features a hand-written lexer, recursive descent parser, streaming mode for processing files over 1 GB, and multithreaded validation.

## Features

- **Lexer + Recursive Descent Parser** -- tokenizer and parser built from scratch with detailed error reporting (line and column numbers)
- **`std::variant`-based value representation** -- type-safe JSON values without inheritance overhead
- **Streaming mode for 1 GB+ files** -- processes large files in chunks using approximately 50 MB of memory
- **Multithreaded validation** -- validates JSON structure across multiple threads for faster feedback on large inputs
- **Tolerant parsing** -- continues parsing after encountering errors, collecting all issues in a single pass
- **Path-based search** -- query nested values using dot-notation paths (e.g., `root.users[0].name`)
- **JSON generator** -- generates random JSON files of configurable size, up to 10 GB, for testing and benchmarking
- **Pretty-print serialization** -- outputs parsed JSON with configurable indentation

## Tech Stack

- **Language:** C++17
- **Build system:** CMake 3.10+
- **Threading:** POSIX Threads (`std::thread`, `std::mutex`)

## Building

```bash
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)
```

The executable `jsonparser` will be created in the `build/` directory.

## Usage

Run the parser interactively:

```bash
./build/jsonparser
```

The program presents a menu-driven interface:

| Menu Item | Description |
|-----------|-------------|
| [1] | Load a JSON file |
| [2] | Display structure (tree view) |
| [3] | Search by path (e.g., `user.address.city`) |
| [4] | Edit a value |
| [5] | Add an element |
| [6] | Delete an element |
| [7] | Save to file |
| [8] | Validate JSON |
| [9] | Statistics |
| [10] | Execution metrics |
| [11] | Generate JSON file (up to 100 MB) |
| [12] | Validate with error counting |
| [13] | System information |
| [14] | Multithreaded validation |
| [15] | Generate large files (1--10 GB) |

### Large file handling

When loading a file larger than 500 MB, the parser offers a choice:

- **Streaming validation** -- validates without loading the entire file into memory (~50 MB RAM, ~40 seconds for 2 GB)
- **Tolerant loading** -- skips errors and loads all valid elements into memory

## Performance

| Operation | Size | Time | Memory |
|-----------|------|------|--------|
| File generation | 100 MB | ~10 sec | ~50 MB |
| File generation | 1 GB | ~5 min | ~50 MB |
| Full loading | 100 MB | ~5 sec | ~200 MB |
| Streaming validation | 1 GB | ~40 sec | ~50 MB |
| Tolerant loading | 1 GB | ~120 sec | ~2 GB |

## Architecture

```
include/
  Lexer.hpp             -- Token types and lexer interface
  Parser.hpp            -- Recursive descent parser
  JsonValue.hpp         -- std::variant-based JSON value type
  Serializer.hpp        -- JSON pretty-printer
  Generator.hpp         -- Random JSON file generator
  Validator.hpp         -- Structural validator
  ParallelProcessor.hpp -- Multithreaded chunk processing
  SystemInfo.hpp        -- Runtime system information
  ProgressBar.hpp       -- Console progress indicator

src/
  Lexer.cpp             -- Tokenizer implementation (strings, numbers, keywords)
  Parser.cpp            -- Recursive descent parsing logic
  JsonValue.cpp         -- Value accessors and type utilities
  Serializer.cpp        -- Indented JSON output
  Generator.cpp         -- Configurable random JSON generation
  Validator.cpp         -- Tolerant validation with error collection
  ParallelProcessor.cpp -- Thread pool for parallel validation
  main.cpp              -- CLI entry point and menu system
```

## Requirements

- **Compiler:** C++17 support (GCC 7+, Clang 5+, MSVC 2017+)
- **CMake:** 3.10+
- **OS:** Linux, macOS, Windows
- **RAM:** 4 GB minimum (8+ GB recommended for large file processing)

## License

This project is provided as-is for educational and practical use.
