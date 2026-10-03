


<img width="1456" height="720" alt="xr1" src="https://github.com/user-attachments/assets/530e71ce-bf83-4dbd-bfde-52a3f124d3eb" />





<p align="left">
  <img src="https://img.shields.io/badge/PYTHON-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/HASHLIB-000000?style=for-the-badge&logo=security&logoColor=white" alt="Hashlib">
  <img src="https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white" alt="JSON">
  <img src="https://img.shields.io/badge/OS-333333?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="OS">
  <img src="https://img.shields.io/badge/RSA-00599C?style=for-the-badge&logo=codeforces&logoColor=white" alt="RSA">
  <img src="https://img.shields.io/badge/LINUX-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux">
</p>

# IDA Pro License Generator and Binary Patcher

## Overview
This repository contains an automated Python script designed to generate customized license files (`idapro.hexlic`) for IDA Pro and perform byte-level modifications (RSA modulus patches) on the application's core binaries (`ida32.dll`, `ida.dll`, shared libraries `.so`, and framework files `.dylib`) to ensure compatibility and validation of additional plugins and architectures.

---

## Key Features
- **Custom License Generation:** Constructs a structured JSON license file featuring user metadata, extended expiration dates, and add-on activations.
- **Cryptographic Signing (RSA / SHA-256):** Implements modular exponentiation mathematical operations using predefined private keys and secure hash digests to cryptographically sign the license payload.
- **Massive Add-on Inclusion:** Automates the insertion of all available processor architecture add-ons (x86, x64, ARM, MIPS, PowerPC, RISC-V, ARC) directly into the license parameters.
- **Automatic Binary Patching:** Dynamically searches for the original Hex-Rays public module inside the application's binary files, replaces the hexadecimal buffer with the modified key, and outputs a backed-up copy with the `.patched` extension.

---

## Technologies and Modules Used
The script is built entirely using Python's standard library, leveraging specific built-in modules for data manipulation, file handling, and cryptographic hashing:
- **`hashlib`**: Utilized specifically for invoking SHA-256 (`hashlib.sha256()`) to compute secure digests over the alphabetical JSON data strings.
- **`json`**: Utilized for structural serialization, indentation-free formatting, and alphabetical key sorting (`json.dumps(obj, sort_keys=True)`) to preserve signature validity.
- **`os`**: Utilized to interface with the operating system file paths (`os.path.exists()`) to verify target binaries before executing byte replacements.
- **`RSA / BigInt Math`**: Custom byte-to-integer conversion routines (`int.from_bytes` / `to_bytes`) handling low-level modular exponentiation via Python native integer handling.

---

## Code Structure
The script's execution flow is divided into the following logical blocks:

1. **Payload Definition:** Initial configuration of owner details, product identifiers (`ida-pro`), extended validity dates up to the year 2033, and support activations.
2. **Add-on Extension Function (`add_every_addon`):** Iterates over distinct processor architectures to inject corresponding add-on codes (`HEXX86`, `HEXX64`, `HEXARM`, `HEXARM64`, etc.) into the license structure.
3. **Cryptography and Bigint Handling:** 
   - `buf_to_bigint` and `bigint_to_buf`: Convert byte streams into little-endian integers to perform RSA operations.
   - `encrypt` and `decrypt`: Message encryption and decryption functions using a fixed exponent and custom RSA moduli (`pub_modulus_patched` and `private_key`).
4. **Digital Signature (`sign_hexlic`):** Generates the initialized data buffer, appends the SHA-256 hash of the JSON content, and encrypts the result to obtain the final hexadecimal signature.
5. **Patcher Generator (`generate_patched_dll`):** 
   - Reads binary files in binary mode (`rb`).
   - Searches for the hexadecimal sequence corresponding to the original Hex-Rays modulus (`EDFD425CF978`).
   - Replaces the block with the modified variant (`EDFD42CBF978`).
   - Exports the result as a `.patched` file ready to be renamed or substituted.

---

## Supported Binaries for Patching
The script automatically evaluates the presence of the following binaries in its execution directory to apply matching modifications:
- `ida32.dll` (Windows 32-bit)
- `ida.dll` (Windows 64-bit)
- `libida32.so` (Linux 32-bit)
- `libida.so` (Linux 64-bit)
- `libida32.dylib` (macOS 32-bit)
- `libida.dylib` (macOS 64-bit)

---

## Usage
Execute the script directly from the terminal using Python:

```bash
python IDA..py
