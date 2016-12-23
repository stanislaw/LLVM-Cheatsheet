# LLVM Cheatsheet

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Clang](#clang)
  - [Generate human-readable LLVM IR from a file.](#generate-human-readable-llvm-ir-from-a-file)
  - [Generate LLVM Bitcode from a file.](#generate-llvm-bitcode-from-a-file)
- [lli](#lli)
  - [Execute Objective-C code compiled into LLVM Bitcode using LLVM JIT](#execute-objective-c-code-compiled-into-llvm-bitcode-using-llvm-jit)
- [Ninja](#ninja)
  - [Get compilation_database.json](#get-compilation_databasejson)
  - [Get list of commands required to build a target](#get-list-of-commands-required-to-build-a-target)
- [dyld](#dyld)
  - [Display libraries that are loaded by dyld](#display-libraries-that-are-loaded-by-dyld)
  - [Display the shared libraries that object file uses](#display-the-shared-libraries-that-object-file-uses)
- [Etc](#etc)
  - [Display symbol table of an object file](#display-symbol-table-of-an-object-file)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Clang

### Generate human-readable LLVM IR from a file.


```bash
clang -S -emit-llvm testee.c -o testee.ll
```

### Generate LLVM Bitcode from a file.


```bash
clang -c -emit-llvm -o testee.bc testee.c
```

## lli

```bash
brew install llvm # installed to /usr/local/opt/llvm
```

### Execute Objective-C code compiled into LLVM Bitcode using LLVM JIT

```bash
/usr/local/opt/llvm/bin/lli -load=/System/Library/Frameworks/Foundation.framework/Versions/Current/Foundation jitobjc.bc
```

## Ninja

### Get compilation_database.json

```bash
mkdir build
cd build
cmake -G Ninja -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DLLVM_TARGETS_TO_BUILD="X86" ../llvm
ninja llvm-tblgen
cd ..
cp build/compile_commands.json ./
```

### Get list of commands required to build a target

```
cd LLVM/BuildNinja
ninja -t commands YourTargetName
```

## dyld

### Display libraries that are loaded by dyld

```bash
$ export DYLD_PRINT_LIBRARIES=1
$ rustc --version 2>&1
dyld: loaded: /Users/Stanislaw/.cargo/bin/rustc
dyld: loaded: /usr/lib/libcurl.4.dylib
dyld: loaded: /usr/lib/libz.1.dylib
...
```

### Display the shared libraries that object file uses

```bash
otool -L Distribution/iOS/MusicXML.framework/MusicXML 
Distribution/iOS/MusicXML.framework/MusicXML:
	@rpath/MusicXML.framework/MusicXML (compatibility version 1.0.0, current version 1.0.0)
	/usr/lib/libxml2.2.dylib (compatibility version 10.0.0, current version 10.9.0)
	/System/Library/Frameworks/Foundation.framework/Foundation (compatibility version 300.0.0, current version 1349.0.0)
	/usr/lib/libobjc.A.dylib (compatibility version 1.0.0, current version 228.0.0)
	/usr/lib/libSystem.dylib (compatibility version 1.0.0, current version 1238.0.0)
	/System/Library/Frameworks/CoreFoundation.framework/CoreFoundation (compatibility version 150.0.0, current version 1348.0.0)
```

## Etc

### Display symbol table of an object file

```bash
$ nm Distribution/iOS/MusicXML.framework/MusicXML 
0000000000000ec4 t +[MXMLConverter ConventionalParts]
0000000000008514 t +[RXMLElement elementFromHTMLData:]
00000000000083cd t +[RXMLElement elementFromHTMLFile:]
000000000000842e t +[RXMLElement elementFromHTMLFile:fileExtension:]
00000000000084b3 t +[RXMLElement elementFromHTMLFilePath:]
000000000000835d t +[RXMLElement elementFromHTMLString:encoding:]
0000000000008207 t +[RXMLElement elementFromURL:]
...
```

## Known issues

### Attempt to read debug information from a bitcode file compiled with Apple clang

```
warning: ignoring debug info with an invalid version (700000003) in /Users/stanislaw/Projects/LLVM/BuildXcode/projects/mull-project/unittests/Debug/fixtures/fixture_google_test_tester.bc
warning: ignoring debug info with an invalid version (700000003) in /Users/stanislaw/Projects/LLVM/BuildXcode/projects/mull-project/unittests/Debug/fixtures/fixture_google_test_testee.bc
```

Solution: compile files with custom clang e.g. `brew install llvm / /usr/local/opt/llvm/bin/clang`.
