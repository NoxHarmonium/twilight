Lithium
========

A Highly Reactive Programming Language and Runtime
--------------------------------------------------

[![Build Status](https://travis-ci.org/NoxHarmonium/lithium-lang.svg?branch=master)](https://travis-ci.org/NoxHarmonium/lithium-lang)

This isn't really a thing,
it is just me experimenting
to see if I can create the basic
building blocks of a programming
language, write a basic lexer and
a parser and emit LLVM bytecode.

If it actually did evolve into a full language the goals might be something like this:
- Reactive programming as a first class concept and built into the language
- Safe concurrency by default using message passing between objects
- Strongly functional
- Stict depedency injection as part of the langauge
- Compiled rather than intepreted and no VM so it can run on iOS which disallows things like JITing.

As I work through existing languages for insperation and ideas I will add notes [here](doc/insperation.md).

### Building

#### Quick Start (OSX and Homebrew)
Long story short, if you are building on Mac OSX and use Homebrew 
as a package manager you will need to do the following things in your terminal.

```
# Make sure the stack build system is installed
brew install stack

# We need llvm 3.5 for the llvm-general package to work
brew tap homebrew/versions
brew install llvm35

# Checkout the project
git clone https://github.com/NoxHarmonium/lithium-lang.git
cd lithium-lang

# Setup the project
stack init # You would only need this if you have never used stack before
stack setup
stack build

```

#### Slow Start (OSX and not Homebrew)

If you use a different package manager you will have to modify 
`extra-lib-dirs` setting in stack.yaml to point to your package managers 
library directory.

Then you can follow the Quick Start guide.

#### Very Slow Start (Not OSX)

I have not tried this yet so I can't give advice but I assume it will 
work easily on Linux but not at all on Windows.


#### Troubleshooting

##### Libffi Issues

If you get an error message that looks like the following:

```
dyld: Library not loaded: @rpath/libffi.dylib
...   
Reason: Incompatible library version: LibraryFunction_hsc_make requires version 7.0.0 or later, but libffi.dylib provides version 1.0.0
```

This happens because the build process cannot find the libffi library
that was used to build llvm.
Modify stack.yaml to point to the directory where the version of libffi
that your llvm setup is using. With homebrew this is actually tucked 
away under the `/usr/local/opt/libffi/lib` directory because it is a 
'keg only library'.

[Reference](https://github.com/commercialhaskell/stack/issues/1826)

#### Random Linking Issues

If you get something like the following:

```
unknown symbol `___dso_handle
```

or some other linking issue with lots of random characters. It is 
probably because you need to build llvm-general with the 
`-fenable-shared` flag. I have already handled with by adding an option
to stack.yaml but I am adding this so that other people don't have to 
go through my pain if it happens to them.

[Reference](https://github.com/bscarlet/llvm-general/issues/85)