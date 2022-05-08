# Time

## NTP

Network Time Protocol: used to synchronize clocks

## Why can't we rely on system clock in distributed systems?

- There's no guarantee that time are synchronized
- In case of an NTP synchronization, the system clock of one node can jump backward in time
