## Details

**Organisation - LabLua**  <br/>
**Project - C Header File Parser in Lua Using Clang AST** <br/>
**Mentors - Gabriel de Quadros Ligneul, Hugo Musso Gualandi** <br/>

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

**Repository :** [c-decl-parser](https://github.com/Vishnu-M/c-decl-parser "c-decl-parser")

## Phase 2

The objective of this phase was to create Lua bindings for the Libclang functions used in the first phase, making use of the Lua C API. This was done so that the parser can be written in Lua itself using the bindings.

Unit testing in Lua was done with the help of [busted](https://olivinelabs.com/busted/ "busted") framework. Documentation was also done for each of the functions.

**Repository:** [lua-clang](https://github.com/Vishnu-M/lua-clang/ "lua-clang")

## Phase 3

The goal of the last phase was ultimately to build the parser in Lua. This was made along the same lines as the work done in first phase, but instead of just printing everything, making use of tables to form the corresponding AST representation in Lua.

The intitial goal was to represent the types as simple strings.

For example, 

`int* a[10]` has type `int* [10] `

The final deliverable was to represent the types in a more elaborate manner i.e;

```lua
type = {
      n = 10.0,
      tag = "array",
      type = {
        tag = "pointer",
        type = "int"
      }
    }

```

Citing another example,

```c
struct a
{
        int b;
};

struct a instance; 
```
has corresponding Lua representation as - 

```lua
{ {
        fields = { {
            name = "b",
            type = "int"
          } },
        name = "a",
        tag = "struct"
      }, {
        name = "instance",
        storage_specifier = "none",
        tag = "variable",
        type = {
          decl = 1,                             --type pointing to the first declaration i.e; struct a
          tag = "decl"
        }
} }
```

Similarly, work for all the declarations mentioned have been completed.

The unit testing framework used was again *busted*. 

**Repository :** [lua-c-parser](https://github.com/Vishnu-M/lua-c-parser "lua-c-parser")

### Pending work

- Add support for forward declared structs
- Bundle up the lua-clang library into a LuaRocks package
