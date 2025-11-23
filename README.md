# qchartist
QChartist user base

To compile QChartist Technical Analysis (TA) modules using RapidQ and MinGW, you typically use a hybrid approach where RapidQ handles the GUI and scripting logic, while MinGW compiles the C++ backend components.

Here’s a breakdown of how this process works:
🧰 Tools Involved

    RapidQ: A BASIC-like language that compiles to bytecode and runs via an interpreter. It’s used for scripting and GUI in QChartist.

    RQPC (RapidQ Pre Compiler): A tool developed by the QChartist team to allow integration of C++ code within RapidQ programs.

    MinGW (Minimalist GNU for Windows): A Windows port of GCC used to compile the C++ parts of QChartist.

🛠️ Compilation Workflow

    RapidQ Setup:

        Ensure you have the following files from the RapidQ distribution:

            RAPIDQ.EXE (IDE)

            RC.EXE (RapidQ compiler)

            RAPIDQ32.LIB and RAPIDQCC.LIB (required libraries)

    Using RQPC:

    QChartist uses a custom pre-compiler called RQPC to embed and compile C++ code within RapidQ scripts.

    RQPC invokes the MinGW G++ compiler to compile the embedded C++ code.

    Unfortunately, the source code for RQPC is not publicly available, which limits customization

    Compiling with MinGW:

    MinGW is used to compile the C++ backend of QChartist, including custom indicators and performance-critical components

    If you need to link additional libraries (e.g., OpenGL), you must pass flags like -lGL to G++. However, RQPC currently lacks a built-in way to add custom flags unless modified

    Linking and Execution:

    The compiled C++ code is linked with the RapidQ bytecode/interpreter.

    The final executable combines GUI (from RapidQ) and backend logic (from C++ via MinGW).

    Limitations and Considerations

    Customization: Since RQPC is not open-source, adding new compiler flags or modifying its behavior is not straightforward.

    Library Linking: If you need to link new C++ libraries, you may need to manually compile the C++ code with MinGW and interface it with RapidQ via DLLs or external calls.

    To compile QChartist as a 64-bit application, you must bypass RapidQ entirely and rebuild the software using C++ and Qt in a 64-bit development environment. The original RapidQ-based architecture is inherently 32-bit and cannot be upgraded directly.

Here’s how to approach a 64-bit build of QChartist:

Why RapidQ Can’t Be Used for 64-bit

    RapidQ (RC.EXE) is a 32-bit compiler/interpreter and hasn’t been updated for 64-bit compatibility.

    RQPC, the precompiler that integrates C++ into RapidQ, is also 32-bit only and hardcoded for 32-bit MinGW.

    Therefore, any 64-bit build must abandon RapidQ and be rewritten in a modern 64-bit compatible language and toolchain.

    🛠️ Path to a 64-bit QChartist Build
✅ 1. Use Qt Creator with C++

The QChartist developer has confirmed plans to rewrite the application in pure C/C++ using Qt Creator, which supports 64-bit builds and cross-platform deployment

✅ 2. Set Up a 64-bit Qt Development Environment

    Install Qt Creator from https://www.qt.io/download

    During installation, select:

        MinGW 64-bit or MSVC 64-bit toolchain

        Qt libraries for 64-bit Windows

    Configure your Kits in Qt Creator:

        Go to Tools > Options > Kits

        Ensure a 64-bit compiler and Qt version are selected

        ✅ 3. Port RapidQ Logic to C++

    Since RapidQ is no longer usable, you’ll need to rewrite the GUI and logic in C++/Qt.

    Focus on:

        Replacing RapidQ GUI components with Qt Widgets

        Porting TA indicators and scripting logic to C++ classes or QtScript/QJSEngine

        ✅ 4. Compile and Test

    Use Qt Creator to build the project as a 64-bit executable.

    Test for compatibility with existing data formats and plugins.

    🧩 Optional: Plugin System for TA Modules

If you want to preserve modularity:

    Design a plugin architecture using Qt’s QPluginLoader and QInterface.

    Each TA module can be a separate .dll or .so compiled in 64-bit C++.

    🧪 Experimental: Hybrid Approach

If you want to keep using some RapidQ logic:

    Run the 32-bit RapidQ logic as a separate process

    Communicate with the 64-bit C++ frontend via:

        Named pipes

        Sockets

        Temporary files

This is complex and not recommended unless you have strong reasons to preserve legacy code.

You're absolutely right — porting QChartist to a full 64-bit architecture without RapidQ is a massive undertaking. It’s not just a recompile; it’s a fundamental rewrite of the application’s architecture, especially since RapidQ is deeply embedded in the GUI and scripting logic.

Here’s a realistic view of what makes it so challenging:

🧩 Why It’s So Complex

    Legacy Codebase: RapidQ is outdated, 32-bit only, and not maintained. Replacing it means rewriting every GUI component and logic handler.

    Custom Precompiler (RQPC): It’s a black box. Without source code, it’s impossible to adapt it for 64-bit or modern compilers.

    Tight Coupling: The TA modules, GUI, and scripting are tightly integrated, which makes modular replacement difficult.

    Lack of Documentation: Much of the logic is undocumented or sparsely commented, requiring reverse engineering.

    🛠️ What It Would Take

    1–2 developers working part-time could take 1–2 years to fully port QChartist to 64-bit C++/Qt.

    A team of 3–5 experienced developers could potentially do it in 6–9 months, depending on scope and testing.

    You’d need to:

        Rebuild the GUI in Qt Widgets or QML

        Port TA logic to C++ classes or a plugin system

        Replace scripting with QtScript or Python (via PySide or embedded interpreter)

        Rebuild the charting engine using Qt Graphics View or OpenGL

        💡 A More Manageable Path

If you're passionate about this but working solo or in a small team, consider this phased approach:

    Phase 1: Extract and Rebuild Core Charting Engine in C++/Qt

    Phase 2: Re-implement GUI in Qt Widgets

    Phase 3: Port or Replace TA Modules

    Phase 4: Add Plugin or Scripting Support (e.g., Python or Lua)

    Phase 5: Migrate to 64-bit and cross-platform deployment

    🤝 Community Collaboration

You’re not alone in this. If you’re serious about the project:

    Start a GitHub fork and document the migration process.

    Invite contributors from the QChartist forum or open-source community.

    Even partial progress (e.g., a 64-bit charting engine) could benefit others.
