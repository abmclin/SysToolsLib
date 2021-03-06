###############################################################################
#                                                                             #
#  File name        Makefile                                                  #
#                                                                             #
#  Description      A GNU make makefile to build all SysToolsLib  program.    #
#                                                                             #
#  Notes            MUST BE EXECUTED BY GMAKE (GNU Make), NOT UNIX MAKE.      #
#                   Else the conditional directives won't work.               #
#                                                                             #
#  History                                                                    #
#    2016-10-14 JFL jf.larvoire@hpe.com created this file.                    #
#    2017-08-29 JFL `make install` now also installs Tcl files.               #
#                                                                             #
#         � Copyright 2016 Hewlett Packard Enterprise Development LP          #
# Licensed under the Apache 2.0 license - www.apache.org/licenses/LICENSE-2.0 #
###############################################################################

# Standard installation directories.
# NOTE: This directory must exist when you start the install.
prefix = /usr/local
datarootdir = $(prefix)/share
datadir = $(datarootdir)
exec_prefix = $(prefix)
# Where to put the executables.
bindir = $(exec_prefix)/bin
# Where to put the libraries.
libdir = $(exec_prefix)/lib
# Where to put the info files.
infodir = $(datarootdir)/info

# Default rule.
.PHONY: default
default: all

# Place holder for environment dependancies rules
.PHONY: check
check:
	@if ! ( echo ":$(PATH):" | grep -q ":$(bindir):" ) ; then \
	  >&2 echo ERROR: $(bindir) not in PATH. Please add it for the installed programs to work. ; \
	  false ; \
	fi
	@echo Success
	@true

# How to reluild all targets
DIRS=SysLib SRC
.PHONY: all
all: $(DIRS)
	@echo "All done."

STINCLUDE=$(CURDIR)/include
SYSLIB=$(CURDIR)/SysLib
MAKEDEFS="STINCLUDE=$(STINCLUDE)" "SYSLIB=$(SYSLIB)"

$(DIRS): force
	$(MAKE) -$(MAKEFLAGS) -C $@ $(MAKEDEFS)

# Force building dependent targets
force: ; 

# How to install all programs built above
.PHONY: install
install:
	# Install Tcl files, renaming them without the .tcl extension
	@cd ../Tcl && for f in `cat ../Scripts.lst | grep Tcl.*tcl | sed -r "s/Tcl.(.*).tcl.*/\\1/"` ; do \
	  echo cp -p $$PWD/$$f.tcl $(bindir)/$$f ; \
	       cp -p $$PWD/$$f.tcl $(bindir)/$$f ; \
	done
	# Then recurse into SRC to install C executables
	@$(MAKE) -$(MAKEFLAGS) -C SRC install

# Cleanup all
.PHONY: clean
clean:
	for dir in $(DIRS) ; do $(MAKE) -$(MAKEFLAGS) -C $$dir clean ; done
	-$(RM) *.log    >/dev/null 2>&1

define HELP
Usage: make [MAKEOPTS] [MAKEDEFS] [TARGETS]

MakeDefs:
  STINCLUDE=PATH    SysToolsLib global include dir. Default: $(STINCLUDE)
  SYSLIB=PATH       SysLib library base dir. Default: $(SYSLIB)

Targets:
  all       Build all SysToolsLib libraries and programs. Default.
  clean     Delete all files generated by this Makefile
  help      Display this help message
  install   Copy the programs built and tcl scripts to $$bindir. Default: $(bindir)

endef

export HELP
help:
	echo "$$HELP"

