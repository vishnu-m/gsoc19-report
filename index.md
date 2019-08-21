## Details

**Organization - LabLua**  <br/>
**Project - C Header File Parser in Lua Using Clang AST** <br/>
**Mentors - Gabriel de Quadros Ligneul, Hugo Musso Gualandi** <br/>
[**Project Link**](https://summerofcode.withgoogle.com/projects/#5042240966098944 "**Project Link**")

## Overview

The goal of this project was to build a Lua library that takes as input a C file and generate as output a Lua representation for the declarations in it. The tool which was used to parse the source code into an Abstract Syntax Tree (AST) is Libclang, which is Clang's C API. 

The benefit of using a tool like Libclang is that, it is robust and highly efficient. This is better than creating the whole parser by hand, from scratch. 

This library will be used to build an FFI to C later on.

## Coding Period

### Phase 1

The initial phase was all about learning how Clang works and understanding what all Libclang (Clang's C API) functions would be required for the project. 

The project was supposed to parse the following kinds of C declarations :
- Enumerations
- Structs and Unions
- Functions
- Typedefs
- Variables

The objective of this phase was only to print the AST(Abstract Syntax Tree) representation to `stdout`. 

Example - 

Input : `sample.c`
```c
struct mycallback {
        int (*f)(int);
};

typedef struct mycallback MYCALLBACK;

MYCALLBACK instance;
```
Output :
```c
StructDecl	"mycallback"		Parent = "src/sample.c"	
FieldDecl	"f"		type = "int (*)(int)"		Parent = "mycallback"
TypedefDecl		underlying type = "struct mycallback"		type = "MYCALLBACK"
VarDecl		"instance"		storage class specifier = ""		type = "MYCALLBACK"

```
The unit tests where made in plain shell script with the help of `cmp` command.

#### Instructions to setup and run tests - 
[README](https://github.com/Vishnu-M/c-decl-parser/blob/master/README.md "README")

**Repository :** [c-decl-parser](https://github.com/Vishnu-M/c-decl-parser "c-decl-parser")

### Phase 2

The objective of this phase was to create Lua binding for the Libclang functions used in the first phase, making use of the Lua C API. This was done so that the parser can be written in Lua itself using the binding.

Example -
Input : `sample.h`
```c
struct student{
        int a;
};

enum A {B, C};

struct student stu1;

typedef struct student STUDENT;

STUDENT foo(struct student a);
```
Sample Lua script : 
```lua
local luaclang = require "luaclang"
local parser = luaclang.newParser("sample.h")
local cursor = parser:getCursor()  
cursor:visitChildren(function(cursor, parent)
        print(cursor:getSpelling())
        return "continue"
end)
parser:dispose()
```
This script prints the spelling of all the *toplevel* declarations.

Output :
```c
student
A
stu1
STUDENT
foo
```
Unit testing in Lua was done with the help of [busted](https://olivinelabs.com/busted/ "busted") framework. Documentation was also done for each of the functions.

#### Instructions to setup and run tests - 
[README](https://github.com/Vishnu-M/lua-clang/blob/master/README.md "README")

**Repository:** [lua-clang](https://github.com/Vishnu-M/lua-clang/ "lua-clang")

### Phase 3

The goal of the last phase was ultimately to build the parser in Lua. This was made along the same lines as the work done in the first phase. But instead of just printing everything, using Lua tables to form the corresponding AST representation.

Citing an example of how the Lua representation of the AST looks like :

For the C code - 

```c
struct a
{
        int b;
        float **c;
};

struct a instance;
```
the corresponding Lua representation as - 

```lua
declarations = { {
    fields = { {
        name = "b",
        type = "int"
      }, {
        name = "c",
        type = {
          tag = "pointer",
          type = {
            tag = "pointer",
            type = "float"
          }
        }
      } },
    name = "a",
    tag = "struct"
  }, {
    name = "instance",
    storage_specifier = "none",
    tag = "variable",
    type = {
      decl = 1, 		--type pointing to the first declaration i.e; struct a
      tag = "decl"
    }
  } }

```

Similarly, work for all the declarations aforementioned have been completed.

The unit testing framework used was again *busted*. 

#### Instructions to setup and run tests - 
[README](https://github.com/Vishnu-M/lua-c-parser/blob/master/README.md "README")

**Repository :** [lua-c-parser](https://github.com/Vishnu-M/lua-c-parser "lua-c-parser")

### Pending work

- Add support for forward declared structs
- Bundle up the lua-clang library into a LuaRocks package
- Sync the project with Travis CI
- Proper handling of error messages

### Contact

- E-Mail : [vishnumurali7000@gmail.com](mailto:vishnumurali7000@gmail.com "vishnumurali7000@gmail.com")
- GitHub : [Vishnu-M](https://github.com/Vishnu-M "Vishnu-M")
