# LLVM Cheatsheet

### See the libraries that are loaded by dyld

```bash
export DYLD_PRINT_LIBRARIES=1
rustc --version 2>&1 | grep libstd
```

