> IPC is Inter-Process-Communication group of technologies that (obviously) provides communication between processes.

# Shared Memory
> It's a dedicated part of RAM, managed by OS, that allow processes to exchange data between them.
> * There are no OS blocking operations for that (for Windows mostly)
> * It's more like publishing data, it's storage is kinda huge
> * Access is allowed by any number of processes
> * Access should be closed after usage, but it's not really always required.


# Pipes
 Dedicated part of Shared memory. Could be "constructed" only between 1 pair of processes
> * It has OS-built-in blocking mechanics
> * Only 2 process can be "bridged"
> * Doesn't require to be closed manually
> * It's buffer size is fixed (about 4kB for Linux prb, but it could me changed). So it mostly used for small data / messages transmitting.

- *link: https://learn.microsoft.com/en-us/windows/win32/ipc/pipes*
- *link: https://csandker.io/2021/01/10/Offensive-Windows-IPC-1-NamedPipes.html*

> There are 2 types of pipes:
> - named -> determined by OS
> - anonymous -> usually created for somekind of process, like command `cat` in Unix systems and etc.
# Windows
 * LPC / ALPC
# Linux 
* D-Bus
