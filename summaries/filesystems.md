# WANIX Filesystem Implementations Specification

## Overview

This document provides a summary of various filesystem implementations used in the WANIX project, an experimental, web-native, Unix-like operating and development environment. These implementations enable WANIX to work with diverse data sources using a consistent filesystem interface, supporting its goal of creating a flexible and powerful web-based computing environment.

## Implementations

### 1. githubfs

**Purpose**: Provides a read-write filesystem interface for GitHub repositories within WANIX.

**Key Features**:

- Full read-write access to GitHub repository contents
- Caching to reduce API calls
- Support for multiple branches
- File operations (read, write, create, delete)

**Assessment**:
Enables WANIX users to interact with GitHub repositories as if they were local filesystems, facilitating easier integration of GitHub-hosted content into the WANIX environment.

### 2. httpfs

**Purpose**: Offers a read-only filesystem interface over HTTP within WANIX.

**Key Features**:

- Read-only access to remote filesystems
- File watching capabilities
- Works with any HTTP server implementing specific endpoints

**Assessment**:
Allows for easy integration of remote, HTTP-accessible file storage into WANIX applications, providing a familiar filesystem interface for read operations.

### 3. indexedfs

**Purpose**: Implements a filesystem interface using the browser's IndexedDB API for WANIX.

**Key Features**:

- Read-write access to local browser storage
- Persistent storage across sessions
- Implements standard filesystem operations

**Assessment**:
Enables WANIX to have a filesystem-like interface for local storage, useful for offline capabilities and managing larger amounts of structured data than other web storage APIs allow.

### 4. mountablefs

**Purpose**: Allows composing multiple filesystems into a single interface within WANIX.

**Key Features**:

- Mount other filesystems at specific directories
- Route filesystem operations to the appropriate underlying filesystem
- Handle cross-filesystem operations
- Support for a wide range of filesystem operations

**Assessment**:
Allows for creating more complex filesystem structures in WANIX by combining multiple filesystems. This enables the creation of unified views of diverse data sources (e.g., local files, remote repositories, browser storage) accessible through a single filesystem interface.

### 5. osfs

**Purpose**: Provides a filesystem interface in WANIX that directly interacts with the operating system's file operations.

**Key Features**:

- Implements standard filesystem operations using OS-level functions
- Handles path conversions between filesystem and Unix-style paths
- Supports all standard file operations (create, read, write, delete, etc.)

**Assessment**:
Enables WANIX applications to interact with the local filesystem using a consistent interface that matches other custom filesystem implementations. This allows for easier swapping between different filesystem backends in WANIX applications.

### 6. procfs

**Purpose**: Provides a filesystem interface in WANIX to interact with process information.

**Key Features**:

- Represents running processes as files in a virtual filesystem
- Allows reading process information in JSON format
- Supports terminating processes through file removal
- Read-only for most operations, with process termination as an exception

**Assessment**:
Enables WANIX applications to interact with process information using a familiar filesystem interface, facilitating easy access to and manipulation of process data within the WANIX environment.

## Comparative Analysis

1. **Flexibility**:

   - `mountablefs` is the most flexible, able to compose multiple filesystem types.
   - `osfs` provides direct access to the local filesystem.
   - `procfs` provides a specialized interface for process information.
   - Others are specialized for specific backends (GitHub, HTTP, IndexedDB).

2. **Read/Write Capabilities**:

   - `githubfs`, `indexedfs`, `mountablefs`, and `osfs` offer full read-write operations.
   - `httpfs` is read-only.
   - `procfs` is primarily read-only, with write capabilities limited to process termination.

3. **Environment**:

   - `indexedfs` is specifically for web browsers.
   - `osfs` is designed for direct interaction with the local operating system.
   - `procfs` is designed to work with a process management service, typically in a Unix-like environment.
   - Others can potentially run in any Go environment.

4. **Persistence**:

   - `githubfs` persists data on GitHub servers.
   - `httpfs` relies on the remote HTTP server for persistence.
   - `indexedfs` persists data locally in the browser.
   - `mountablefs` depends on its constituent filesystems for persistence.
   - `osfs` persists data on the local filesystem.
   - `procfs` doesn't persist data; it provides a real-time view of running processes.

5. **Complexity**:

   - `mountablefs` has additional complexity in routing operations and handling cross-filesystem scenarios.
   - `osfs` is relatively simple, directly mapping to OS-level operations.
   - `procfs` has a focused implementation for representing process information as a filesystem.
   - Others have focused implementations for their specific use cases.

6. **Specialized Functionality**:
   - `procfs` uniquely provides process management capabilities through a filesystem interface.
   - `githubfs` offers specific features for working with Git repositories.
   - `httpfs` includes file watching capabilities over HTTP.
   - `indexedfs` provides browser-based storage with a filesystem interface.

## Conclusion

These filesystem implementations provide powerful abstractions for working with diverse data sources within the WANIX environment. They enable WANIX to interact with various storage systems and system information using a consistent interface, simplifying the development of applications that need to handle multiple data sources or system interactions.

The suite of implementations covers a wide range of use cases within WANIX:

- `githubfs` for GitHub integration
- `httpfs` for read-only access to remote resources
- `indexedfs` for browser-based storage
- `mountablefs` for composing complex filesystem structures
- `osfs` for direct local filesystem access
- `procfs` for process information and management

By providing a consistent filesystem interface across these diverse backends, WANIX can offer a flexible and powerful environment that can adapt to different storage or information sources as needed. This approach promotes better code organization, reusability, and adaptability within the WANIX ecosystem, supporting its goal of creating a web-native, Unix-like operating and development environment.
