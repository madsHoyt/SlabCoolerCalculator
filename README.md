# CoolerCalculator – Heat Transfer Simulation
This program is adapted from Benjamin DEGLO DE BESSES' CoolDT calculator to be used for **slabs instead of pipes**.  
It is a heat transfer simulation program that calculates **cool-down times through multi-layered slabs**.  

The program uses **WebAssembly (WASM)** so it can run directly in a browser.  

Note: Calculations from this program are **not guaranteed to be accurate**. Use for educational or estimation purposes only.

Note: The following notes are for my personal journal of my process to turn the program into a WebAssembly app.
## Table of Contents

1. Getting Started
2. Building the Backend
3. Running CoolDT
4. Updating the Web Backend
5. Viewing Output
6. Notes

7. Getting Started

---

## 1. Requirements (for Cygwin/Windows build):

-   Cygwin64 installed
-   gcc, make, and other development packages installed in Cygwin
-   Source code located inside your Cygwin home directory (e.g., /home/cooler)

## 2. Building the Backend (Standalone Executable)

Open the Cygwin64 terminal and navigate to your source folder:

    cd /home/cooler

Run the Makefile to compile the core executable:

    make -f Makefile

After successful compilation, a cooldt.exe file will be created in the same folder.

## 3. Running CoolDT


Use the included example input file:

    ./cooldt.exe example.dat

This will produce an output file, typically:

    cat example.txt

The .txt file contains calculated cooldown results.

## 4. Updating the Web Backend

To compile the project to WebAssembly for web use, run the following in PowerShell:

    PS C:\cygwin64\home\cooler> emcc source/main.c source/algebra.c source/alloc.c source/automatic.c source/check-data.c source/input.c source/mesh.c source/messages.c source/solution.c source/steady-state.c source/transient.c -o cooldt.js -s EXPORTED_FUNCTIONS='["_main"]' -s EXPORTED_RUNTIME_METHODS='["FS","callMain","ccall","cwrap"]' -s ALLOW_MEMORY_GROWTH=1 -s MODULARIZE=1 -s EXPORT_NAME="CoolDTModule" -s FORCE_FILESYSTEM=1 -O3

After the build, you can verify the generated WebAssembly files:

    PS C:\cygwin64\home\cooler> ls cooldt.*

This produces:

-   cooldt.js – JavaScript wrapper to call the WASM module
-   cooldt.wasm – compiled WebAssembly binary

## 5. Viewing Output

-   Use `cat example.txt` to view output in the terminal.
-   You can also open example.txt with Notepad or VSCode for easier reading.

## 6. Notes


-   Keep source code inside the Cygwin home folder.
-   Make small changes, then rebuild with `make -f Makefile` for testing.
-   The program is currently standalone; no Excel/XLL integration is required anymore.
