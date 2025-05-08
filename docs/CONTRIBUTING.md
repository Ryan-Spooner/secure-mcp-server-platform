# Contributing to Secure Model Context Protocol (SMCP)

First off, thank you for considering contributing to SMCP! We're excited to build this project with the community and appreciate your help in making it a valuable resource for understanding and utilizing the Model Context Protocol (MCP).

This document provides guidelines for contributing to SMCP. Whether you're fixing a bug, proposing a new feature, improving documentation, or adding to our educational examples, your contributions are welcome!

## Table of Contents

*   [Code of Conduct](#code-of-conduct)
*   [How Can I Contribute?](#how-can-i-contribute)
    *   [Reporting Bugs](#reporting-bugs)
    *   [Suggesting Enhancements](#suggesting-enhancements)
    *   [Contributing Code or Documentation](#contributing-code-or-documentation)
    *   [Adding to the MCP Server Catalog](#adding-to-the-mcp-server-catalog)
    *   [Contributing to Educational Content](#contributing-to-educational-content)
*   [Getting Started](#getting-started)
    *   [Fork the Repository](#fork-the-repository)
    *   [Clone Your Fork](#clone-your-fork)
    *   [Set Upstream Remote](#set-upstream-remote)
    *   [Create a Branch](#create-a-branch)
*   [Making Changes](#making-changes)
    *   [Commit Messages](#commit-messages)
    *   [Testing](#testing)
    *   [Submitting a Pull Request (PR)](#submitting-a-pull-request-pr)
*   [Development Environment Setup](#development-environment-setup) (To be expanded)
*   [Coding Style Guidelines](#coding-style-guidelines) (To be expanded)
*   [Documentation Guidelines](#documentation-guidelines)
*   [Community and Communication](#community-and-communication)

## Code of Conduct

This project and everyone participating in it is governed by the [SMCP Code of Conduct](CODE_OF_CONDUCT.md)

## How Can I Contribute?

There are many ways to contribute to SMCP:

### Reporting Bugs

If you encounter a bug in the SMCP platform, examples, or documentation:

1.  **Search existing issues:** Check if the bug has already been reported.
2.  If not, **open a new issue**.
3.  Provide a clear and descriptive title.
4.  Include steps to reproduce the bug, expected behavior, and actual behavior.
5.  Mention your environment (OS, browser, relevant software versions).
6.  Include screenshots or code snippets if helpful.

### Suggesting Enhancements

If you have an idea for a new feature, an improvement to an existing one, or a new educational topic:

1.  **Search existing issues and discussions:** See if your idea has already been proposed.
2.  If not, **open a new issue** (for well-defined enhancements) or start a **GitHub Discussion** (for broader ideas).
3.  Clearly describe the proposed enhancement and its benefits to SMCP users.
4.  Provide use cases or examples.

### Contributing Code or Documentation

This includes:
*   Fixing bugs in the codebase.
*   Implementing new features for the SMCP platform.
*   Adding or improving educational examples (e.g., the Universal MCP example, project templates).
*   Writing or refining documentation, tutorials, or guides.

Please follow the [Getting Started](#getting-started) and [Making Changes](#making-changes) sections below.

### Adding to the MCP Server Catalog

As the MCP Server Catalog develops, we'll establish a process for community submissions. This might involve:
*   Submitting details via a form on the SMCP platform.
*   Making Pull Requests to a data file (e.g., JSON or YAML) that populates the catalog.
*   (Details for this will be provided as the feature is developed.)

### Contributing to Educational Content

Our educational resources are key to SMCP's mission. You can help by:
*   Improving existing tutorials or examples.
*   Proposing and/or writing new tutorials (e.g., "Advanced MCP Server Security," "Integrating MCP with X Tool").
*   Creating new example projects or client/server implementations.

## Getting Started

To contribute code or documentation, you'll need to set up your local environment:

1.  **Ensure you have a GitHub account.**
2.  **Install Git** on your local machine.
3.  **(Optional but Recommended) Install relevant development tools:**
    *   For Python development: Python (latest stable version or as specified by the project), `pip`, `venv`.
    *   For frontend development (if applicable): Node.js, npm/yarn.
    *   Docker (for working with containerized examples or the platform).

### Fork the Repository

Click the "Fork" button at the top right of the [SMCP GitHub repository page](https://github.com/Ryan-Spooner/secure-mcp-server-platform) to create a copy of the repository under your own GitHub account.

### Clone Your Fork

Clone your forked repository to your local machine:
```bash
git clone https://github.com/YOUR_USERNAME/smcp.git
cd smcp
```

### Set Upstream Remote

Add the original SMCP repository as the "upstream" remote. This allows you to keep your fork synchronized with the main project.
```bash
git remote add upstream https://github.com/Ryan-Spooner/secure-mcp-server-platform.git
git fetch upstream
```

### Create a Branch

Create a new branch for your changes. Choose a descriptive branch name (e.g., `fix/login-bug`, `feature/new-tutorial-module`, `docs/update-readme`).
```bash
git checkout -b your-branch-name main  # Or from upstream/main if your main is outdated
# To base off the latest from upstream:
# git fetch upstream
# git checkout -b your-branch-name upstream/main
```

## Making Changes

1.  Make your desired code or documentation changes in your local branch.
2.  Ensure your changes adhere to any [Coding Style Guidelines](#coding-style-guidelines) and [Documentation Guidelines](#documentation-guidelines).
3.  Test your changes thoroughly.

### Commit Messages

*   Write clear, concise, and descriptive commit messages.
*   Use the present tense (e.g., "Add feature" not "Added feature").
*   Consider using [Conventional Commits](https://www.conventionalcommits.org/) for more structured messages, especially if the project adopts this standard.
    *   Example: `feat: Add user authentication module`
    *   Example: `fix: Correct calculation error in XYZ function`
    *   Example: `docs: Update installation instructions`

### Testing

*   **Local Testing:** Run any relevant tests locally to ensure your changes don't break existing functionality. If you're adding new features, please include tests.
*   **(Future) Automated Tests:** The project aims to implement automated tests (unit, integration). Ensure your changes pass these tests before submitting a PR.

### Submitting a Pull Request (PR)

1.  Push your changes to your forked repository on GitHub:
    ```bash
    git push origin your-branch-name
    ```
2.  Go to the original SMCP repository on GitHub. You should see a prompt to create a Pull Request from your recently pushed branch.
3.  Click "Compare & pull request."
4.  **Base Repository/Branch:** Ensure it's targeting the correct branch in the upstream SMCP repository (usually `main` or a `develop` branch).
5.  **Title:** Write a clear and descriptive title for your PR.
6.  **Description:**
    *   Provide a detailed summary of the changes.
    *   Explain the "why" behind your changes.
    *   Link to any relevant GitHub issues (e.g., "Closes #123", "Fixes #456").
    *   If your PR is a work in progress, consider creating a "Draft Pull Request."
7.  Submit the Pull Request.
8.  Be responsive to any feedback or review comments from maintainers. You may need to make further commits to your branch to address these.

## Development Environment Setup

*(This section will be expanded with specific instructions for setting up the development environment for different parts of SMCP as the project matures. For now, ensure you have the basic tools mentioned in [Getting Started](#getting-started).)*

## Coding Style Guidelines

*(This section will detail any specific coding style guidelines, linters (e.g., Black for Python, Prettier for JS/MD), or formatters used by the project. For now, please try to match the style of existing code.)*

*   Write clean, readable, and maintainable code.
*   Comment your code where necessary, especially for complex logic.

## Documentation Guidelines

*   Documentation is crucial for SMCP!
*   Write in clear, concise English.
*   Use Markdown for documentation files.
*   For tutorials and examples, provide step-by-step instructions and explain concepts clearly.
*   Ensure any code snippets are accurate and runnable.

## Community and Communication

*   **GitHub Issues:** For bug reports and specific feature requests.
*   **GitHub Discussions:** For broader ideas, questions, and community discussions.
*   **Dynamous AI Community Discord:** Join the [Dynamous Discord server](https://discord.gg/YOUR_DYNAMOUS_INVITE_LINK) (replace with actual link) for real-time discussions and to connect with the wider community. Look for SMCP-related channels.

Thank you for contributing to SMCP! Your efforts help make this project better for everyone.
