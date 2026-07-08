# Liasis Programming Language

> **"Python simplicity. C++ performance. AI built into the language."**

Liasis is a next-generation, AI-first programming language designed to be the absolute easiest and fastest way to build AI applications and bare-metal systems. It combines the clean, significant-indentation syntax of Python with the memory safety, security, and native performance of Rust and C++ through source-to-source compilation.

---

## Key Features

* **🧠 AI-First Design**: Native APIs for LLMs, image generation, speech, and agents are built right into the core language. No external imports, packages, or SDK installations are required.
* **⚡ High-Performance Native Compilation**: Liasis compiles directly to safe, highly-optimized Rust code, which is then compiled using `rustc` / `LLVM` into optimized native executables, executing significantly faster than Python.
* **🟢 Minimal Resource Footprint**: Consumes up to 90% less RAM and significantly lower CPU usage compared to Python by bypassing heavy runtime VM execution and garbage collection cycles.
* **🔒 Hardened memory management**: Bypasses Python's heavy object-wrapping and runtime GC overhead. It features a manual, C-speed memory allocation namespace (`Memory`) hardened with built-in AddressSanitizer-style bounds checking to protect against buffer overflows, double frees, and segmentation faults.
* **💾 Bare-Metal OS Compilation**: Supports compiling Liasis scripts directly into raw, bare-metal `x86_64` kernels packaged into bootable floppy disk images (`.img`) for VMs (QEMU, VirtualBox) or physical hardware.
* **🐍 Pythonic Syntax**: Brace-free, clean, readable, using significant indentation and intuitive syntax structures.

---

## Installation

Liasis is built on top of the Rust toolchain. Ensure you have `cargo` and `rustc` installed.

1. Clone or download this repository.
2. Build the toolchain using Cargo:
   ```bash
   cargo build --release
   ```
3. Add the compiled binary `target/release/liasis` (or `target/release/liasis.exe` on Windows) to your system path.

---

## CLI Manual

Use the `liasis` command-line executable to manage your projects:

* `liasis new <project_name>`: Scaffold a new project structure.
* `liasis run <file.lis>`: Compile and run a Liasis program instantly.
* `liasis build <file.lis>`: Build a release-ready, stand-alone native binary.
* `liasis build-os <file.lis>`: Compile Liasis code into a bootable bare-metal floppy disk OS image.
* `liasis test`: Run your project's test suite.
* `liasis fmt <file.lis>`: Auto-format a Liasis source file.
* `liasis lint <file.lis>`: Run static analysis and warning checks on your code.
* `liasis repl`: Launch the interactive Liasis shell.

---

## Tutorial: Code Cookbooks

### 1. Interactive LLM Chat
AI model loading and prompt streaming are built directly into the language.
```python
fn main():
    print("Loading Llama LLM model natively...")
    model = AI.load("llama")
    
    print("Ask the AI anything (type 'exit' to quit):")
    while true:
        prompt = input("> ")
        if prompt == "exit":
            break
            
        # Stream the prompt directly into the model using the << operator
        reply = model << prompt
        print(reply)
```

### 2. C-Style Hardened Memory Allocation
Execute manual heap operations at hardware speed with automatic security bounds validation.
```python
fn main():
    print("=== Raw Memory Allocation ===")
    # Allocate 1024 bytes on the heap
    ptr = Memory.alloc(1024)
    
    # Write a test integer directly to the raw memory address
    Memory.write_int(ptr, 987654321)
    
    # Read it back securely
    val = Memory.read_int(ptr)
    print(val) # Prints 987654321
    
    # Attempting to read outside the 1024-byte range will trigger a panic
    # preventing memory corruption vulnerabilities:
    # val_bad = Memory.read_int(ptr + 1024) # Panics immediately
    
    # Free memory manually
    Memory.free(ptr, 1024)
```

### 3. Writing a Bare-Metal OS Kernel
Compile a Liasis script into a bootable x86 floppy image.
```python
# Save as kernel.lis and run: liasis build-os kernel.lis
fn main():
    # Write directly to the VGA video memory buffer
    VGA.clear(0x0A) # Clear screen to light green
    VGA.write_string(10, 20, "Welcome to Liasis OS!", 0x0E) # Print string at row 10, col 20
```
