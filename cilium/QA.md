#### Error of NativeEndian

Go not allow adding internal package in other dependency. And NativeEndian is in the internal package. We can use `binary.LittleEndian` or `binary.BigEndian` instead.

So, Some example may not work.