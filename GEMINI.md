# Docusaurus Project Overview

This document provides a summary of the Docusaurus project, how to build and run it, and the development conventions used.

## Project Overview

Docusaurus is a static site generator that helps you build, deploy, and maintain open source project websites easily. It is built with Node.js and uses React for the view layer. The project is structured as a monorepo using Lerna and Yarn workspaces to manage multiple packages.

The main packages are located in the `packages/` directory. The `website/` directory contains the source code for the Docusaurus documentation website itself.

## Building and Running

### Prerequisites

- [Node.js](https://nodejs.org/)
- [Yarn](https://yarnpkg.com/)

### Installation

1.  Clone the repository.
2.  Run `yarn install` in the root directory to install all dependencies and build the local packages.

### Key Commands

The following commands are available in the root `package.json`:

- **`yarn start`**: Builds all packages and starts the development server for the documentation website.
- **`yarn build`**: Builds all packages and the documentation website for production.
- **`yarn test`**: Runs the Jest test suite.
- **`yarn lint`**: Lints the JavaScript and CSS files.
- **`yarn format`**: Formats the code using Prettier.

To run the documentation website in development mode:

```bash
yarn workspace website start
```

## Development Conventions

### Code Style

- The project uses [Prettier](https://prettier.io/) for code formatting and [ESLint](https://eslint.org/) for linting.
- The coding style should match the existing code in the project.
- Commit messages should follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification.

### Testing

- The project uses [Jest](https://jestjs.io/) for unit testing.
- Tests are located in `__tests__` directories within each package.
- End-to-end tests are also used to test the functionality of the generated websites.

### Contributing

- Contributions are welcome. Please read the [CONTRIBUTING.md](https://github.com/facebook/docusaurus/blob/main/CONTRIBUTING.md) file for detailed instructions.
- Issues and pull requests are managed on GitHub.
- A Contributor License Agreement (CLA) is required for all contributions.
