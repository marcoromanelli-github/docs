# Enhanced Virtual Environment Guide

Guides for setting up development/research environments on the cluster (Made for Linux)

## General Tips (New Section)

- Always use version control (e.g., Git) alongside virtual environments
- Document your project setup and dependencies
- Regularly update your virtual environment and dependencies
- Be consistent with naming conventions for your projects and environments

## Python

### How to create and use a virtual environment in Python

#### Setup environment

1. Ensure Python is installed:
   ```
   python3 --version
   ```
   If not installed, use your distribution's package manager to install Python 3.

2. Create directory for project:
   ```
   mkdir Project
   ```

3. Navigate to directory:
   ```
   cd Project
   ```

4. Create a virtual environment:
   ```
   python3 -m venv research1
   ```

5. Ensure the environment is created:
   ```
   ls
   ```
   It should be listed as a directory in the results of running `ls`:
   ```
   research1
   ```

6. Activate the environment:
   ```
   source research1/bin/activate
   ```
   Your current line should be prefixed with the environment name:
   ```
   (research1) user@super-computer
   ```

7. Deactivate the environment (when you're done):
   ```
   deactivate
   ```

#### Installing packages

(While virtual environment is activated)

1. Upgrade pip to the latest version:
   ```
   pip install --upgrade pip
   ```

2. List installed packages:
   ```
   pip list
   ```

3. Install new package:
   ```
   pip install package_name
   ```
   For a specific version: `pip install package_name==1.0.0`

4. List installed packages again to ensure the needed packages are installed:
   ```
   pip list
   ```

#### Saving dependencies

(While virtual environment is activated)

1. Save packages to a file:
   ```
   pip freeze > requirements.txt
   ```

2. Ensure the file was successfully created with package info:
   ```
   cat requirements.txt
   ```

3. Consider separating development and production dependencies:
   ```
   pip freeze > requirements-dev.txt
   ```

#### Using dependencies

1. Install dependencies from requirements file:
   ```
   pip install -r requirements.txt
   ```

2. For development environments:
   ```
   pip install -r requirements-dev.txt
   ```

3. List installed packages to ensure dependencies are installed:
   ```
   pip list
   ```

#### Delete virtual environment

1. Deactivate the environment if it's active:
   ```
   deactivate
   ```

2. Delete directory with virtual environment:
   ```
   rm -r research1
   ```

**Warning:**
* Do not store source code inside virtual environment directories
* All source code should be stored in main directory (project in this example)
* Always use `.gitignore` to exclude the virtual environment directory from version control


## R

### How to create and use a virtual environment in R

#### Setup environment

1. Ensure R is installed:
   ```
   R --version
   ```

2. Create directory for project:
   ```
   mkdir Project
   ```

3. Navigate to directory:
   ```
   cd /Project
   ```

4. Create an R project:
   ```R
   R
   > usethis::create_project(".")
   ```

5. Install renv for virtual environments:
   ```R
   > install.packages("renv")
   ```

6. Initialize renv:
   ```R
   > renv::init()
   ```
   This creates a project-specific library and .Rprofile

#### Activate an environment

```R
> renv::activate()
```

#### Deactivate an environment

```R
> renv::deactivate()
```

#### Installing Packages

(While virtual environment is activated)

1. Install a package:
   ```R
   > install.packages("package")
   ```

2. Record the new package in the lockfile:
   ```R
   > renv::snapshot()
   ```

#### Using dependencies

1. Save environment state to lock file:
   ```R
   > renv::snapshot()
   ```

2. Recreate another environment given the lock file:
   ```R
   > renv::restore()
   ```

3. Update all packages in the project:
   ```R
   > renv::update()
   ```

#### Delete virtual environment

1. Exit R if you're in an R session

2. Delete directory with virtual environment:
   ```
   rm -r renv
   ```

3. Remove .Rprofile and renv.lock files:
   ```
   rm .Rprofile renv.lock
   ```

## Julia

### How to create and use a virtual environment in Julia

#### Setup environment

1. Check Julia version:
   ```
   julia -version
   ```

2. Create directory for project:
   ```
   mkdir Project
   ```

3. Navigate to directory:
   ```
   cd /Project
   ```

4. Start Julia and enter package manager mode:
   ```
   julia
   julia> ]
   ```

5. Create a new environment and activate:
   ```
   (@v1.10) pkg> activate .
   ```
   This creates a new environment in the current directory.

6. Create Project.toml and Manifest.toml files:
   ```
   (@v1.10) pkg> instantiate
   ```

#### Deactivate environment

```
(@v1.10) pkg> activate
```

#### Install packages

(While virtual environment is activated)

1. Install package:
   ```
   (@v1.10) pkg> add Package
   ```

2. Press backspace to exit package manager

3. Ensure the new package is installed:
   ```julia
   julia> using Package
   ```

#### Using dependencies

1. Enter package manager:
   ```
   julia> ]
   ```

2. Install dependencies from .toml files in the project directory:
   ```
   (@v1.10) pkg> instantiate
   ```

3. Update all packages:
   ```
   (@v1.10) pkg> update
   ```

#### Delete virtual environment

Simply delete the Project.toml and Manifest.toml files from your project directory.

## Node JS

### How to create and use a virtual environment in Node JS

#### Setup environment

1. Check node version:
   ```
   node -v
   ```

2. Create directory for project:
   ```
   mkdir Project
   ```

3. Navigate to directory:
   ```
   cd /Project
   ```

4. Initialize the project:
   ```
   npm init -y
   ```

5. Create a .nvmrc file that specifies node version:
   ```
   node -v > .nvmrc
   ```

#### Install packages

1. Install a package and save it to package.json:
   ```
   npm install package --save
   ```

2. Install a development dependency:
   ```
   npm install package --save-dev
   ```

#### Using dependencies

1. Use .nvmrc for node version:
   ```
   nvm use
   ```

2. Install packages from package.json:
   ```
   npm install
   ```

3. Update all packages:
   ```
   npm update
   ```

#### Managing different Node.js versions

1. Install a specific Node.js version:
   ```
   nvm install 14.17.0
   ```

2. Switch to a specific Node.js version:
   ```
   nvm use 14.17.0
   ```

#### Delete virtual environment

In Node.js, the "virtual environment" is essentially your `node_modules` directory and `package.json` file. To remove:

1. Delete the node_modules directory:
   ```
   rm -rf node_modules
   ```

2. Optionally, remove package.json and package-lock.json:
   ```
   rm package.json package-lock.json
   ```

Remember to keep your `package.json` if you want to reinstall your dependencies later.
