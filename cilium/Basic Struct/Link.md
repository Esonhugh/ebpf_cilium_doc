
link.Link 
```go
type Link interface {
	// Replace the current program with a new program.
	//
	// Passing a nil program is an error. May return an error wrapping ErrNotSupported.
	Update(*ebpf.Program error)

	// Persist a link by pinning it into a bpffs.
	//
	// May return an error wrapping ErrNotSupported.
	Pin(string) error

	// Undo a previous call to Pin.
	//
	// May return an error wrapping ErrNotSupported.
	Unpin() error
	// Close frees resources.
	//
	// The link will be broken unless it has been successfully pinned.
	// A link may continue past the lifetime of the process if Close is
	// not called.
	Close() error

	// Info returns metadata on a link.
	//
	// May return an error wrapping ErrNotSupported.
	Info() (*[Info], error)
	// contains filtered or unexported methods
}
```

  
Link is an interface for ebpf program. We can control the ebpf program behavior. 

### Attach

There are many exported `AttachXXX(AttachXXXOpts) `function in link package. The Link will be generated when you "Load" your ebpf program from bpf-fs or "Attach" the ebpf tracing point in kernel.

If you not pin that Link, your program will be removed  by kernel when your usermode program shutdown. Automatically Detach.

### Pin for persist

Using `Link.Pin(location)` to pin the Link in bpf fs. The ebpf program won't be removed by kernel when your usermode program shutdown.

> Same, Unpin() is for unpin the link.

The `LoadPinnedXXXX()` will load the Link from bpf fs.

The Pinning make the usermode program can detach.

