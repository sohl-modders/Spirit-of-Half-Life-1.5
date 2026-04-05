#
# Spirit of Half-Life 1.5 - spirit_*.so Makefile for Linux
#
# Based on the Half-Life Full SDK 2.3 Makefile by Leon Hartwig (Valve Software)
# Modified for Spirit of Half-Life 1.5
#

DLLNAME=spirit

# Detect host architecture; override with ARCH=i486 for 32-bit builds
UNAME_M := $(shell uname -m)
ifeq ($(UNAME_M),x86_64)
    ARCH=amd64
    ARCHFLAGS=-m64
else
    ARCH=i486
    ARCHFLAGS=-march=i486 -m32
endif

# Compiler
CC=g++

# Source directories (relative to this Makefile)
DLL_SRCDIR=../dlls
ENGINE_SRCDIR=../engine
COMMON_SRCDIR=../common
PM_SHARED_SRCDIR=../pm_shared
GAME_SHARED_SRCDIR=../game_shared

# Object directories (inside linux/ build directory)
DLL_OBJDIR=obj/dlls
PM_SHARED_OBJDIR=obj/pm_shared
GAME_SHARED_OBJDIR=obj/game_shared

BASE_CFLAGS=	-Dstricmp=strcasecmp -D_strnicmp=strncasecmp -Dstrnicmp=strncasecmp \
		-DCLIENT_WEAPONS

# -w suppresses warnings from legacy GoldSrc SDK code that uses non-standard constructs
# -fpermissive allows pointer casts that the original MSVC-targeted code relied on
CFLAGS=$(BASE_CFLAGS) -w -fpermissive $(ARCHFLAGS) -O1

INCLUDEDIRS=-I$(DLL_SRCDIR) -I$(ENGINE_SRCDIR) -I$(COMMON_SRCDIR) \
            -I$(PM_SHARED_SRCDIR) -I$(GAME_SHARED_SRCDIR) -I..

LDFLAGS=-lm -lstdc++

SHLIBEXT=so
SHLIBCFLAGS=-fPIC
SHLIBLDFLAGS=-shared

DO_CC=$(CC) $(CFLAGS) $(SHLIBCFLAGS) $(INCLUDEDIRS) -o $@ -c $<
DO_C=$(CC) $(CFLAGS) $(SHLIBCFLAGS) $(INCLUDEDIRS) -x c -o $@ -c $<

#############################################################################
# PATTERN RULES
#############################################################################

$(DLL_OBJDIR)/%.o: $(DLL_SRCDIR)/%.cpp
	$(DO_CC)

$(GAME_SHARED_OBJDIR)/%.o: $(GAME_SHARED_SRCDIR)/%.cpp
	$(DO_CC)

$(PM_SHARED_OBJDIR)/%.o: $(PM_SHARED_SRCDIR)/%.c
	$(DO_C)

#############################################################################
# SOURCE FILES
#############################################################################

OBJ := $(patsubst $(DLL_SRCDIR)/%.cpp,$(DLL_OBJDIR)/%.o,$(wildcard $(DLL_SRCDIR)/*.cpp))
OBJ += $(GAME_SHARED_OBJDIR)/voice_gamemgr.o
OBJ += $(patsubst $(PM_SHARED_SRCDIR)/%.c,$(PM_SHARED_OBJDIR)/%.o,$(wildcard $(PM_SHARED_SRCDIR)/*.c))

#############################################################################
# TARGETS
#############################################################################

all: neat $(DLLNAME)_$(ARCH).$(SHLIBEXT)

$(DLLNAME)_$(ARCH).$(SHLIBEXT): $(OBJ)
	$(CC) $(CFLAGS) $(SHLIBLDFLAGS) $(LDFLAGS) -o $@ $(OBJ)

neat:
	-mkdir -p $(DLL_OBJDIR)
	-mkdir -p $(GAME_SHARED_OBJDIR)
	-mkdir -p $(PM_SHARED_OBJDIR)

clean:
	-rm -f $(DLL_OBJDIR)/*.o
	-rm -f $(GAME_SHARED_OBJDIR)/*.o
	-rm -f $(PM_SHARED_OBJDIR)/*.o
	-rm -f $(DLLNAME)_$(ARCH).$(SHLIBEXT)

spotless: clean
	-rm -rf obj
