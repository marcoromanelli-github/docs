
# Environment Modules

To compile and run jobs on a cluster, a special shell environment is typically set up for the software that is used. However, setting up the right environment for a particular software package and version can be tricky, and it can be hard to keep track of how it was set up.

For example, users want to have a clean way to bring up the right environment for compiling code according to the various MPI implementations, but it can easily get confusing about which libraries have been used or are needed, and one can end up with multiple libraries with similar names installed in a disorganized manner.

One might also like to conveniently test new versions of a software package before permanently installing the package.

On a Linux system without special utilities, setting up environments can be complex. To simplify this task, HPC clusters often make use of the environment modules package, which provides the `module` command. The `module` command is a handy utility that makes taking care of the shell environment much easier.

## Available Commands

Practical use of the modules commands is given in the following sections.

For reference, the help text for the module command can be viewed as follows:

```
[me@cluster ~]$ module --help
Modules Release 4.5.3 (2020-08-31)
Usage: module [options] [command] [args ...]

Loading / Unloading commands:
  add | load      modulefile [...]  Load modulefile(s)
  rm | unload     modulefile [...]  Remove modulefile(s)
  purge                             Unload all loaded modulefiles
  reload | refresh                  Unload then load all loaded modulefiles
  switch | swap   [mod1] mod2       Unload mod1 and load mod2

Listing / Searching commands:
  list            [-t|-l|-j]        List loaded modules
  avail   [-d|-L] [-t|-l|-j] [-S|-C] [--indepth|--no-indepth] [mod ...]
                                    List all or matching available modules
  aliases                           List all module aliases
  whatis  [-j]    [modulefile ...]  Print whatis information of modulefile(s)
  apropos | keyword | search [-j] str
                                    Search all name and whatis containing str
  is-loaded       [modulefile ...]  Test if any of the modulefile(s) are loaded
  is-avail        modulefile [...]  Is any of the modulefile(s) available
  info-loaded     modulefile        Get full name of matching loaded module(s)

Collection of modules handling commands:
  save            [collection|file] Save current module list to collection
  restore         [collection|file] Restore module list from collection or file
  saverm          [collection]      Remove saved collection
  saveshow        [collection|file] Display information about collection
  savelist        [-t|-l|-j]        List all saved collections
  is-saved        [collection ...]  Test if any of the collection(s) exists

Shell's initialization files handling commands:
  initlist                          List all modules loaded from init file
  initadd         modulefile [...]  Add modulefile to shell init file
  initrm          modulefile [...]  Remove modulefile from shell init file
  initprepend     modulefile [...]  Add to beginning of list in init file
  initswitch      mod1 mod2         Switch mod1 with mod2 from init file
  initclear                         Clear all modulefiles from init file

Environment direct handling commands:
  prepend-path [-d c] var val [...] Prepend value to environment variable
  append-path [-d c] var val [...]  Append value to environment variable
  remove-path [-d c] var val [...]  Remove value from environment variable

Other commands:
  help            [modulefile ...]  Print this or modulefile(s) help info
  display | show  modulefile [...]  Display information about modulefile(s)
  test            [modulefile ...]  Test modulefile(s)
  use     [-a|-p] dir [...]         Add dir(s) to MODULEPATH variable
  unuse           dir [...]         Remove dir(s) from MODULEPATH variable
  is-used         [dir ...]         Is any of the dir(s) enabled in MODULEPATH
  path            modulefile        Print modulefile path
  paths           modulefile        Print path of matching available modules
  clear           [-f]              Reset Modules-specific runtime information
  source          scriptfile [...]  Execute scriptfile(s)
  config [--dump-state|name [val]]  Display or set Modules configuration

Switches:
  -t | --terse    Display output in terse format
  -l | --long     Display output in long format
  -j | --json     Display output in JSON format
  -d | --default  Only show default versions available
  -L | --latest   Only show latest versions available
  -S | --starts-with
                  Search modules whose name begins with query string
  -C | --contains Search modules whose name contains query string
  -i | --icase    Case insensitive match
  -a | --append   Append directory to MODULEPATH
  -p | --prepend  Prepend directory to MODULEPATH
  --auto          Enable automated module handling mode
  --no-auto       Disable automated module handling mode
  -f | --force    By-pass dependency consistency or confirmation dialog

Options:
  -h | --help     This usage info
  -V | --version  Module version
  -D | --debug    Enable debug messages
  -v | --verbose  Enable verbose messages
  -s | --silent   Turn off error, warning and informational messages
  --paginate      Pipe mesg output into a pager if stream attached to terminal
  --no-pager      Do not pipe message output into a pager
  --color[=WHEN]  Colorize the output; WHEN can be 'always' (default if
                  omitted), 'auto' or 'never'
[me@cluster ~]$
```

## Managing Environment Modules

There is a good chance the cluster administrator has set up the user’s account, `fred` for example, so that some modules are loaded already by default. In that case, the modules loaded into the user’s environment can be seen with the `module list` command:

```
[fred@cluster ~]# module list
Currently Loaded Modulefiles:
 1) shared   2) slurm/slurm/21.08.8   3) gcc/11.2.0
```

