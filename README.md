# Lean Builder

<p align="center">
  <!-- Consider adding a logo here -->
  <img src="https://via.placeholder.com/150?text=Lean+Builder" alt="Lean Builder Logo" width="150" height="150">
</p>

<p align="center">
  <a href="https://pub.dev/packages/lean_builder"><img src="https://img.shields.io/pub/v/lean_builder.svg" alt="Pub"></a>
  <a href="https://github.com/username/lean_builder/actions"><img src="https://github.com/username/lean_builder/workflows/tests/badge.svg" alt="Build Status"></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-purple.svg" alt="License: MIT"></a>
</p>

A streamlined Dart build system that applies lean principles to minimize waste and maximize speed.

## Table of Contents

- [Disclaimer](#disclaimer)
- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
  - [One-time Build](#one-time-build)
  - [Watch Mode](#watch-mode)
  - [Cleaning Build Cache](#cleaning-build-cache)
- [Creating Builders](#creating-builders)
  - [Using LeanGenerator](#using-leangenerator)
  - [Custom Builders](#custom-builders)
  - [Registering Runtime Types](#registering-runtime-types)
- [Advanced Usage](#advanced-usage)
- [Performance Tips](#performance-tips)
- [Contributing](#contributing)
- [License](#license)

## Disclaimer
Some core concepts and code-abstractions are borrowed from the Dart build system. However, Lean Builder is designed as a more efficient and user-friendly alternative, not a direct replacement. It prioritizes performance and simplicity while offering a streamlined approach to code generation.

## Overview

Lean Builder is a code generation system designed to be fast, efficient, and easy to use. It provides a streamlined alternative to other build systems with a focus on performance and developer experience.

## Features

- ⚡ Fast incremental builds
- 🚀 Native code compatibility (no mirrors dependency)
- 🧵 Parallel processing for maximum efficiency
- 🔄 Watch mode with hot reload support
- 📝 Simple, declarative builder configuration using annotations
- 🧩 Comprehensive asset tracking and dependency management
- 📚 Support for shared part builders, library builders, and standard builders
- 🔧 Customizable build pipelines
- 📊 Build performance analytics
- 🛠️ And much more!

## Installation

Add Lean Builder to your `pubspec.yaml` as a dependency:

```yaml
dev_dependencies:
  lean_builder: ^1.0.0
```

## Usage

### One-time Build

To generate code just once:

```bash
dart run lean_builder build
```

Options:
```bash
dart run lean_builder build [--release] [--verbose] [--dir=<directory>]
```

### Watch Mode

For continuous builds during development:

```bash
dart run lean_builder watch
```

Use the `--dev` flag for development mode with hot reload support:

```bash
dart run lean_builder watch --dev
```

Additional options:
```bash
dart run lean_builder watch [--dev] [--verbose] [--port=<number>]
```

### Cleaning Build Cache

To clean all generated files and cache:

```bash
dart run lean_builder clean
```

## Creating Builders

Lean Builder offers multiple ways to create code generators, from simple annotations to custom builders.

### Using LeanGenerator

#### Library Generator

Create a generator that outputs standalone library files:

```dart 
@LeanGenerator({'.lib.dart'})
class MyGenerator extends GeneratorForAnnotatedClass<MyAnnotation> {
  @override
  Future<String> generateForClass(BuildStep buildStep, ClassElement element, MyAnnotation annotation) async {
    return '// Generated code for ${element.name}';
  }
}
```

#### Shared Part Generator

Create a generator that outputs part files that can be included in multiple libraries:

```dart 
@LeanGenerator.shared()
class MySharedGenerator extends GeneratorForAnnotatedClass<MyAnnotation> {
  @override
  Future<String> generateForClass(BuildStep buildStep, ClassElement element, MyAnnotation annotation) async {
    return '// Generated code for ${element.name}';
  }
}
```

### Custom Builders

For more control, create a custom builder by extending the `Builder` class:

```dart
@LeanBuilder()
class MyBuilder extends Builder {
  @override
  Set<String> get outputExtensions => {'.lib.dart'};

  @override
  bool shouldBuildFor(BuildCandidate candidate) {
    return candidate.isDartSource && candidate.hasTopLevelMetadata;
  }

  @override
  FutureOr<void> build(BuildStep buildStep) async {
    final resolver = buildStep.resolver;
    final library = await resolver.resolveLibrary(buildStep.asset);
    final elements = library.annotatedWith('<type-checker>');
    // Your build logic here
    await buildStep.writeAsString('// Generated code', extension: '.lib.dart');
  }
}
```

### Registering Runtime Types
To use your runtime types with type checkers, register them with Lean Builder:

```dart
@LeanBuilder(registerTypes: {MyAnnotation}) 
   // ...
    final myAnnotationChecker = resolver.typeCheckerOf<MyAnnotation>(); 
}

**Note**: Generic types of `GeneratorForAnnotation<Type>` and its subclasses are automatically registered.

## Advanced Usage

### Build Configuration
Create a `build.yaml` file in your project root to customize build behavior:

```yaml
targets:
  $default:
    builders:
      lean_builder:my_builder:
        enabled: true
        options:
          option1: value1
          option2: value2
```

### Performance Optimization

Configure build performance settings:
@LeanBuilder(
  concurrency: 4,
  priority: BuildPriority.high,
  cacheResults: true
)
class MyOptimizedBuilder extends Builder {
  // Implementation
}
```
## Performance Tips

- Use incremental builds for faster development cycles
- Register only the types you need to minimize startup time
- Leverage shared builders when appropriate to reduce redundant code generation
- Consider setting appropriate concurrency levels based on your machine's capabilities
## Contributing
Contributions are welcome! Please feel free to submit a Pull Request.
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
## License
This project is licensed under the MIT License - see the LICENSE file for details.
      constant.get('constKey'); // Constant?;
    }

    if (constant is ConstList) {
      constant.value; // List<Constant>
      constant.literalValue; // List<dynamic>, dart values
    }

    if (constant is ConstFunctionReference) {
      constant.name; // the name of the function
      constant.type; // the type of the function
      constant.element; // the executable element of the function
    }
  }
 ```

### Using LeanBuilder directly inside of a project
