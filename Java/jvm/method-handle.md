### MethodHandle

[Method Handles in Java](https://www.baeldung.com/java-method-handles)

From a performance standpoint, the MethodHandles API can be much faster than the Reflection API since the access checks are made at creation time rather than at execution time. This difference gets amplified if a security manager is present, since member and class lookups are subject to additional checks.

[MethodHandle - What is it all about?(好文)](https://stackoverflow.com/questions/8823793/methodhandle-what-is-it-all-about)