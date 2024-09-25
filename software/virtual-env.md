# Virtual Environment Guide

Managing software dependencies and configurations can be challenging in an HPC environment. Users often need different versions of the same software or libraries, leading to conflicts and complex setups. Virtual environments provide a solution by allowing users to isolate their project and its dependencies. This simplifies the setup process, ensures that users have the correct software environment for their applications, and reduces conflicts and errors caused by incompatible software versions. This guide provides different methods for installing or simulating virtual environments in multiple languages including Python, R, Julia, Rust, C, C++, and other languages. This will allow you to create projects in isolated environments that will not require root or sudo access.

## Python

### How to create and use a virtual environment in Python

There are several different ways you can create a virtual environment and install packages for Python. The `venv` module that comes with Python 3 is a lightweight tool that provides a standard way to create virtual environments. It is suitable for simple projects with minimal external dependencies. `virtualenv` is another tool that serves a similar purpose, but works with both Python 2 and 3. In either case, you would use `pip` to manage and install the Python packages in the virtual environments created with `venv` or `virtualenv`. Another way that is often recommended is using `conda`, the package manager and environment manager tool provided by Anaconda. Anaconda is better suited for projects that require data science libraries and have more complex dependencies. All these tools allow you to manage virtual environments for Python and packages within that environment. Below are directions for using venv to illustrate the concept. The steps may vary with other tools.

#### Setup environment
Create a new Python virtual environment using venv.

1. Ensure Python is installed:
   ```
   python3 --version
   ```
   If not installed, use your distribution's package manager to install Python 3.

2. Create a new directory for your project:
   ```
   mkdir Project
   ```

3. Navigate to the new directory:
   ```
   cd Project
   ```

4. Create a virtual environment:
   ```
   python3 -m venv research1
   ```

5. Check if the environment was created:
   ```
   ls
   ```
   It should be listed as a directory in the results of running `ls`:
   ```
   research1
   ```

6. Activate the environment for use:
   ```
   source research1/bin/activate
   ```
   Your current line should be prefixed with the environment name:
   ```
   (research1) user@super-computer
   ```

#### Deactivate environment
Deactivate environment when finished:

```
deactivate
```

#### Installing packages
Install packages specific to your project.

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
Saving your project's dependencies allows you to easily recreate your environment on another machine or share it with others.

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
Install dependencies from a requirements file, when setting up a project on a new machine or collaborating with others.

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

#### Running Python scripts
Run a script within the virtual environment:

1. Ensure the virtual environment is activated
2. Run the script:
   ```
   python your_script.py
   ```


#### Delete virtual environment
Deleting the virtual environment removes all installed packages and the environment itself.

1. Deactivate the environment if it's active:
   ```
   deactivate
   ```

2. Delete directory with virtual environment:
   ```
   rm -rf research1
   ```

## R

### How to create and use a virtual environment in R

Manage virtual environments for R and packages within that environment. R uses the 'renv' package to create project-specific libraries.

#### Setup environment
Create a new R project and initialize it with renv

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
   cd Project
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
Activating an environment in R loads the project-specific library, ensuring that your code uses the correct versions of packages for your project.

```R
> renv::activate()
```

#### Deactivate an environment
Deactivating an environment returns you to the global R library, which is useful when switching between projects.

```R
> renv::deactivate()
```

#### Installing Packages
Installing packages in an renv environment adds them to your project-specific library.

1. Install a package:
   ```R
   > install.packages("package_name")
   ```

2. Record the new package in the lockfile:
   ```R
   > renv::snapshot()
   ```

#### Using dependencies
Save the state of your environment for reuse.

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

#### Running R scripts
Run an R script within the project environment:

1. Ensure you're in the project directory
2. Run the script:
   ```
   Rscript your_script.R
   ```


#### Sharing environments
To share your project environment:

1. Include both renv.lock and .Rprofile in your version control system
2. Others can recreate your exact environment using:
   ```R
   > renv::restore()
   ```

#### Delete virtual environment
Remove the renv directory and associated files. This deletes the environment and its packages.

