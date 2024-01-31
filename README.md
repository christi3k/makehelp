# makehelp

<div style="font-size: 150%; font-weight: bold; text-align: center;">! Doxygen (sort of) for GNU make !</div>

"Sort of" in that `makehelp` generates text to screen based on in-line comments. It does not generate other kinds of documentation (e.g. html). It works best interactively on the command line like you might use `man` or `info`.

**Note:** This is forked and modified version of the original [christianhujer/makehelp](https://github.com/christianhujer/makehelp) by Christian Hujer. Thank you Christian! I ended up making changes that I'm not sure are compatible with Christian's vision. If I have time, I'll figure out which make sense to submit as PRs upstream. In the meantime, I'm sharing them here in case they are useful for anyone.

Changes from the upstream version:

- Changed comment style to single hash only `#`.
- Enabled support for single spacing.
- Update variable parsing to support spaces around assignment.
- Added display of prerequisites to GOALs listing.
- Made some minor formatting changes to improve readability.
- Updated `install` target to support macOS (tested on macOS Sonoma 14.2).
- Added support for a `Description` comment at the top to explain the purpose of the makefile.

## What is `makehelp`?
`makehelp.pl` is a Makefile and a Perl script which provide built-in doxygen-style help in `Makefile`s.
Special comments in the `Makefile` are used to provide user documentation for goals and variables.
These comments are extracted by makehelp to print a nice documentation.
That documentation is similar to the built-in help texts of UNIX commands.

## Using `makehelp` without Installation
To use `makehelp` in your project without installing it, append the following lines to your `Makefile`:

~~~~make
-include .makehelp/include/makehelp/Help.mk

ifeq "help" "$(filter help,$(MAKECMDGOALS))"
.makehelp/include/makehelp/Help.mk:
	git clone --depth=1 https://github.com/christi3k/makehelp.git .makehelp
endif
~~~~

And add the following lines to your `.gitignore`:

~~~~.gitignore
.makehelp
.help.mk
~~~~


## Installing `makehelp`
Makehelp can run with or without installation.
If you want to install makehelp, run `sudo make install`.

Per default, the include files are installed in `/usr/local/include/makehelp/` and the binary files are installed in `/usr/local/bin/`.

## Using `makehelp`.

### Running `makehelp` from your `Makefile`
To run `makehelp` from your `Makefile`, simply include the following line in your `Makefile`:

~~~~make
-include makehelp/Help.mk
~~~~

The best position to include this is at the **end** of your `Makefile`.

If you are including other Makefiles and do not want to export the help they provide, include those Makefiles after `makehelp/Help.mk`, not before.

### Using `makehelp` in your `Makefile`
Makehelp comments start with `##` instead of `#`.
They should be positioned before those variables or goals that you want to document.

Included makefiles are supported.

#### Example for a variable documentation

~~~~make
# The prefix path for installation.
# Unless overridden individually, other installation paths are derived from this.
PREFIX:=/usr/local/
~~~~

#### Example for goal documentation

~~~~make
.PHONY: all
# Builds everything.
all: hello
~~~~

#### Sample Makefile

~~~~make
# Description: Sample makefile.

# The prefix path for installation.
# Unless overridden individually, other installation paths are derived from this.
PREFIX:=/usr/local/

# The path to install binary files.
BINDIR=$(PREFIX)bin/

.PHONY: all
# Builds everything.
all: hello

.PHONY: clean
# Removes generated files.
clean::
	$(RM) hello hello.o

.PHONY: install
# Installs the binary program to $(BINDIR).
# On most systems, this needs to be run as root, i.e. using sudo.
install: all
	install -d $(BINDIR) hello

-include makehelp/Help.mk
~~~~

Sample output of running `make help`:

~~~~none
Usage: make [OPTION|GOAL|VARIABLE]...
Runs make to make the specified GOALs.
If no GOAL is specified, the default goal is made (usually "all").

Popular make OPTIONs:
  -s    Silent mode, disables command echo.
  -k    Keep going, continues after errors.
  -j N  Run N jobs in parallel.
  -n    Don't run the commands, just print them.
  -q    Run no commands; exit status says if up to date.
  -h    Print make help text.
Use option -h to lists the GNUmake part of the help.

Makefile: Sample makefile.

A VARIABLE is specified as name=value pair.
Supported VARIABLEs:
  BINDIR            The path to install binary files.
                    Current value: $(PREFIX)bin/
  PREFIX            The prefix path for installation.
                    Unless overridden individually, other installation paths are derived from this.
                    Current value: /usr/local/

Supported GOALs:
  all               Builds everything.
  clean             Removes generated files.
  help              Prints this help message.
  install           Installs the binary program to $(PREFIX)bin/.
                    On most systems, this needs to be run as root, i.e. using sudo.
~~~~

For more examples, see
* https://github.com/christianhujer/sclog4c

## Too "heavy"?
While `makehelp` is designed to be lightweight, some might consider it still "too heavy".
If you believe that `makehelp` is still too heavy, you could try using `sed` instead, like this:

~~~~make
# My little Makefile
#

.PHONY: all
# all: Builds the hello, world application.
all: hello

.PHONY: clean
# clean: Removes all auto-generated files.
clean::
	$(RM) hello *.[adios]

.PHONY: help
# help: Prints this help text.
help:
	sed -n 's/^# \?//p' $(MAKEFILE_LIST)

##
## Questions? Send an email to <…@…>
~~~~

This would, running `make help`, produce the following output:
~~~~none
My little Makefile

all: Builds the hello, world application.
clean: Removes all auto-generated files.
help: Prints this help text.

Questions? Send an email to <…@…>
~~~~

Have fun!
