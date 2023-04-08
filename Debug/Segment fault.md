A segfault occurs when you try to access a piece of memory that "does not belong to you." There are several things that can cause a segfault including

1.  Accessing an array out of bounds. Note that accessing an array out of bounds will not always lead to a segfault. The index at which a segfault will occur is somewhat unpredictable.
2.  Derefrencing a null pointer.
3.  Accessing a pointer that has been `free`'d (`free` is not in the scope of this lab).
4.  Attempting to write to read-only memory. For example, strings created with the following syntax are read only. This means that you cannot alter the value of the string after you have created it. In other words, it is immutable.