1. Exit R if you're in an R session

2. Delete directory with virtual environment:
   ```
   rm -rf renv
   ```

3. Remove .Rprofile and renv.lock files:
   ```
   rm .Rprofile renv.lock
   ```

## Julia

### How to create and use a virtual environment in Julia

Manage project-specific environments in Julia. Julia's built-in package manager provides functionality similar to virtual environments.

#### Setup environment
Create a new project directory and activate it as a Julia environment. 

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
   cd Project
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

#### Activate environment
Activate an existing environment:

```
julia> ]
(@v1.10) pkg> activate /path/to/your/project
```

#### Deactivate environment
Deactivating an environment in Julia returns you to the default (global) environment.

```
(@v1.10) pkg> activate
```

#### Install packages
Installing packages in a Julia environment adds them to your project-specific manifest, keeping your project dependencies isolated.

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
Install dependencies from your project files, ensuring consistent version usage.

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

#### Running Julia scripts
Run a Julia script from within the project environment:

1. Activate the environment in Julia REPL
2. Run the script:
   ```julia
   julia> include("your_script.jl")
   ```

Or from the command line:

```
julia --project=. your_script.jl
```

#### Managing multiple environments
Julia allows you to have multiple environments for different purposes:

1. Create a test environment:
   ```
   (@v1.10) pkg> activate --temp
   ```

2. Switch between environments:
   ```
   (@v1.10) pkg> activate /path/to/environment
   ```

#### Delete virtual environment
Removing the project-specific files deletes environment and its package information.

Delete the Project.toml and Manifest.toml files from your project directory.

#### Sharing environments
To share your project environment:

1. Include both Project.toml and Manifest.toml in your version control system
2. Others can recreate your exact environment using:
   ```
   (@v1.10) pkg> instantiate
   ```

## Node JS

### How to create and use a virtual environment in Node JS

Manage project-specific environments in Node.js. While Node.js doesn't have traditional virtual environments, it uses npm (Node Package Manager) to manage dependencies in individual projects. 

#### Setup environment
Create a new directory and initialize it with npm. This creates a package.json file to manage your project's dependencies.

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
   cd Project
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
Installing packages in a Node.js project adds them to your package.json file and the node_modules directory.

1. Install a package and save it to package.json:
   ```
   npm install package --save
   ```

2. Install a development dependency:
   ```
   npm install package --save-dev
   ```

#### Using dependencies
Use a specific Node.js version for your project and install or update all dependencies listed in your package.json file.

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
Node Version Manager (nvm) allows you to install and use different versions of Node.js for different projects.

1. Install a specific Node.js version:
   ```
   nvm install 14.17.0
   ```

2. Switch to a specific Node.js version:
   ```
   nvm use 14.17.0
   ```

#### Running your Node.js application
To run your Node.js application:

```
node your-app.js
```

Or, if you've defined a start script in your package.json:

```
npm start
```

#### Creating and using an .npmrc file
An .npmrc file can be used to configure npm behavior for your project:

1. Create an .npmrc file in your project root:
   ```
   touch .npmrc
   ```

2. Add configuration to the file, for example:
   ```
   save-exact=true
   ```
   This will save exact versions of packages instead of using npm's default semantic versioning range operator.

#### Delete virtual environment
Remove the "virtual environment" by deleting the node_modules directory and optionally the package.json file. This removes all installed packages and dependency information.

1. Delete the node_modules directory:
   ```
   rm -rf node_modules
   ```

2. Optionally, remove package.json and package-lock.json:
   ```
   rm package.json package-lock.json
   ```

You can keep the `package.json` if you want to reinstall your dependencies later.


# C and C++ Development Guide Using Home Directory

## C

### How to use the home directory for development with C

Setting up a local development environment for C without requiring root access. Configure your home directory to install and use libraries, as well as how to structure and compile C projects.

#### Setup environment
Create directories for binaries, libraries, and include files, and setting up environment variables to use these directories.

1. Verify installation:
   ```
   gcc --version
   make --version
   ```

