# Which software is installed on Star

To find out what software packages are available, type:

    module avail

## Changes in application software

News about planned/unplanned downtime, changes in hardware, and
important changes in software will be published on the HPC Hofstra twitter
account <https://twitter.com/hpc_uit> and on the login screen of star.
For more information on the different situations see <span
class="title-ref">news</span>.

The easiest way to check which software and versions available is to use
the `module` command. List all software available:

    module avail

List all version of a specific software:

    module avail software-name

# Missing or new software

If there is any software missing on this list that you would like to
have installed on Star, or you need help to install your own software,
please feel free to contact the support personal about this:
<starhpc-support@hofstra.edu>.

If there is software missing on Star that you would like to have,
there are different options to install it: \* Use our installation
framework EasyBuild \* Use a package manager like Conda or especially
for bioinformatic software Bioconda \* Compile it yourself and install
into your home directory \* Ask us to install for you

In this document we will show some ways of installing a new software. If
you run into problems, need help or have question, please feel free to
contact the support personal about this: <starhpc-support@hofstra.edu>.

## Installing new software using EasyBuild

Our building and installation framework,
[EasyBuild](https://easybuild.readthedocs.io/en/latest/Using_the_EasyBuild_command_line.html)
can be used by you to install software into your own home directory.
This is especially interesting for you if you need a software but it is
probably not used by other users of Star, so a systemwide installation
doesn't make much sense.

To see if your software is available via the EasyBuild system have a
look into the [easyconfig github
repository](https://github.com/easybuilders/easybuild-easyconfigs). if
you can find a version of your software there. Alternatively use the
search function of EasyBuild:

    eb -S REGEX

Let's assume you want to install the genome aligner Kraken2. We find a
easyconfig file with the name
[Kraken2-2.0.7-beta-foss-2018b-Perl-5.28.0.eb](https://github.com/easybuilders/easybuild-easyconfigs/blob/master/easybuild/easyconfigs/k/Kraken2/Kraken2-2.0.7-beta-foss-2018b-Perl-5.28.0.eb)
in the easyconfig repository.

Before we start the installation process we first have to load the
necessary modules:

    ml purge
    ml EasyBuild

Before we actually build we can check the building process by adding
<span class="title-ref">--dry-run-short</span> or <span
class="title-ref">-D</span> to the build command:

    eb Kraken2-2.0.7-beta-foss-2018b-Perl-5.28.0.eb --dry-run-short

As you probably see, a long list of packages is printed with some
packages being marked as missing. To automatically install all necessary
dependencies we can add <span class="title-ref">--robot</span> and then
run the installation:

    eb Kraken2-2.0.7-beta-foss-2018b-Perl-5.28.0.eb --robot

Before we can load your newly installed module, we have to add the path
to the Lmod search path. You might have to do this from time to time, or
even every time you use your software, as your local cache is regularly
updated. :

    ml use ~/.local/easybuild/modules/all
    ml Kraken2/2.0.7

## Conda/ Bioconda

Many software packages, especially if they are python based, can be
easily installed using the Conda package manager. For many
bioinformatics software, Bioconda has become a nice solution.

A small tutorial can be found in the [Python]({% link software/python_r_perl.md %}#python) section of this
documentation.
