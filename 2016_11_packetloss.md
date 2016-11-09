# Packet loss issue

The first part of the AA-Alert pipeline roughly consists of the following parts:

 1. the telescopes
 2. FPGAs doing preprocessing and beam forming
 3. TCP/IP network to ARTS cluster
 4. Ringbuffer
 5. Pipeline (CPU/GPU for FRB and pulsar search)

where the NLeSC project is concerned with parts 4 and 5.
At commisioning there were some issues, possibly related to the network:

 * Packets would not arrive in order. This was solved by using a single network address per band.
 * upto 35%% of packets were lost in steps 3 and 4, mostly at the same bands.
 * CPU and memory usage were high, total load of around 11

## Attempted solutions

### code optimization

The code copying the packets from the network port to the ringbuffer is [fill_ringbuffer](https://github.com/AA-ALERT/ringbuffer).
Some attempts had been made to deal with the packet loss issue and the packet overtaking issue.
The code had become had to read and hence the problem difficult to diagnose.
A rewrite allowed me to remove one memory copy from the inner loop of the program; but this did not lead to significant improvements.

### compiler tuning

Background: 

* [GCC performance hints](https://software.intel.com/en-us/blogs/2012/09/26/gcc-x86-performance-hints)
* [GCC Optimization](https://wiki.gentoo.org/wiki/GCC_optimization#Optimizing)

All code was compiled using `-O3`, a recompile using `-O3 -march=native` did not improve much.

### memory optimization

Leon noticed that, when commisioning, the fill ringbuffer instances were using a large amount of memory.
The PSRDARA ringbuffer size was set very high (100s of MBs).
Reducing the buffer size to a much lower value (about 4 MB per instance) significantly decreased memory use and packet loss.

### hardware tuning

Increasing the NIC ringbuffer size to their maximum values helped a lot:
```
  $ ethtool -G p4p1 rx 8192
  $ ethtool -G p4p2 rx 8192
```

## Current status

Current packet loss is below 1% per band. This is good, but other use cases will require even higher bandwith. Also, we are not doing much with the data yet. When the full pipeline is operational packet loss might go up again.

### further improvements

 * [update network card driver](http://www.mellanox.com/page/products_dyn?product_family=27)
 * [further hardware tuning](http://www.mellanox.com/related-docs/prod_software/Performance_Tuning_Guide_for_Mellanox_Network_Adapters.pdf)
 * Recompile all software with proper optimizations, only `fill_ringbuffer` and `FFTW3` have been tuned. (for FFTW3 a wisdom file has also been created)
