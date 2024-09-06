# Virtual Environment Guide

This guide provides different methods for installing virtual environments in multiple languages including 
Python, R, Julia, Rust, C, C++, and other languages. This will allow you to create projects in isolated environments that will not require root or sudo access.

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

# C and C++ Development Guide Using Home Directory

## C

### How to use the home directory for development with C

#### Setup environment

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

## C++

### How to use the home directory for development with C++

#### Setup environment

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

Install a library with example boost:
```
wget https://boostorg.jfrog.io/artifactory/main/release/1.76.0/source/boost_1_76_0.tar.gz
tar xzf boost_1_76_0.tar.gz
cd boost_1_76_0
./bootstrap.sh --prefix=$HOME/local
./b2 install
```

#### Create project

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
# Rust

### How to simulate a virtual environment with rust

This guide outlines how to create a project-specific environment in Rust, similar to virtual environments in other languages. (Most similar to node)

## Setup Environment

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

## Install Packages

Install a package:
```bash
cargo add package
```

## Using Dependencies

Install packages using Cargo.toml:
```bash
cargo build
