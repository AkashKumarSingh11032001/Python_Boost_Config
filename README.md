# Standalone Environment Setup Script

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Shell](https://img.shields.io/badge/shell-bash-green.svg)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg)](https://www.linux.org/)

A complete, standalone environment setup script that installs custom Python and Boost environments from source code without external dependencies. Designed specifically for project with full automation and zero-configuration setup.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Command Reference](#command-reference)
- [Examples](#examples)
- [Installation Process](#installation-process)
- [Environment Configuration](#environment-configuration)
- [Troubleshooting](#troubleshooting)
- [Performance](#performance)
- [Contributing](#contributing)

## Overview

The Standalone Environment Setup Script (`standalone_master_setup.sh`) provides a complete solution for setting up development environments with custom Python and Boost installations built from source. Unlike other setup scripts, this works entirely standalone without requiring pre-existing Python or Boost installations.

### Key Differentiators

- **True Standalone**: No external Python/Boost dependencies required
- **Source-Based Installation**: Builds Python and Boost from official source code
- **Production Ready**: Optimized builds with shared libraries and performance flags
- **Smart Caching**: Reuses downloaded sources and skips existing installations
- **Multi-Mirror Support**: Robust download with fallback mirrors
- **Root Installation**: System-wide installation in `/opt/` directories

## Features

- ✅ **Complete Source Installation**: Builds Python and Boost from scratch
- ✅ **Optimized Builds**: Enables LTO, computed gotos, and other optimizations
- ✅ **Smart Download Management**: Multiple mirrors, corruption detection, resume capability
- ✅ **Installation Detection**: Skips existing installations, handles upgrades
- ✅ **Environment Script Generation**: Creates activation scripts for easy use
- ✅ **Verification System**: Comprehensive installation testing
- ✅ **System Integration**: Proper library cache updates and path configuration
- ✅ **Error Recovery**: Robust error handling and cleanup procedures

## Prerequisites

### System Requirements

- **Operating System**: Linux (Ubuntu 18.04+, CentOS 7+, RHEL 7+)
- **Architecture**: x86_64
- **Root Access**: Required for system-wide installation
- **Internet Connection**: For downloading source packages
- **Disk Space**: ~2GB for build process, ~500MB for final installation

### Required System Packages

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install build-essential wget curl tar cmake pkg-config \
           libssl-dev libffi-dev zlib1g-dev libbz2-dev \
           libreadline-dev libsqlite3-dev libncurses5-dev \
           libgdbm-dev libnss3-dev libssl-dev
```

**CentOS/RHEL:**
```bash
sudo yum groupinstall "Development Tools"
sudo yum install wget curl tar cmake pkgconfig openssl-devel \
         libffi-devel zlib-devel bzip2-devel readline-devel \
         sqlite-devel ncurses-devel gdbm-devel nss-devel
```

## Quick Start

### Most Useful Command

For production development environments, use this command to create and install a complete Python + Boost environment:

```bash
sudo bash standalone_master_setup.sh --install
```

This command will:
1. ✅ Check system requirements and dependencies
2. ✅ Download Python 3.8.10 source code (with verification)
3. ✅ Build and install optimized Python to `/opt/python3.8.10/`
4. ✅ Download Boost 1.82.0 source code (with fallback mirrors)
5. ✅ Build and install Boost with Python bindings to `/opt/boost_1_82_0_python38/`
6. ✅ Create environment activation script (`setup_boost_python38.sh`)
7. ✅ Verify installation with comprehensive testing
8. ✅ Configure system library paths

**Expected Output:**
```
[10:30:15] Standalone Environment Setup
[INFO] Python Version: 3.8.10
[INFO] Boost Version: 1.82.0
[INFO] Action: install

[10:30:15] Checking system requirements...
[10:30:15] System requirements check passed
[10:30:15] Creating complete setup script: python3810_boost1820_setup.sh
[10:30:16] Running installation...
[10:30:16] Starting Python 3.8.10 + Boost 1.82.0 setup...
[10:30:16] Building and installing Python 3.8.10 to /opt/python3.8.10...
[10:45:32] Python 3.8.10 installed successfully
[10:45:32] Building and installing Boost 1.82.0 with Python 3.8.10 support...
[11:15:45] Boost 1.82.0 installed successfully
[11:15:45] Environment script created: setup_boost_python38.sh
[11:15:46] Installation verification successful!
[11:15:46] Setup completed successfully!

To use this environment:
 source setup_boost_python38.sh
 python3 --version
```

**Installation Time**: ~45 minutes on modern hardware

## Usage

### Basic Syntax

```bash
sudo bash standalone_master_setup.sh [OPTIONS] [PYTHON_VERSION] [BOOST_VERSION]
```

### Default Versions

- **Python**: 3.8.10 (Stable, -tested)
- **Boost**: 1.82.0 (Latest stable with Python 3.8 support)

### Sudo Requirements

⚠️ **Important**: This script requires root privileges because it:
- Installs to system directories (`/opt/`)
- Updates system library cache (`ldconfig`)
- Modifies system library configuration (`/etc/ld.so.conf.d/`)

## Command Reference

| Option | Description | Root Required | Use Case |
|--------|-------------|---------------|----------|
| `--help`, `-h` | Show detailed usage information | ❌ | Getting started |
| `--check-system` | Validate system requirements only | ❌ | Pre-installation check |
| `--list-versions` | Show supported Python/Boost versions | ❌ | Version planning |
| `--create-only` | Generate installation script without running | ❌ | Script inspection |
| `--install` | **[Recommended]** Create and run full installation | ✅ | Production setup |

## Examples

### 1. Pre-Installation System Check
```bash
bash standalone_master_setup.sh --check-system
```
**Output:**
```
[10:30:15] Checking system requirements...
[10:30:15] System requirements check passed
```

### 2. Production Installation (Recommended)
```bash
sudo bash standalone_master_setup.sh --install
```
Installs Python 3.8.10 + Boost 1.82.0 with full optimization.

### 3. Custom Version Installation
```bash
sudo bash standalone_master_setup.sh --install 3.10.12 1.83.0
```
Installs specific Python and Boost versions.

### 4. View Supported Versions
```bash
bash standalone_master_setup.sh --list-versions
```
**Output:**
```
Common Python Versions:
 3.8.10, 3.8.18
 3.9.15, 3.9.18
 3.10.8, 3.10.12
 3.11.4, 3.11.6

Common Boost Versions:
 1.80.0, 1.81.0, 1.82.0, 1.83.0, 1.84.0

Recommended Combinations:
 Python 3.8.10 + Boost 1.82.0 (default)
 Python 3.10.12 + Boost 1.83.0 (latest)
```

### 5. Generate Installation Script Only
```bash
bash standalone_master_setup.sh --create-only 3.8.10 1.82.0
```
Creates: `python3810_boost1820_setup.sh` without executing it.

### 6. Manual Installation Control
```bash
# Generate script
bash standalone_master_setup.sh --create-only

# Review the generated script
cat python3810_boost1820_setup.sh

# Run with specific options
sudo bash python3810_boost1820_setup.sh --install # Python + Boost only
sudo bash python3810_boost1820_setup.sh --all   # Full installation + verification
```

## Installation Process

### Phase 1: Python Installation (15-20 minutes)

1. **Source Download**
  - Downloads from `https://www.python.org/ftp/python/`
  - Verifies archive integrity with tar test
  - Reuses existing downloads if valid

2. **Compilation**
  - Configures with optimizations: `--enable-optimizations`, `--with-lto`
  - Builds with shared library support: `--enable-shared`
  - Uses all CPU cores: `make -j$(nproc)`

3. **Installation**
  - Installs to `/opt/python{VERSION}/`
  - Updates system library cache
  - Creates library configuration

### Phase 2: Boost Installation (20-25 minutes)

1. **Source Download**
  - Tries multiple mirrors for reliability:
   - GitHub releases (primary)
   - Official Boost website
   - JFrog Artifactory
   - Archives.boost.io
   - SourceForge (fallback)

2. **Configuration**
  - Bootstrap with custom Python: `--with-python=/opt/python{VERSION}/bin/python3`
  - Configures Python version and paths

3. **Compilation**
  - Builds with specific Python support
  - Creates shared libraries for runtime linking
  - Multi-threaded build: `-j$(nproc)`

### Phase 3: Environment Setup (1-2 minutes)

1. **Script Generation**
  - Creates activation script with all required environment variables
  - Sets up PATH, LD_LIBRARY_PATH, PYTHONPATH, CMAKE_PREFIX_PATH

2. **Verification**
  - Tests Python executable
  - Verifies Boost.Python libraries
  - Checks pkg-config availability

## Environment Configuration

### Installation Locations

```
/opt/python3.8.10/        # Python installation
├── bin/python3         # Python executable
├── lib/libpython3.8.so     # Shared library
├── include/python3.8/     # Headers
└── lib/python3.8/       # Standard library

/opt/boost_1_82_0_python38/   # Boost installation
├── include/boost/       # Boost headers
├── lib/libboost_python38.so  # Boost.Python library
└── lib/cmake/         # CMake configuration
```

### Generated Environment Script

**File**: `setup_boost_python38.sh`

```bash
#!/bin/bash

# Environment setup for Python 3.8.10 + Boost 1.82.0
export PYTHON_ROOT="/opt/python3.8.10"
export BOOST_ROOT="/opt/boost_1_82_0_python38"
export PYTHON_VERSION="3.8.10"
export BOOST_VERSION="1.82.0"

# Update PATH
export PATH="${PYTHON_ROOT}/bin:${PATH}"

# Update library path
export LD_LIBRARY_PATH="${PYTHON_ROOT}/lib:${BOOST_ROOT}/lib:${LD_LIBRARY_PATH}"

# Update Python path
export PYTHONPATH="${BOOST_ROOT}/lib/python3.8/site-packages:${BOOST_ROOT}/lib:${PYTHONPATH}"

# CMake configuration
export CMAKE_PREFIX_PATH="${PYTHON_ROOT}:${BOOST_ROOT}:${CMAKE_PREFIX_PATH}"

# Boost-specific variables
export BOOST_INCLUDEDIR="${BOOST_ROOT}/include"
export BOOST_LIBRARYDIR="${BOOST_ROOT}/lib"
```

### Using the Environment

**Activate in current session:**
```bash
source setup_boost_python38.sh
```

**Verify installation:**
```bash
python3 --version                  # Should show: Python 3.8.10
python3 -c "import sys; print(sys.executable)"   # Should show: /opt/python3.8.10/bin/python3
ls ${BOOST_ROOT}/lib/libboost_python*.so      # Should list Boost.Python libraries
```

**CMake Integration:**
```cmake
find_package(Boost REQUIRED COMPONENTS python)
find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
```

## Troubleshooting

### Common Issues

#### 1. "This script must be run as root"
**Cause**: Script requires root privileges for system installation
**Solution**: 
```bash
sudo bash standalone_master_setup.sh --install
```

#### 2. "Missing required tools"
**Cause**: Build dependencies not installed
**Solution**: Install development packages
```bash
# Ubuntu/Debian
sudo apt-get install build-essential cmake

# CentOS/RHEL
sudo yum groupinstall "Development Tools"
```

#### 3. "Failed to download Python/Boost"
**Cause**: Network connectivity or mirror issues
**Solutions**:
- Check internet connection
- Try again (script will resume from cache)
- Manual download to `/tmp/` build directories

#### 4. Compilation errors
**Cause**: Missing development libraries
**Solution**: Install complete development environment
```bash
# Ubuntu
sudo apt-get install build-essential libssl-dev libffi-dev zlib1g-dev

# CentOS
sudo yum install openssl-devel libffi-devel zlib-devel
```

#### 5. "Installation verification failed"
**Cause**: Library linking issues
**Solution**: Update library cache
```bash
sudo ldconfig
source setup_boost_python38.sh
```

### Advanced Troubleshooting

#### Enable Debug Mode
```bash
sudo bash -x standalone_master_setup.sh --install
```

#### Check Installation Status
```bash
# Check Python installation
ls -la /opt/python*/bin/python3

# Check Boost installation 
ls -la /opt/boost_*/lib/libboost_python*.so

# Check library cache
ldconfig -p | grep python
ldconfig -p | grep boost
```

#### Manual Cleanup
```bash
# Remove installations
sudo rm -rf /opt/python* /opt/boost_*

# Remove library configs
sudo rm -f /etc/ld.so.conf.d/python*.conf

# Update cache
sudo ldconfig
```

### Log Analysis

The script provides detailed logging with timestamps:
- 🟢 **[HH:MM:SS] Message**: Progress updates
- 🔵 **[INFO] Message**: Configuration information 
- 🟡 **[WARNING] Message**: Non-fatal issues
- 🔴 **[ERROR] Message**: Critical errors requiring attention

## Performance

### Build Times (Typical Hardware)

| Component | Time | Notes |
|-----------|------|--------|
| System Check | < 1 second | Dependency validation |
| Python Download | 1-3 minutes | ~25MB download |
| Python Compilation | 15-20 minutes | With optimizations |
| Boost Download | 2-5 minutes | ~100MB download |
| Boost Compilation | 20-25 minutes | Multi-threaded build |
| Environment Setup | 1-2 minutes | Script generation + verification |
| **Total** | **40-50 minutes** | **Complete installation** |

### Hardware Requirements

- **CPU**: Multi-core recommended (utilizes all cores)
- **RAM**: 4GB minimum, 8GB recommended
- **Disk**: 2GB temporary space, 500MB final installation
- **Network**: Broadband for source downloads

### Optimization Features

- **Python Optimizations**: LTO, computed gotos, profile-guided optimization
- **Boost Optimizations**: Release variant, shared libraries, multi-threading
- **Build Parallelization**: Uses all CPU cores (`-j$(nproc)`)
- **Smart Caching**: Reuses downloads and skips existing installations

## Generated Scripts Reference

### Master Installation Script

**File**: `python{VERSION}_boost{VERSION}_setup.sh`

**Options**:
- `--install`: Install Python and Boost only
- `--setup-only`: Create environment scripts only 
- `--verify`: Verify existing installation
- `--all`: Full installation + verification (default)

**Example**:
```bash
sudo ./python3810_boost1820_setup.sh --all
```

### Environment Activation Script

**File**: `setup_boost_python{VERSION}.sh`

**Usage**:
```bash
source setup_boost_python38.sh
```

**Features**:
- Sets all required environment variables
- Provides verification commands
- Shows usage instructions

## Integration Examples

### With Project

```bash
# Setup environment
sudo bash standalone_master_setup.sh --install
source setup_boost_python38.sh

```

### With CMake Projects

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Find packages (will use environment paths)
find_package(Boost REQUIRED COMPONENTS python)
find_package(Python3 REQUIRED COMPONENTS Interpreter Development)

# Your targets
add_executable(myapp main.cpp)
target_link_libraries(myapp ${Boost_LIBRARIES} ${Python3_LIBRARIES})
```

### With CI/CD

```yaml
# .github/workflows/build.yml
- name: Setup Environment
 run: |
  sudo bash standalone_master_setup.sh --install
  echo "source $(pwd)/setup_boost_python38.sh" >> $GITHUB_ENV
  
- name: Build Project
 run: |
  source setup_boost_python38.sh
  cmake . && make
```

## Best Practices

### Production Deployment

1. **Pre-validate Environment**
  ```bash
  bash standalone_master_setup.sh --check-system
  ```

2. **Use Default Versions**: Stick with tested Python 3.8.10 + Boost 1.82.0

3. **Cache Downloads**: Keep `/tmp/` directories for re-installations

4. **Verify Installation**
  ```bash
  source setup_boost_python38.sh
  python3 -c "import sys; print('Python OK:', sys.version)"
  ```

### Development Workflow

1. **Environment Activation**: Always source environment script
2. **Version Consistency**: Use same versions across team
3. **Documentation**: Include environment requirements in project README

## Author

**Script Information:**
- **File**: `standalone_master_setup.sh`
- **Purpose**: Standalone environment setup with source compilation
- **Compatibility**: Linux x86_64 systems
- **Version**: 1.0

## Support

For issues with this script:
1. Check [Troubleshooting](#troubleshooting) section
2. Verify system requirements
3. Enable debug mode for detailed logs
4. Report issues with complete error output

---

**Ready to get started?** Run the most useful command:
```bash
sudo bash standalone_master_setup.sh --install
```
