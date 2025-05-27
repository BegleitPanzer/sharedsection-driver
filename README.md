# SharedSection Driver

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Description

The `SharedSection Driver` is a Windows Kernel Driver designed to facilitate communication between processes using shared memory sections. This project aims to provide robust and efficient methods for reading and writing physical and virtual memory, as well as other low-level operations.

---

## Features

- **Memory Manipulation**:
  - Read and write physical memory.
  - Translate linear addresses.
  - Access and modify virtual memory.

- **Kernel-Level Operations**:
  - Direct interaction with kernel objects and memory regions.
  - Custom implementations for memory operations (`memcpy`, `memset`, etc.).

- **Communication Mechanism**:
  - Shared memory-based communication between user-space and kernel-space.

- **Security Considerations**:
  - Includes cryptographic utilities for secure data handling.
  - Well-structured namespaces with inline methods for optimized performance.

---

## Repository Structure

- **`client/`**: Contains the user-space components for interacting with the driver.
  - `comm.cpp`: Implements physical memory read/write operations.

- **`driver/`**: Kernel-mode driver implementation.
  - `kernel/physical/`: Functions for manipulating physical and virtual memory.
  - `hide/`: Utilities for obfuscating and securing driver operations.
  - `comm/`: Communication handler for different request types.

- **`shared/`**: Shared utilities between user-space and kernel-space.

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/vasie1337/sharedsection-driver.git
   cd sharedsection-driver
   ```

2. Build the driver using a compatible Windows Driver Kit (WDK).

3. Load the driver using a kernel-mode driver loader (e.g., OSR Loader).

---

## Usage

### Writing Physical Memory
```cpp
bool Comm::WritePhysicalMemory(std::uint64_t Address, void* Buffer, std::size_t Size)
{
    shared_section->type = comm_type::write_physical;
    shared_section->address = Address;
    shared_section->buffer = reinterpret_cast<std::uint64_t>(Buffer);
    shared_section->size = Size;

    SetEvent(event_handle);
    WaitForSingleObject(event_handle_response, INFINITE);
    ResetEvent(event_handle_response);

    return true;
}
```

### Reading Physical Memory
```cpp
bool Comm::ReadPhysicalMemory(std::uint64_t Address, void* Buffer, std::size_t Size)
{
    shared_section->type = comm_type::read_physical;
    shared_section->address = Address;
    shared_section->buffer = reinterpret_cast<std::uint64_t>(Buffer);
    shared_section->size = Size;

    SetEvent(event_handle);
    WaitForSingleObject(event_handle_response, INFINITE);
    ResetEvent(event_handle_response);

    return true;
}
```

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes.

---

## Acknowledgments

Special thanks to all contributors and supporters of this project.