2. Make directory for local installations:
   ```
   mkdir -p ~/local/{bin,lib,include}
   ```

3. Add the following to your `~/.bashrc` or `~/.bash_profile`:
   ```
   export PATH=$HOME/local/bin:$PATH
   export LD_LIBRARY_PATH=$HOME/local/lib:$LD_LIBRARY_PATH
   export CPATH=$HOME/local/include:$CPATH
   export PKG_CONFIG_PATH=$HOME/local/lib/pkgconfig:$PKG_CONFIG_PATH
   ```

4. Reload shell configuration:
   ```
   source ~/.bashrc  # or source ~/.bash_profile
   ```

#### Install packages
Installing packages (libraries) for C development in your home directory by downloading source code, compiling it, and installing it in your local directories.

Install a library with example libcurl:
```
wget https://curl.se/download/curl-7.78.0.tar.gz
tar xzf curl-7.78.0.tar.gz
cd curl-7.78.0
./configure --prefix=$HOME/local
make
make install
```

#### Create project
Create directory structure, source files, and a Makefile for easy compilation.

1. Set up project structure:
   ```
   mkdir my_c_project
   cd my_c_project
   mkdir src
   ```

2. Create file that uses the library:
   ```
   cat << EOF > src/main.c
   #include <stdio.h>
   #include <curl/curl.h>
   
   int main(void) {
       CURL *curl = curl_easy_init();
       if(curl) {
           printf("CURL initialized successfully\n");
           curl_easy_cleanup(curl);
       } else {
           printf("Failed to initialize CURL\n");
       }
       return 0;
   }
   EOF
   ```

3. Create a makefile for easy compilation:
   ```
   cat << EOF > Makefile
   CC = gcc
   CFLAGS = -I$(HOME)/local/include
   LDFLAGS = -L$(HOME)/local/lib
   LIBS = -lcurl
   
   main: src/main.c
   	$(CC) $(CFLAGS) $(LDFLAGS) $^ $(LIBS) -o $@
   
   .PHONY: clean
   
   clean:
   	rm -f main
   EOF
   ```

4. Compile using:
   ```
   make
   ```

5. Clean using:
   ```
   make clean
   ```

#### Using the project
To run your compiled C program:

```
./main
```

#### Updating libraries
To update a library, you can download the new version, compile, and install it similar to the initial installation process. For example, to update libcurl:

1. Download and extract the new version
2. Navigate to the extracted directory
3. Run the installation commands again:
   ```
   ./configure --prefix=$HOME/local
   make
   make install
   ```

#### Deleting the environment
In C, there's no virtual environment to delete. However, you can remove your local installations and project files:

1. Remove local installations:
   ```
   rm -rf ~/local
   ```

2. Remove project directory:
   ```
   rm -rf my_c_project
   ```

Remember to also remove or comment out the environment variable settings in your `~/.bashrc` or `~/.bash_profile` if you no longer need them.


## C++

### How to use the home directory for development with C++

Create a local C++ development environment using your home directory. Install libraries, create projects, and compile C++ code without needing system-wide installations.

#### Setup environment
Creating directories for binaries, libraries, and include files, and create environment variables.

1. Verify installation:
   ```
   g++ --version
   make --version
   ```

2. Make directory for local installations:
   ```
   mkdir -p ~/local/{bin,lib,include}
   ```

3. Add the following to your `~/.bashrc` or `~/.bash_profile`:
   ```
   export PATH=$HOME/local/bin:$PATH
   export LD_LIBRARY_PATH=$HOME/local/lib:$LD_LIBRARY_PATH
   export CPATH=$HOME/local/include:$CPATH
   export PKG_CONFIG_PATH=$HOME/local/lib/pkgconfig:$PKG_CONFIG_PATH
   ```

4. Reload shell configuration:
   ```
   source ~/.bashrc  # or source ~/.bash_profile
   ```

#### Install packages
Installing packages (libraries) for C++ development in your home directory by downloading source code, compiling it, and installing it in your local directories.

