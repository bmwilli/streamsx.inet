# Copyright (C) 2013-2014, International Business Machines Corporation
# All Rights Reserved
$(info ************************************************************************)
$(info ***  make libftp                                                     ***)
$(info ************************************************************************)

# Handle PPC platforms that require toolchain Compiler
ifeq ($(CXX),g++)
  PLATFORM := $(shell uname -i)
  ifeq ($(PLATFORM),ppc64)
    CXX=/opt/at7.0/bin/g++
  endif
endif


# InfoSphere Streams Compiler/Linker Options
SPL_PKGCFG=$(realpath $(STREAMS_INSTALL)/bin/dst-pe-pkg-config.sh)
ifeq "" "$(SPL_PKGCFG)"
  $(error ERROR: STREAMS_INSTALL is not set)
endif
SPL_PKG=dst-spl-pe-install
SPL_COMPILE_OPTIONS = $(shell $(SPL_PKGCFG) --cflags $(SPL_PKG))
SPL_LINK_OPTIONS = $(shell $(SPL_PKGCFG) --libs $(SPL_PKG))
#$(info "$(SPL_COMPILE_OPTIONS)")
#$(info "$(SPL_LINK_OPTIONS)")

# -----------------------------------------------------------------------------
# Folders
# -----------------------------------------------------------------------------
TRG := .
INC:= $(TRG)/cpp/include
LIB := $(TRG)/lib
OBJ := $(TRG)/cpp/bin
SRC := $(TRG)/cpp/src

# -----------------------------------------------------------------------------
# phony targets are targets being not in the filesystem but are always "active"
# -----------------------------------------------------------------------------

.PHONY: all build clean clean_objects clean_library help $(LIB) $(OBJ)

# -----------------------------------------------------------------------------
# Library
# -----------------------------------------------------------------------------

LIBRARY := $(LIB)/libinettoolkit.so

# -----------------------------------------------------------------------------
# Helpers
# -----------------------------------------------------------------------------

INCLUDES = $(wildcard $(INC)/*.h) $(wildcard $(SRC)/*.h) 
SOURCES  = $(wildcard $(SRC)/*.cpp)
OBJECTS  = $(patsubst $(SRC)/%.cpp,$(OBJ)/%.o,$(SOURCES))
CXXFLAGS := -O3 $(SPL_COMPILE_OPTIONS) -Werror -Wall
LINKFLAGS := -shared $(SPL_LINK_OPTIONS)

all: clean build clean_objects

build: $(LIBRARY)

$(LIB) $(OBJ):
	@mkdir -pv $@
	
$(OBJ)/%.o: $(SRC)/%.cpp $(INCLUDES)
	@echo Compiling '$<' ...
	$(CXX) $(CXXFLAGS) -c $< -o $@ -I $(INC)

$(LIBRARY): $(LIB) $(OBJ) $(OBJECTS)
	@echo Building C++ shared library '$@'
	$(CXX) $(LINKFLAGS) -o $@  $(OBJECTS)

clean: clean_objects clean_library

clean_objects:
	@echo Cleaning object files ...
	@rm -rfv $(OBJ)

clean_library:
	@echo Cleaning library files ...
	@rm -fv $(LIBRARY)

help:
	@echo "make -f Makefile.libftp <all|clean|build|help>"
	@echo "targets:"
	@echo "help: print this help text"
	@echo "all: clean build this is the default target"
	@echo "clean: remove library and object files"
	@echo "build: build the library" 
	
