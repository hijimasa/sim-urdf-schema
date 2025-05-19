# sim-urdf-schema

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
English | [日本語](README-ja.md)
---

## Overview

`sim-urdf-schema` is a common specification repository that extends URDF for robot simulation (ROS 2/Gazebo, Unity, Isaac Sim, etc.).
* Adds `<stiffness>`/`<damping>` to standard URDF to control joint stiffness
* Describes `<sensor>` definitions under `<simulation>` for automatic sensor generation
* Enables consistent analysis and rendering across multiple platforms

Please refer to the following links for detailed specifications.

* Specification Reference: [spec/schema.md](spec/schema.md)
* XML Schema: [spec/urdf-extended.xsd](spec/urdf-extended.xsd)

## Features

* **Extended tag specification**

  * Describe `<stiffness>`/`<damping>` within `<joint>`
  * Describe `<sensor>` information within `<simulation>`

* **XML Schema (XSD)**

  * Schema validation with `spec/urdf-extended.xsd`

* **Xacro macros**

  * Can be concisely described in `macros/extended.urdf.xacro`

* **Platform-specific importer samples**

  * ROS 2 node, Unity URDF Importer extension, Isaac Sim script

* **Documentation & CI**

  * Official documentation published on GitHub Pages
  * Build and test automation with GitHub Actions

## Directory structure

```
📦 sim-urdf-schema
┣ 📂 spec
┃ ┣ 📄 schema.md            # List of extended tags and units
┃ ┗ 📄 urdf-extended.xsd    # XML Schema definition
┣ 📂 macros
 ┃ ┗ 📄 extended.urdf.xacro  # Xacro macro collection
┣ 📂 examples
┃ ┗ 📄 diffbot.urdf
┣ 📄 .github/workflows/ci.yml
┗ 📄 README.md
```

## Validation with XSD

```bash
xmllint --noout --schema spec/urdf-extended.xsd examples/robot_stiffness.urdf
```

## Using in Unity

T.B.D.

## Using in Isaac Sim

T.B.D.

## Contributions

* Issues and pull requests are welcome!
* For details, see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache License 2.0
