# CONTRIBUTING.md

# Contributing to chatpm

Thank you for considering contributing to **chatpm**! Your contributions are valuable and help make this project better for everyone.

## Table of Contents

- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Features](#suggesting-features)
  - [Submitting Pull Requests](#submitting-pull-requests)
- [Development Guidelines](#development-guidelines)
  - [Code Style](#code-style)
  - [Commit Messages](#commit-messages)
  - [Testing](#testing)
- [Community](#community)
- [License](#license)

---

## Getting Started

- Ensure you have read the [README.md](README.md) to understand the project and its requirements.
- Familiarize yourself with the [Code of Conduct](CODE_OF_CONDUCT.md).

## How to Contribute

### Reporting Bugs

If you find a bug, please create an issue and include:

- A clear and descriptive title.
- Steps to reproduce the issue.
- Expected and actual results.
- Screenshots or logs, if applicable.
- Any additional information that might be helpful.

[Submit a Bug Report](https://github.com/cheuqar/chatpm/issues/new?template=bug_report.md)

### Suggesting Features

We welcome new ideas! To suggest a feature:

- Check if the feature has already been suggested or implemented.
- Create an issue with:
  - A clear and descriptive title.
  - A detailed description of the proposed feature.
  - Any relevant use cases or examples.

[Suggest a Feature](https://github.com/cheuqar/chatpm/issues/new?template=feature_request.md)

### Submitting Pull Requests

We appreciate your contributions! To submit a pull request:

1. **Fork the Repository**

   - Click the "Fork" button at the top of the repository page.

2. **Clone Your Fork**

   ~~~bash
   git clone https://github.com/cheuqar/chatpm.git
   cd chatpm
   ~~~

3. **Create a Branch**

   - Use a descriptive name for your branch.

   ~~~bash
   git checkout -b feature/your-feature-name
   ~~~

4. **Make Changes**

   - Follow the [Development Guidelines](#development-guidelines).
   - Ensure your code is well-documented.

5. **Commit Changes**

   ~~~bash
   git add .
   git commit -m "Brief description of your changes"
   ~~~

6. **Push to Your Fork**

   ~~~bash
   git push origin feature/your-feature-name
   ~~~

7. **Create a Pull Request**

   - Go to the original repository and click "New Pull Request".
   - Select your branch and provide a clear description of your changes.

## Development Guidelines

### Code Style

- **Python Backend:**
  - Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) style guidelines.
  - Use meaningful variable and function names.
  - Write docstrings for modules, classes, and functions.

- **Vue.js Frontend:**
  - Follow the official [Vue Style Guide](https://vuejs.org/v2/style-guide/).
  - Use single-file components (`.vue` files).
  - Organize components logically.

### Commit Messages

- Write clear and concise commit messages.
- Use the present tense (e.g., "Add feature" not "Added feature").
- Reference issues when applicable (e.g., "Fix #123").

### Testing

- Write tests for new features and bug fixes.
- Ensure all tests pass before submitting a pull request.
- Use existing test frameworks and follow the project's testing conventions.

## Community

- Be respectful and considerate in all interactions.
- Follow the [Code of Conduct](CODE_OF_CONDUCT.MD).
- If you have questions, feel free to open an issue or start a discussion.

## License

By contributing to **chatpm**, you agree that your contributions will be licensed under the [MIT License](LICENSE).

