ifndef VERBOSE
		QUIET:=@
endif

LLVM_CONFIG?=llvm-config
LDFLAGS+=$(shell $(LLVM_CONFIG) --ldflags)
LDFLAGS+=-ldl -lpthread
LDFLAGS+=-L/usr/local/opt/llvm@5/lib -Wl,-rpath,/usr/local/opt/llvm@5/lib
COMMON_FLAGS=-Wall -Wextra -fno-rtti
CXXFLAGS+=$(COMMON_FLAGS) $(shell $(LLVM_CONFIG) --cxxflags)
CPPFLAGS+=$(shell $(LLVM_CONFIG) --cppflags) -I$(SRC_DIR)
CPPFLAGS+=-I/usr/local/opt/llvm@5/include -I/usr/local/opt/llvm/include/c++/v1/

CLANGLIBS = \
	-Wl \
	-lclang \
	-lclangAST \
	-lclangAnalysis \
	-lclangBasic \
	-lclangDriver \
	-lclangEdit \
	-lclangIndex \
	-lclangFrontend \
	-lclangFrontendTool \
	-lclangLex \
	-lclangParse \
	-lclangSema \
	-lclangEdit \
	-lclangASTMatchers \
	-lclangRewrite \
	-lclangRewriteFrontend \
	-lclangStaticAnalyzerFrontend \
	-lclangStaticAnalyzerCheckers \
	-lclangStaticAnalyzerCore \
	-lclangSerialization \
	-lclangToolingCore \
	-lclangTooling \
	-Wl

LLVMLIBS=$(shell $(LLVM_CONFIG) --libs --system-libs)

SOURCEDIR    = src
BUILDDIR     = build

SOURCE    = main visitor util
HEAD      = main visitor util common

HEADER = $(join $(addsuffix $(SOURCEDIR)/, $(dir $(HEAD))), $(notdir $(HEAD:=.h)))
OBJECT = $(join $(addsuffix $(BUILDDIR)/, $(dir $(SOURCE))), $(notdir $(SOURCE:=.o)))

EXECUTABLE = tool

all: default

default: directory $(OBJECT)
	@echo Linking Project
	$(QUIET)$(CXX) -o $(EXECUTABLE) $(CXXFLAGS) $(OBJECT) $(CLANGLIBS) $(LLVMLIBS) $(LDFLAGS)

directory:
	mkdir -p $(BUILDDIR) 

$(BUILDDIR)/%.o : $(SOURCEDIR)/%.cpp $(HEADER)
	@echo Compiling $*.cpp
	$(QUIET)$(CXX) -o $@ -c $(CPPFLAGS) $(CXXFLAGS) $<


clean:
	$(QUIET)rm -rf $(EXECUTABLE) $(BUILDDIR)
