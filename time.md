# Time

## NTP

Network Time Protocol: used to synchronize clocks

## Why can't we rely on the system clock in distributed systems?

- There's no guarantee that times are synchronized
- In the case of an [NTP](#ntp) synchronization, the system clock of one node can jump backward in time
