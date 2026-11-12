# Module 3: Doxygen — Automated Code Documentation

**Doxygen** is the de facto industry standard documentation generator for languages such as C, C++, Java, Python, C#, PHP, and Fortran. It automatically extracts comments, class hierarchies, function signatures, and call graphs directly from your codebase to generate browsable HTML websites, PDF reference manuals, and XML files.

---

## 📑 Table of Contents

1. [Introduction to Doxygen](#1-introduction-to-doxygen)
2. [How Doxygen Works](#2-how-doxygen-works)
3. [The `Doxyfile` Configuration](#3-the-doxyfile-configuration)
4. [Doxygen Comment Syntaxes](#4-doxygen-comment-syntaxes)
5. [Essential Doxygen Tags & Commands](#5-essential-doxygen-tags--commands)
6. [Documenting Classes, Methods, & Files](#6-documenting-classes-methods--files)
7. [Comprehensive C++ Code Example](#7-comprehensive-c-code-example)
8. [Comprehensive Python Code Example](#8-comprehensive-python-code-example)
9. [Generating and Viewing the Documentation](#9-generating-and-viewing-the-documentation)
10. [Advanced Features (Graphviz Diagrams)](#10-advanced-features-graphviz-diagrams)

---

## 1. Introduction to Doxygen

### Why Automate Documentation?
- **Single Source of Truth:** Code and documentation stay together in the same files, preventing documentation from drifting out of date.
- **Multi-Format Generation:** Produces responsive HTML sites (with instant search), searchable chm, PDF manuals (via LaTeX), and RTF reports.
- **Architectural Discovery:** Automatically graphs class inheritance trees, caller/callee graphs, and header inclusion relationships using Graphviz.
- **Strict Software Engineering:** Enforces clarity in function signatures, parameter boundaries, and return types across development teams.

---

## 2. How Doxygen Works

The generation workflow takes source code and a configuration file (`Doxyfile`) and compiles it into browsable documentation:

```text
+-----------------------+
|  Source Files         |
|  (.cpp, .hpp, .py)    |
+-----------------------+
           |
           +------------------> [ Doxygen Engine ]
           |                           |
+-----------------------+              v
|  Configuration File   |      +------------------------+
|  (Doxyfile)           |      | Output Formats:        |
+-----------------------+      | - html/index.html      |
                               | - latex/refman.pdf     |
                               | - xml/ / rtf/          |
                               +------------------------+
```

---

## 3. The `Doxyfile` Configuration

The `Doxyfile` is plain text containing key-value configuration directives.

### 1. Generating a Default Template:
```bash
# Generate a default configuration file named 'Doxyfile':
doxygen -g Doxyfile
```

### 2. Crucial Configuration Directives:

| Setting | Recommended Value | Purpose |
|---------|-------------------|---------|
| `PROJECT_NAME` | `"MyProject"` | Name displayed in headers and browser tabs |
| `PROJECT_NUMBER` | `"1.0.0"` | Version number displayed alongside project name |
| `OUTPUT_DIRECTORY` | `"docs"` | Directory where generated HTML and LaTeX will be stored |
| `INPUT` | `src include README.md` | Paths to source code files and markdown landing pages |
| `RECURSIVE` | `YES` | Traverse subdirectories recursively |
| `FILE_PATTERNS` | `*.cpp *.hpp *.h *.py` | File extensions to inspect |
| `EXTRACT_ALL` | `YES` | Extract documentation even for undocumented entities |
| `EXTRACT_PRIVATE` | `YES` | Include private class members (great for internal docs) |
| `GENERATE_HTML` | `YES` | Build modern responsive web documentation |
| `GENERATE_LATEX` | `NO` (or `YES`) | Generate LaTeX source for compiling a PDF manual |
| `USE_MDFILE_AS_MAINPAGE`| `README.md` | Sets the project `README.md` as the HTML homepage |

---

## 4. Doxygen Comment Syntaxes

Doxygen detects special comment markers. Both **JavaDoc style** (`/** ... */`) and **C++ style** (`///`) are supported:

### Style 1: JavaDoc Style (Most Popular)
```cpp
/**
 * @brief Brief summary line.
 *
 * Detailed explanation spanning multiple lines
 * and providing technical context.
 */
```

### Style 2: C++ Triple Slash (Clean & Modern)
```cpp
/// @brief Brief summary line.
/// Detailed explanation goes here.
```

### Style 3: Inline Member Comment (`<`)
To document a variable or struct member on the same line, append `<`:
```cpp
int timeout_ms; ///< Connection timeout duration in milliseconds.
```

---

## 5. Essential Doxygen Tags & Commands

Tags begin with `@` (or `\`).

| Tag | Purpose | Example |
|-----|---------|---------|
| `@file` | Marks file documentation (mandatory for global functions/constants) | `@file math_utils.hpp` |
| `@brief` | Single-sentence high-level summary | `@brief Calculates Euclidean distance.` |
| `@param[in,out]` | Documents input/output function arguments | `@param[in] x Initial x-coordinate.` |
| `@return` / `@returns` | Documents what the function returns | `@return true on success, false on error.` |
| `@throws` / `@exception`| Lists exceptions thrown | `@throws std::invalid_argument If radius < 0.` |
| `@see` | Cross-references related functions or URLs | `@see reset_coordinates()` |
| `@note` | Adds an informative highlight box | `@note This method is thread-safe.` |
| `@warning` | Adds a prominent warning box | `@warning Do not call before initializing socket.` |
| `@deprecated` | Flags obsolete functions | `@deprecated Use execute_async() instead.` |
| `@author`, `@date` | Documents code provenance | `@author Jane Doe` |

---

## 6. Documenting Classes, Methods, & Files

> ⚠️ **IMPORTANT RULE:** For Doxygen to document global functions, constants, or typedefs not inside a class, the file **must** contain an `@file` tag at the top of the file!

---

## 7. Comprehensive C++ Code Example

Save as `include/Matrix.hpp`:

```cpp
/**
 * @file Matrix.hpp
 * @author Jane Doe (jdoe@example.com)
 * @brief A lightweight, templated mathematical matrix library.
 * @version 1.0.0
 * @date 2026-11-12
 * 
 * @copyright MIT License
 */

#ifndef MATRIX_HPP
#define MATRIX_HPP

#include <vector>
#include <stdexcept>

namespace math {

/**
 * @class Matrix
 * @brief Represents a two-dimensional numerical matrix.
 * 
 * Provides basic linear algebra routines including addition,
 * scalar multiplication, and determinant calculation.
 * 
 * @tparam T Numerical data type (e.g., float, double, int).
 */
template <typename T>
class Matrix {
private:
    size_t rows_;                       ///< Number of rows in matrix.
    size_t cols_;                       ///< Number of columns in matrix.
    std::vector<std::vector<T>> data_;  ///< Internal 2D container.

public:
    /**
     * @brief Constructs a zero-initialized matrix of given dimensions.
     * 
     * @param rows Number of rows (must be > 0).
     * @param cols Number of columns (must be > 0).
     * @throws std::invalid_argument If rows or cols is zero.
     */
    Matrix(size_t rows, size_t cols);

    /**
     * @brief Retrieves the value at the specified coordinates.
     * 
     * @param r Row index (0-based).
     * @param c Column index (0-based).
     * @return Const reference to element at (r, c).
     * @throws std::out_of_range If indices exceed dimensions.
     */
    const T& at(size_t r, size_t c) const;

    /**
     * @brief Computes the trace (sum of diagonal elements) of a square matrix.
     * 
     * @return T The computed trace value.
     * @note Only applicable to square matrices ($rows == cols$).
     * @warning Throws logic_error if matrix is non-square.
     * @see is_square()
     */
    T compute_trace() const;

    /**
     * @brief Checks if the matrix has equal row and column dimensions.
     * @return true if square, false otherwise.
     */
    bool is_square() const noexcept {
        return rows_ == cols_;
    }
};

} // namespace math

#endif // MATRIX_HPP
```

---

## 8. Comprehensive Python Code Example

Doxygen supports Python docstrings using `"""! ... """`:

```python
"""!
@file image_processor.py
@brief High-performance image filter pipeline.
@author John Doe
"""

class ImageFilter:
    """!
    @brief Applies spatial convolutions to image matrices.
    """

    def __init__(self, kernel_size: int = 3):
        """!
        @brief Initializes the filter kernel.
        @param kernel_size Dimension of odd square kernel (default: 3).
        """
        self.kernel_size = kernel_size

    def apply_blur(self, image_data: list) -> list:
        """!
        @brief Applies a Gaussian blur filter across the input pixel array.
        @param[in] image_data 2D list of pixel intensity values [0-255].
        @return New 2D list containing blurred pixel data.
        @warning Input array must not be empty.
        """
        # Processing logic here...
        return image_data
```

---

## 9. Generating and Viewing the Documentation

### Step 1: Run Doxygen
From your repository root:
```bash
doxygen Doxyfile
```

### Step 2: Open the HTML Documentation
Open the generated `index.html` file in your preferred web browser:
```bash
# Windows:
start docs/html/index.html

# Linux:
xdg-open docs/html/index.html

# macOS:
open docs/html/index.html
```

### Step 3: Compiling PDF Documentation (Optional):
If `GENERATE_LATEX = YES` was set:
```bash
cd docs/latex
make
# Produces 'refman.pdf'
```

---

## 10. Advanced Features (Graphviz Diagrams)

By integrating with **Graphviz (`dot`)**, Doxygen automatically diagrams code architectures:

In `Doxyfile`, set:
```ini
HAVE_DOT               = YES
CALL_GRAPH             = YES
CALLER_GRAPH           = YES
CLASS_GRAPH            = YES
COLLABORATION_GRAPH    = YES
UML_LOOK               = YES
```

This renders visual UML inheritance trees and call-hierarchy graphs directly inside each function and class page in the HTML output!
