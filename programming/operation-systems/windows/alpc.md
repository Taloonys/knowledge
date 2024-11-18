# Pipes
> Like it's name.. but using shared memory.
*link: https://csandker.io/2021/01/10/Offensive-Windows-IPC-1-NamedPipes.html*

# RPC
> `Remote Procedure Call` but doesn't meant to be in server-client, user1-user1 or even process1-process1 contextes only.
*link: https://csandker.io/2021/02/21/Offensive-Windows-IPC-2-RPC.html*

# ALPC (and LPC)
> `Local Procedure Call` - is a synchronous message deliver mechanic.
> `Advanced LPC` (sometimes 'A' stands for Asynchronous) - is a kinda fast asynchronous message deliver mechanic, that uses RPC and shm in some scenarios (for example if msg is bigger than 4kB).
*link: https://csandker.io/2022/05/24/Offensive-Windows-IPC-3-ALPC.html*