Install a library with example boost:
```
wget https://boostorg.jfrog.io/artifactory/main/release/1.76.0/source/boost_1_76_0.tar.gz
tar xzf boost_1_76_0.tar.gz
cd boost_1_76_0
./bootstrap.sh --prefix=$HOME/local
./b2 install
```

#### Create project
Create a C++ project by creating a directory structure, source files, and a Makefile for easy compilation.

1. Set up project structure:
   ```
   mkdir my_cpp_project
   cd my_cpp_project
   mkdir src
   ```

2. Create a C++ file that uses the library:
   ```
   cat << EOF > src/main.cpp
   #include <iostream>
   #include <boost/version.hpp>
   #include <boost/algorithm/string.hpp>
   
   int main() {
       std::cout << "Boost version: " 
                 << BOOST_VERSION / 100000 << "."
                 << BOOST_VERSION / 100 % 1000 << "."
                 << BOOST_VERSION % 100 << std::endl;
   
       std::string str = "Hello, World!";
       boost::to_upper(str);
       std::cout << "Uppercase: " << str << std::endl;
   
       return 0;
   }
   EOF
   ```

3. Create a Makefile for easy compilation:
   ```
   cat << EOF > Makefile
   CXX = g++
   CXXFLAGS = -I$(HOME)/local/include
   LDFLAGS = -L$(HOME)/local/lib
   LIBS = -lboost_system
   
   main: src/main.cpp
   	$(CXX) $(CXXFLAGS) $(LDFLAGS) $^ $(LIBS) -o $@
   
   .PHONY: clean
   
   clean:
   	rm -f main
   EOF
   ```

4. Compile using:
   ```
   make
   ```

5. Clean using:
   ```
   make clean
   ```

#### Using the project
To run your compiled C++ program:

```
./main
```

#### Updating libraries
To update a library, you can download the new version, compile, and install it similar to the initial installation process. For example, to update Boost:

1. Download and extract the new version
2. Navigate to the extracted directory
3. Run the installation commands again:
   ```
   ./bootstrap.sh --prefix=$HOME/local
   ./b2 install
   ```

#### Deleting the environment
In C++, there's no virtual environment to delete. However, you can remove your local installations and project files:

1. Remove local installations:
   ```
   rm -rf ~/local
   ```

2. Remove project directory:
   ```
   rm -rf my_cpp_project
   ```

Remember to also remove or comment out the environment variable settings in your `~/.bashrc` or `~/.bash_profile` if you no longer need them.

# Rust

### How to simulate a virtual environment with rust

Rust uses Cargo, its package manager and build system, to manage dependencies and create isolated project environments.

#### Setup Environment
Create a new directory and intiialize it with Cargo. This creates a new Rust project with its own Cargo.toml file for managing dependencies.

1. Ensure Rust and Cargo are installed:
   ```bash
   rustc -V
   cargo -V
   ```

2. Create directory for project:
   ```bash
   mkdir Project
   ```

3. Navigate to directory:
   ```bash
   cd Project
   ```

4. Initialize a Cargo project:
   ```bash
   cargo new my_project
   cd my_project
   ```

#### Install Packages
Installing packages in a Rust project adds them to your Cargo.toml file and downloads them to a local cache, keeping your project dependencies isolated and easily reproducible.

Install a package:
```bash
cargo add package_name
```

#### Using Dependencies
Building your Rust project with Cargo automatically downloads and compiles all necessary dependencies specified in your Cargo.toml file.

Install packages using Cargo.toml:
```bash
cargo build
```

#### Delete virtual environment
In Rust, there isn't a traditional "virtual environment" to delete. Rust's Cargo.toml handles this by default. To "delete" this environment:

1. Remove the entire project directory:
   ```bash
   cd ..
   rm -rf my_project
   ```

This will remove the project files, Cargo.toml, and all compiled artifacts.

Note: If you want to keep the source code but remove all compiled artifacts and dependencies, you can instead run:
```bash
cargo clean
```
This will remove the target directory, which contains all compiled files and downloaded dependencies.