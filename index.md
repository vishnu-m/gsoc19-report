## Details

**Organisation - LabLua**
**Project - C Header File Parser in Lua Using Clang AST**
**Mentors - Gabriel de Quadros Ligneul, Hugo Musso Gualandi**

## Overview

Pallene, the statically typed sister language of Lua requires a Foreign Function Interface(FFI) to C. The goal of this project was to build a Lua library that takes as input a C file and generate as output a Lua representation for the declarations in it.
This library will be used to create the FFI later on.

## Work 

### Phase 1

The initial phase was all about learning how Clang works and understanding what all Libclang (Clang's C API) functions would be required for the project. 

Declarations handled - 
- Enumerations
- Structs and Unions
- Functions
- Typedefs
- Variables

This phase concerned only printing the AST(Abstract Syntax Tree) representation to `stdout`. 

The unit tests where made in plain shell script with the help of `cmp` command.

**Repository : **
[c-decl-parser](https://github.com/Vishnu-M/c-decl-parser "c-decl-parser")
