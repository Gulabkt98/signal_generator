# Digital Signal Generator

A small interactive C++ program that demonstrates line coding, scrambling, and simple modulation techniques with a real-time OpenGL/GLUT visualization.

Project file:

- `signal_generator.cpp` — single-file program implementing encoding schemes, simple PCM/Delta modulation from analog samples, scrambling (B8ZS/HDB3), a few analytical helpers, and an OpenGL-based waveform display.

## Features

- Accepts either binary input or analog samples (PCM / Delta Modulation) via console.
- Line coding options: NRZ-L, NRZ-I, Manchester, Differential Manchester, AMI.
- Scrambling for AMI: B8ZS and HDB3.
- Simple PCM (uniform quantization) and Delta Modulation conversions from analog samples to bitstreams.
- Analytical helpers:
  - Longest palindrome (Manacher's algorithm) in input bitstring.
  - Longest contiguous zero sequence in encoded signal.
- Visualizes the encoded waveform using OpenGL/GLUT.

## Quick analysis / important notes

- The program is interactive and reads options from stdin, then opens an OpenGL window to visualize the produced signal.
- When you select Manchester or Differential Manchester the emitted signal length doubles (each input bit becomes two waveform samples) — the renderer accounts for this via a flag.
- The program uses fixed-size buffers in some places (e.g. `char data[1000]`), so very long inputs may overflow; keep input lengths modest (under ~900 chars) or modify the source to use a dynamic container.
- For AMI scrambling, `apply_B8ZS` and `apply_HDB3` modify the encoded array in-place.
- The PCM routine uses simple uniform quantization into 2^bits levels; delta modulation uses a fixed delta (0.5). These are minimal educational implementations.

## Dependencies

- A C++ compiler (g++, clang++, or MSVC).
- OpenGL and a GLUT implementation (e.g., FreeGLUT) installed on your system.

On Windows with MinGW/MSYS2, install packages like `mingw-w64-x86_64-toolchain` and `mingw-w64-x86_64-freeglut` (package names depend on distribution). With Visual Studio, add GLUT/freeglut include/lib and link against appropriate libraries.

## Build (Windows PowerShell example, MinGW/MinGW-w64)

If you have MinGW with `g++` and FreeGLUT installed and your compiler can find the headers and libs, a typical command is:

```powershell
g++ signal_generator.cpp -o signal_generator.exe -lfreeglut -lopengl32 -lglu32 -lwinmm -lgdi32
```

Notes:
- The exact linker flags may vary depending on how FreeGLUT is provided on your system. If your distribution provides `-lglut` instead of `-lfreeglut`, use that.
- If includes or libs aren't found, specify include and lib paths with `-I` and `-L` flags respectively (e.g. `-IC:/path/to/freeglut/include -LC:/path/to/freeglut/lib`).

## Build (Visual Studio)

1. Create a new Win32/Console project and add `signal_generator.cpp` to the project.
2. Add include directories to point to your GLUT/freeglut headers.
3. Add linker input: `freeglut.lib`, and ensure the directory with `freeglut.lib` is on the Linker -> Additional Library Directories.
4. Ensure `opengl32.lib` and `glu32.lib` are linked (usually provided by Visual Studio's Windows SDK).

## Run / Usage

Run the produced executable from a terminal (PowerShell example):

```powershell
.\\signal_generator.exe
```

Follow the interactive prompts. Example session:

- Program asks whether the input is Digital (1) or Analog (2).
- For Digital, enter a binary string like `1011001`.
- Choose an encoding (e.g. `3` for Manchester).
- For AMI you can choose scrambling and B8ZS/HDB3.
- The program prints analysis info (longest palindrome, zero sequences) then opens an OpenGL window showing the waveform. Close the window to exit.

## Example

Input (interactive):

- Choice: `1` (Digital)
- Binary data: `0011000011110000`
- Encoding: `5` (AMI)
- Scrambling: `1` then `1` for B8ZS

Console output will show the encoded sequence and analysis then the visualization.

## Limitations & suggested improvements

- Input buffers are statically sized in places (e.g. `data[1000]`) — consider using `std::string` or `std::vector<int>` to avoid overflows.
- The PCM quantizer uses a uniform quantizer and simple step computation; consider handling the equal `max==min` case and adding better quantization rounding.
- Delta modulation uses a fixed delta (0.5); make delta configurable or adaptive.
- Improve error handling of user input (currently the program assumes well-formed numeric and binary input).
- Add command-line options for non-interactive usage (e.g., pass data and options directly as args) to facilitate automated testing.

## Files changed

- `README.md` — added. Describes the project, build/run instructions, code summary and suggestions.

## License

No license is included in the repository. If you want this code published with a license, add a `LICENSE` file and choose an appropriate license (MIT, Apache-2.0, etc.).

----

If you'd like, I can also:
- Try to compile the program on your machine and report any build errors (I can run the compile command in your PowerShell here). 
- Update the source to replace static buffers with dynamic containers and add basic input validation.

Tell me which follow-up you'd like next.
