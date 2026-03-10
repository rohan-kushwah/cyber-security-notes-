memory management commands are indispensable for:

- **Optimizing system performance** by balancing memory use.
- **Troubleshooting memory-related issues** like memory leaks or high memory usage.
- **Allocating and controlling memory resources** effectively to prevent crashes and overloads.
- **Ensuring system stability**, especially under high load or multi-user environments.

# Memory Management and System Performance Commands

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Memory-management-commands.md#memory-management-and-system-performance-commands)

## 1. Memory Management Commands

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Memory-management-commands.md#1-memory-management-commands)

1. **Free Command:**
    - `free -h`: Display memory usage in human-readable format (KB, MB, GB).
        
        ```shell
        free -h
        ```
        
    - `free -g`: Display memory usage in gigabytes.
        
        ```shell
        free -g
        ```
        
    - `free -m`: Display memory usage in megabytes.
        
        ```shell
        free -m
        ```
        
    - `free -k`: Display memory usage in kilobytes.
        
        ```shell
        free -k
        ```
        

---

## 2. System Performance Commands

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Memory-management-commands.md#2-system-performance-commands)

1. **Vmstat Command:** virtual  machine status =
    
    - `vmstat`: Display basic system performance stats (memory, CPU, processes, etc.).
        
        ```shell
        vmstat
        ```
        
    - `vmstat -a`: Show active memory and swap usage.
        
        ```shell
        vmstat -a
        ```
        
    - `vmstat -d`: Display disk statistics.
        
        ```shell
        vmstat -d
        ```
        
    - `vmstat -s`: Display summary of memory, swap, and disk statistics.
        
        ```shell
        vmstat -s
        ```
        
    - `vmstat 4 5`: Display performance stats every 4 seconds for 5 iterations.
          har char second pe ek bar aise 5 bar dikhana hai
        ```shell
        vmstat 4 5
        ```
        
    - `vmstat -s m 4 5`: Display memory statistics every 4 seconds for 5 iterations.
        
        ```shell
        vmstat -s m 4 5
        ```
        
2. **Iostat Command:**
    
    - `iostat`: Display input/output statistics for devices and partitions.
        
        ```shell
        iostat
        ```
        
    - Install `iostat` if not installed:
        
        ```shell
        yum install sysstat
        ```
        
    - `iostat 4 5`: Display I/O statistics every 4 seconds for 5 iterations.
        
        ```shell
        iostat 4 5
        ```
        

---

1. **Listing Open Files with `lsof`**
    
    - Install `lsof` Command (if not installed):
        
        ```shell
        yum install lsof
        ```
        
    - To list open files by the user `root`:
        
        ```shell
        lsof -u root
        ```
        
    - To list network files (all types of network connections):
        
        ```shell
        lsof -N -i
        ```
        
    - To list open UDP network connections:
        
        ```shell
        lsof -n -i UDP
        ```
        
    - To list open TCP network connections:
        
        ```shell
        lsof -n -i TCP
        ```
        
    - To list open TCP port 22 (usually SSH):
        
        ```shell
        lsof -n -i TCP:22
        ```
        
    - To list open UDP ports 22, 443, and 90:
        
        ```shell
        lsof -n -i UDP:22,443,90
        ```
        
    - To list open UDP ports in the range 80-220:
        
        ```shell
        lsof -n -i UDP:80-220
        ```
        
    - To list open files for process ID 6029:
        
        ```shell
        lsof -n -p 6029
        ```
        
    - To list open IPv4 network connections:
        
        ```shell
        lsof -n -i 4
        ```
        
    - To list open IPv6 network connections:
        
        ```shell
        lsof -n -i 6
        ```
        
    - To get process IDs (PIDs) of files opened by the user `Armour`:
        
        ```shell
        lsof -t -u Armour
        ```
        

---

## 3. Killing Processes Using `kill`

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Memory-management-commands.md#3-killing-processes-using-kill)

- To kill processes opened by the user `Armour` using the process IDs (PIDs) retrieved from `lsof`:
    
    ```shell
    kill -9 $(lsof -t -u Armour)
    ```