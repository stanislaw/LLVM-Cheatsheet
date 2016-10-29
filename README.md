# LLVM Cheatsheet

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [See the libraries that are loaded by dyld](#see-the-libraries-that-are-loaded-by-dyld)
- [See the shared libraries that object file uses](#see-the-shared-libraries-that-object-file-uses)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### See the libraries that are loaded by dyld

```bash
$ export DYLD_PRINT_LIBRARIES=1
$ rustc --version 2>&1 | grep libstd
dyld: loaded: /Users/Stanislaw/.cargo/bin/rustc
dyld: loaded: /usr/lib/libcurl.4.dylib
dyld: loaded: /usr/lib/libz.1.dylib
...
```

### See the shared libraries that object file uses

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

