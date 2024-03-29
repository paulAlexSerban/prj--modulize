# Modulize
> Project task runner for modular projects

## Project Brief

### Summary

Modulize is a bash task runner script designed to provide flexibility in managing and executing tasks for modular projects. It allows you to run specific tasks on individual modules or the entire project, making it easier to maintain and control the build process.

### Goal Details

The main goal of Modulize is to automate the execution of tasks within modular projects, allowing developers to focus on the code and functionality of their applications. By handling tasks such as build, test, and deployment, Modulize reduces the time and effort required to manage these tasks manually.

### Target Audience

Modulize is intended for developers working on modular projects, specifically those who need a lightweight and efficient solution for managing tasks within their projects.

### Promise

Modulize promises to streamline the management and execution of tasks within modular projects, offering a simple and flexible solution to improve productivity and reduce the time spent on manual task management.

## Tech Stack

Modulize is implemented using the following technologies:

- Bash scripting
- Environment variables and configuration files

## Features

- Task execution for individual modules or entire projects.
- Environment-specific configuration support.
- Automatic discovery of modules and submodules.
- Simple and intuitive command-line interface.
- Color-coded output for enhanced readability.

## Installation

### Requirements

- Bash (version 4.0 or higher)
- Access to a UNIX-based system (Linux, macOS)

### Install

- Download or clone the Modulize repository to your local machine.
- Ensure the main script (modulize.sh) is executable by running `chmod +x modulize.sh`.
- Add `.env.development` to your project root directory and add the following environment variables:

```bash
# 1/ Project modules in order of execution
INSTALL_PROJECT_MODULES=("assets" "backend" "frontend")
UNINSTALL_PROJECT_MODULES=("assets" "frontend" "backend")

# 2/ Project names -------------------------------------------------------
# Must be lower-case, no spaces and no invalid path chars.
PROJECT_NAME=project-name
COMPOSE_PROJECT_NAME=project-name
```

- Each module should have a `.env.development` file containing the environment-specific configuration for that module.
  - Example: `frontend/.env.development`

```bash
# 1/ Module name
MODULE_NAME="frontend"

# 2/ Project modules in order of execution
INSTALL_MODULE_SUBPROJECTS=("nextjs-landing-page" "react-gallery-app" "vue-flash-card-app")
UNINSTALL_MODULE_SUBPROJECTS=("nextjs-landing-page" "react-gallery-app" "vue-flash-card-app")
```

- Each module sub-module should have a `.env.development` file containing the environment-specific configuration for that sub-module and a scripts directory containing the scripts that can be run by modulize
  - Example: `frontend/nextjs-landing-page/.env.development`
```bash
# 1/ Module name
MODULE_NAME="nextjs-landing-page"
```

  -   Example: `frontend/nextjs-landing-page/.mdz/install`
```bash
#!/bin/bash
# makes sure the folder containing the script will be the root folder
cd "$(dirname "$0")" || exit
source ../.env.development
rm -rfv ../node_modules
npm --prefix .. install
```

- Simply invoke the script with the desired options to run the task on the specified modules.
```bash
bash modulize  -e dev -p install -m frontend
```

## Usage

To use Modulize, run the script with the following options:

- `-p | --pahse PHASE (mandatory)`: The task phase to run, such as "build" or "test".
- `-e | --env ENV (mandatory)`: The environment configuration to use, such as "dev" or "prod".
- `-m | --module MODULE (optional)`: The specific module to run the task on. If not specified, the task will run on all modules

## Taxonomy

```bash
MODULE_DIR
├── .env.development
├── SUBMODULE_DIR
│   ├── .env.development
│   ├── scripts
│   └── source
└── SUBMODULE_DIR
    ├── .env.development
    ├── scripts
    └── source
```

### Module `.env.development`

- if MODULE_DIR contains `.env.development`:
  - inside `.env.development` the variables keeping the submodules names must be declared
  - submodules must be declared in the order of execution
    Example:

```bash
  # 2/ Project modules in order of execution
  INSTALL_MODULE_SUBPROJECTS=("traefik" "nginx" "database" "wordpress")
  UNINSTALL_MODULE_SUBPROJECTS=("nginx" "wordpress" "database" "traefik" )
```

## Post-MVP Improvements & To Do's

### Functional Improvements:

- Support for additional task types, such as linting and code analysis.
- Parallel execution of tasks for improved performance.
- Integration with Continuous Integration (CI) and Continuous Deployment (CD) tools for seamless project management.
- Enhanced error handling and reporting capabilities for better debugging.
- Support for custom task plugins, allowing users to extend Modulize's functionality.

### Non-Functional Improvements:

- Improved documentation and examples for easier adoption.
- Performance optimizations for faster task execution.
- Enhanced modularity and code organization for better maintainability.
