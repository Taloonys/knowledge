> IPC is Inter-Process-Communication group of technologies that (obviously) provides communication between processes.

# Shared Memory
> Shared memory is located in RAM, any CPU or process can attract with it in purpose to avoid data-copy. It's a fastest IPC method in PC. <br>
> Also named as `SMA` (Shared Memory architecture) or `shm` sometimes.

# Pipes
> Concept refers to it's name, but it also uses Shared Memory, Process1 -> Pipe -> Process2. <br>
> There are 2 types of pipes:
> - named -> determined by OS
> - anonymous -> usually created for somekind of process, like command `cat` in Unix systems and etc.
