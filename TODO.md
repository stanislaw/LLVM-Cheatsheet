The standard way to see what the interpreter is for a given binary is:

```
$ readelf -l test | grep interpreter
      [Requesting program interpreter: /lib/ld64.so.1]
```