If there are no modules loaded by default, then the `module list` command just returns nothing.

How does one know what modules are available? The `module avail` command lists all modules that are available for loading:

```
[fred@cluster ~]$ module avail
---------------------------- /cm/local/modulefiles -----------------------------
apptainer/1.0.2           cmsh               module-git
boost/1.77.0              cuda-dcgm/3.1.3.1  module-info
cluster-tools/9.2         dot                null
cm-bios-tools             freeipmi/1.6.8     openldap
cm-image/9.2              gcc/11.2.0         openmpi/mlnx/gcc/64/4.1.5a1
cm-scale/cm-scale.module  ipmitool/1.8.18    python3
cm-setup/9.2              lua/5.4.4          python39
cmd                       luajit             shared
cmjob                     mariadb-libs       slurm/slurm/21.08.8

---------------------------- /cm/shared/modulefiles ----------------------------
blacs/openmpi/gcc/64/1.1patch03  hwloc/1.11.11
blas/gcc/64/3.10.0               hwloc2/2.7.1
bonnie++/2.00a                   intel-tbb-oss/ia32/2021.4.0
cm-pmix3/3.1.4                   intel-tbb-oss/intel64/2021.4.0
cm-pmix4/4.1.1                   iozone/3_492
cuda11.8/blas/11.8.0             lapack/gcc/64/3.10.0
cuda11.8/fft/11.8.0              mpich/ge/gcc/64/3.4.2
cuda11.8/toolkit/11.8.0          mvapich2/gcc/64/2.3.7
default-environment              netcdf/gcc/64/gcc/64/4.8.1
fftw3/openmpi/gcc/64/3.3.10      netperf/2.7.0
gdb/10.2                         openblas/dynamic/(default)
git/2.33.1                       openblas/dynamic/0.3.18
globalarrays/openmpi/gcc/64/5.8  openmpi/gcc/64/4.1.2
hdf5/1.12.1                      openmpi4/gcc/4.1.2
hdf5_18/1.8.21                   ucx/1.10.1

```

In the list there are two kinds of modules:

* **local modules**, which are specific to the node, or head node only
* **shared modules**, which are made available from a shared storage, and which only become available for loading after the `shared` module is loaded.

Modules can be loaded using the `add` or `load` options. A list of modules can be added by spacing them:

```
[fred@cluster ~]$ module add shared gcc openmpi/gcc
```

Tab completion works for suggesting modules for the add/load commands. If the tab completion suggestion is unique, even though it is not the full path, then it is still enough to specify the module. For example, looking at the possibile available modules listed by the avail command previously, it turns out that specifying `gcc` is enough to specify `gcc/11.2.0` because there is no other directory path under `gcc/` besides `11.2.0` anyway.

To remove one or more modules, the `module unload` or `module rm` command is used.

To remove all modules from the user’s environment, the `module purge` command is used.

Users should be aware that some loaded modules can conflict with others loaded at the same time. This can happen with MPI modules. For example, loading `openmpi/gcc` without removing an already loaded `intel/mpi/64` can result in conflicts about which compiler should be used.

### The `shared` Module

The `shared` module provides access to shared libraries. By default these are under `/cm/shared`.

The `shared` module is special because often other modules, as seen under `/cm/shared/modulefiles`, depend on it. So, if it is to be loaded, then it is usually loaded first, so that the dependent modules can use it.

The shared module is obviously a useful local module, and is therefore often configured to be loaded for the user by default. Setting the default environment modules is discussed in the next section.

## Changing The Default Environment Modules

If a user has to manually load up the same modules every time upon login it would be inefficient. That is why an initial default state for modules can be set up by the user, by using the module `init*` subcommands:

The more useful ones of these are:

* `module initadd`: add a module to the initial state
* `module initrm`: remove a module from the initial state
* `module initlist`: list all modules loaded initially
* `module initclear`: clear all modules from the list of modules loaded initially

Example:

```
[fred@cluster ~]$ module initclear
[fred@cluster ~]$ module initlist
bash initialization file $HOME/.bashrc loads modules:

[fred@cluster ~]$ module initadd shared gcc openmpi/gcc
[fred@cluster ~]$ module initlist
bash initialization file $HOME/.bashrc loads modules:
   shared gcc openmpi/gcc
```

In the preceding example, the modules defined for the new initial environment for the user are loaded from the next login onward.

Example:

```
[fred@cluster ~]$ module list
No Modulefiles Currently Loaded.
[fred@cluster ~]$ exit
logout
Connection to bright92 closed
[root@basejumper ~]# ssh fred@cluster
fred@cluster's password:
...
[fred@cluster ~]$ module list
Currently Loaded Modulefiles:
 1) shared 2) gcc/9.2.0 3) openmpi/gcc/64/1.10.7
[fred@cluster ~]$
```

If you are unsure about what the module does, it can be checked using `module whatis`:

```
$ module whatis openmpi/gcc
----------------------------------- /cm/shared/modulefiles ------------------------------------
openmpi/gcc/64/1.10.7: adds OpenMPI to your environment variables
```

The man pages for module and modulefile give further details on usage.

