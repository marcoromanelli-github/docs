---
sort: 1
---

# Environment modules

## Introduction to Environment Modules

In an HPC cluster, the diversity and quantity of installed software span many applications in various versions. Often, these applications are installed in non-standard locations for ease of maintenance, practicality, and security reasons. Due to the shared nature of the HPC cluster and its significant scale compared to typical desktop compute machinery, it's neither possible nor desirable to use all these different software versions simultaneously, as conflicts between them may arise. To manage this complexity, we provide the production environment for each application outside of the application itself, through a set of instructions and variable settings known as an application module. This approach not only prevents conflicts but also simplifies control over which application versions are available for use in any given session. We utilize the `lmod` module system for this purpose, with the `module` command being the primary tool for managing these software environments. 

For example, if a user needs to work with a specific Python environment provided by Anaconda, they can simply load the Anaconda module by executing `module load anaconda`.
This is just one instance of how the module command can be utilized. In the following sections, we will fully discuss the module command and its use cases.

For a complete list of options with the `module` command:
```bash
[me@cluster ~]$ module --help
```

## Loading and Managing Modules
### Checking Loaded Modules
To see the modules currently active in your session:
```bash
module list
```

### Listing Available Modules
To view all available modules:
```bash
module avail
```
The list will include both local modules (specific to the node or head node) and shared modules (available from shared storage).

### Loading a Module
To load a module, for example, `gcc`:
```bash
module load gcc
```

To load a specific version of a module:
```bash
module load gcc/11.2.0
```
**Make sure to check the availability of a specific version of the module you are looking for, using `module avail` as it might not be available on the cluster**.

### Unloading Modules
To unload a module:
```bash
module unload gcc
```

### Switching Module Versions
To switch to a different version of a module:
```bash
module switch intel intel/2016b
```

### Avoiding Module Conflicts
Be aware of potential conflicts, especially with MPI modules. Loading conflicting modules like `openmpi` and `impi` simultaneously should be avoided.
Using the `shared` Module

The `shared` module provides access to shared libraries and is often a dependency for other modules. It is typically loaded first:
```bash
module load shared
```

### Setting Default Modules
To avoid manually loading the same modules every login, users can set an initial default state for modules using `module init*` subcommands:

* **Add a module to initial state**: `module initadd <module_name>`
* **Remove a module from initial state**: `module initrm <module_name>`
* **List initial modules**: `module initlist`
* **Clear initial modules**: `module initclear`

Example:
```bash
module initclear
module initadd shared gcc openmpi/gcc
```

### Available Commands and Practical Usage
For practical use of the modules commands:
* **Loading and unloading modules**: `module load <module_name>, module unload <module_name>`
* **Listing loaded and available modules**: `module list, module avail`
* **Switching modules**: `module switch <old_module> <new_module>`
* **Finding out what a module does**: `module whatis <module_name>`

Example of loading modules:
```bash
[fred@cluster ~]$ module load shared gcc openmpi/gcc
```

> Tab completion is available for suggesting modules for the add/load commands.

Example of unloading modules:
```bash
[fred@cluster ~]$ module unload gcc openmpi/gcc
```

### Managing the Default Environment
Users can customize their default environment using module `init*` commands. This ensures the desired modules are automatically loaded at login.

Example:
```bash
[fred@cluster ~]$ module initclear
[fred@cluster ~]$ module initadd shared gcc openmpi/gcc
[fred@cluster ~]$ module initlist
```

## Additional Information
* **Conflicts and Dependencies**: Users should be mindful of conflicts between modules and dependencies, particularly with MPI implementations.
* **Testing New Software Versions**: Modules allow for easy testing of new software versions without permanent installation.

For further details, users are encouraged to refer to the man pages for module and modulefile:
```bash
man module
```
