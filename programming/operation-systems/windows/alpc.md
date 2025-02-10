# Shared memory 
> It's a dedicated part of RAM, managed by OS, that allow processes to exchange data between them.
> * There are no OS blocking operations for that (for Windows mostly)
> * It's more like publishing data, it's storage is kinda huge
> * Access is allowed by any number of processes
> * Access should be closed after usage, but it's not really always required.

# Pipes
> Dedicated part of Shared memory. Could be "constructed" only between 1 pair of processes
> * It has OS-built-in blocking mechanics
> * Only 2 process can be "bridged"
> * Doesn't require to be closed manually
> * It's buffer size is fixed (about 4kB for Linux prb, but it could me changed). So it mostly used for small data / messages transmitting.

- *link: https://learn.microsoft.com/en-us/windows/win32/ipc/pipes*
- *link: https://csandker.io/2021/01/10/Offensive-Windows-IPC-1-NamedPipes.html*

# RPC
> `Remote Procedure Call` but doesn't meant to be in server-client contextes, user1-user1 or even process1-process1 are allowed.
*link: https://csandker.io/2021/02/21/Offensive-Windows-IPC-2-RPC.html*

# ALPC (and LPC)
> `Local Procedure Call` - is a synchronous message deliver mechanic. <br>
> `Advanced LPC` (sometimes 'A' stands for Asynchronous) - is a kinda fast asynchronous message deliver mechanic, that uses RPC and shm in some scenarios (for example if msg is bigger than 4kB). <br>
*link: https://csandker.io/2022/05/24/Offensive-Windows-IPC-3-ALPC.html*
