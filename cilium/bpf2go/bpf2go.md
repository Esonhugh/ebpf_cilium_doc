bpf2go is the commandline to for go generate. 

### General

This CLI tool can analyze the compiled ebpf program object, then generate the Golang struct and Golang function definition into .go file that you handle the ebpf program with.

### Basic Usage

Invoke the program using go generate:  
  
```go
//go:generate go run github.com/cilium/ebpf/cmd/bpf2go foo path/to/src.c -- -I/path/to/include
```
 
This will emit `foo_bpfel.go` and `foo_bpfeb.go`, with types using `foo`  
as a stem. The two files contain compiled BPF for little and big  
endian systems, respectively.

> Uppercase foo like `Foo`. That will make the struct and function in generated go file exported.

### CFLAGS

You can use environment variables to affect all bpf2go invocations  
across a project, e.g. to set specific C flags:  
  
```go
//go:generate go run github.com/cilium/ebpf/cmd/bpf2go -cflags "$BPF_CFLAGS" foo path/to/src.c  
```

By exporting `$BPF_CFLAGS` from your build system you can then control  
all builds from a single location.

### Exporting Custom Struct

bpf2go may not export the unused struct because the Clang will optimize it.

If you defined custom type, please using unused attribute.

```c
struct s {  
   char a;  
   char b;  
};  
  
struct s *unused_s __attribute__((unused));
```

and use `-type <name>` option in go generate comment.

```go
//go:generate go run github.com/cilium/ebpf/cmd/bpf2go foo path/to/src.c -- -I/path/to/include -type s
```