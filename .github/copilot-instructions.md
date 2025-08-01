# GitHub Copilot Instructions

These instructions define how GitHub Copilot should assist with this project. The goal is to ensure consistent, high-quality code generation aligned with our conventions, stack, and best practices.

## Context

- **Project Type**: Graphics Library / DirectX / Direct3D 12 / Game Audio
- **Project Name**: DirectX Tool Kit for DirectX 12
- **Language**: C++
- **Framework / Libraries**: STL / CMake / CTest
- **Architecture**: Modular / RAII / OOP

## Getting Started

- See the tutorial at [Getting Started](https://github.com/microsoft/DirectXTK12/wiki/Getting-Started).
- The recommended way to integrate *DirectX Tool Kit for DirectX 12* into your project is by using the *vcpkg* Package Manager. See [d3d12game_vcpkg](https://github.com/walbourn/directx-vs-templates/tree/main/d3d12game_vcpkg) for a template which uses VCPKG.
- You can make use of the nuget.org packages **directxtk12_desktop_2019**, **directxtk12_desktop_win10**, or **directxtk12_uwp**.
- You can also use the library source code directly in your project or as a git submodule.

> If you are new to DirectX, you may want to start with [DirectX Tool Kit for DirectX 11](https://github.com/microsoft/DirectXTK/wiki/Getting-Started) to learn many important concepts for Direct3D programming, HLSL shaders, and the code patterns used in this project with a more 'noobie friendly' API.

## General Guidelines

- **Code Style**: The project uses an .editorconfig file to enforce coding standards. Follow the rules defined in `.editorconfig` for indentation, line endings, and other formatting. Additional information can be found on the wiki at [Implementation](https://github.com/microsoft/DirectXTK12/wiki/Implementation). The code requires C++11/C++14 features.
- **Documentation**: The project provides documentation in the form of wiki pages available at [Documentation](https://github.com/microsoft/DirectXTK12/wiki/). The audio, input, and math implementations are identical to the DirectX Tool Kit for DirectX 11.
- **Error Handling**: Use C++ exceptions for error handling and uses RAII smart pointers to ensure resources are properly managed. For some functions that return HRESULT error codes, they are marked `noexcept`, use `std::nothrow` for memory allocation, and should not throw exceptions.
- **Testing**: Unit tests for this project are implemented in this repository [Test Suite](https://github.com/walbourn/directxtk12test/) and can be run using CTest per the instructions at [Test Documentation](https://github.com/walbourn/directxtk12test/wiki).
- **Security**: This project uses secure coding practices from the Microsoft Secure Coding Guidelines, and is subject to the `SECURITY.md` file in the root of the repository. Functions that read input from image file, geometry files, and audio files are subject to OneFuzz testing to ensure they are secure against malformed files.
- **Dependencies**: The project uses CMake and VCPKG for managing dependencies, making optional use of DirectXMath, DirectX-Headers, DirectX 12 Agility SDK, GameInput, and XAudio2Redist. The project can be built without these dependencies, relying on the Windows SDK for core functionality.
- **Continuous Integration**: This project implements GitHub Actions for continuous integration, ensuring that all code changes are tested and validated before merging. This includes building the project for a number of configurations and toolsets, running a subset of unit tests, and static code analysis including GitHub super-linter, CodeQL, and MSVC Code Analysis.
- **Code of Conduct**: The project adheres to the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). All contributors are expected to follow this code of conduct in all interactions related to the project.

## File Structure

```txt
.azuredevops/ # Azure DevOps pipeline configuration and policy files.
.github/      # GitHub Actions workflow files and linter configuration files.
.nuget/       # NuGet package configuration files.
build/        # Miscellaneous build files and scripts.
Audio/        # DirectX Tool Kit for Audio implementation files.
Inc/          # Public header files.
Src/          # Implementation header and source files.
  Shaders/    # HLSL shader files.
Tests/        # Tests are designed to be cloned from a separate repository at this location.
```

> Note that *DirectX Tool Kit for DirectX 12* utilizes the `MakeSpriteFont` C# tool and the `XWBTool` C++ Audio tool hosted in the *DirectX Tool Kit for DirectX 11* repository. See [MakeSpriteFont](https://github.com/microsoft/DirectXTK/tree/main/MakeSpriteFont) and [XWBTool](https://github.com/microsoft/DirectXTK/tree/main/XWBTool).

## Patterns

### Patterns to Follow

- Use RAII for all resource ownership (memory, file handles, etc.).
- Many classes utilize the pImpl idiom to hide implementation details, and to enable optimized memory alignment in the implementation.
- Use `std::unique_ptr` for exclusive ownership and `std::shared_ptr` for shared ownership.
- Use `Microsoft::WRL::ComPtr` for COM object management.
- Make use of anonymous namespaces to limit scope of functions and variables.
- Make use of `assert` for debugging checks, but be sure to validate input parameters in release builds.
- Make use of the `DebugTrace` helper to log diagnostic messages, particularly at the point of throwing an exception.

### Patterns to Avoid

- Don’t use raw pointers for ownership.
- Avoid macros for constants—prefer `constexpr` or `inline` `const`.
- Don’t put implementation logic in header files unless using templates, although the SimpleMath library does use an .inl file for performance.
- Avoid using `using namespace` in header files to prevent polluting the global namespace.

## References

- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [Microsoft Secure Coding Guidelines](https://learn.microsoft.com/en-us/security/develop/secure-coding-guidelines)
- [CMake Documentation](https://cmake.org/documentation/)
- [VCPK Documentation](https://learn.microsoft.com/vcpkg/)
- [DirectX Tool Kit for DirectX 12 Wiki](https://github.com/microsoft/DirectXTK12/wiki/)
- [DirectX Tool Kit for Audio Wiki](https://github.com/Microsoft/DirectXTK/wiki/Audio)
- [Games for Windows and the DirectX SDK blog - December 2013](https://walbourn.github.io/directx-tool-kit-for-audio/)
- [Games for Windows and the DirectX SDK blog - July 2016](https://walbourn.github.io/directx-tool-kit-for-directx-12/)
- [Games for Windows and the DirectX SDK blog - May 2020](https://walbourn.github.io/directx-tool-kit-for-audio-updates-and-a-direct3d-9-footnote/)
- [Games for Windows and the DirectX SDK blog - September 2021](https://walbourn.github.io/latest-news-on-directx-tool-kit/)
- [Games for Windows and the DirectX SDK blog - October 2021](https://walbourn.github.io/directx-tool-kit-vertex-skinning-update/)

## No speculation

When creating documentation:

### Document Only What Exists

- Only document features, patterns, and decisions that are explicitly present in the source code.
- Only include configurations and requirements that are clearly specified.
- Do not make assumptions about implementation details.

### Handle Missing Information

- Ask the user questions to gather missing information.
- Document gaps in current implementation or specifications.
- List open questions that need to be addressed.

### Source Material

- Always cite the specific source file and line numbers for documented features.
- Link directly to relevant source code when possible.
- Indicate when information comes from requirements vs. implementation.

### Verification Process

- Review each documented item against source code whenever related to the task.
- Remove any speculative content.
- Ensure all documentation is verifiable against the current state of the codebase.
