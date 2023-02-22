\_\_NOTOC\_\_

SPM requirements in terms or hardware are closely related to those of
MATLAB, see:

- [MATLAB System Requirements](http://www.mathworks.com/support/sysreq/)
- [Choosing Hardware for Use with
  MATLAB](http://www.mathworks.com/products/matlab/choosing_hardware.html)
- [Computer hardware which best optimizes the performance of
  MATLAB](http://www.mathworks.com/support/solutions/en/data/1-18C2A/?solution=1-18C2A)

# Processor

No particular requirements, apart from those for MATLAB. The faster the
better but little memory or a slow hard disk can become a bottleneck so
make sure to have a coherent hardware installation.

## Mono-/multi-processors

Recent MATLABs support implicit multiprocessing allowing to run multiple
threads on a single machine without any change to the MATLAB code
itself: this requires a multiple CPU (multiprocessor or multicore)
system. The gain in compute time with SPM is not dramatic though.

See:

- <http://www.mathworks.com/access/helpdesk/help/techdoc/matlab_prog/brdo29n-1.html>
- <http://www.mathworks.com/access/helpdesk/help/techdoc/ref/maxnumcompthreads.html>

If you run many MATLAB sessions in parallel to manually distribute your
SPM processings, it is recommended to set the number of computational
threads to one.

Some effort is currently done to provide a parallel version of SPM that
could automatically distribute tasks on a multicore/multiprocessor
machine or a cluster.

## 32/64 bit

Major Operating Systems, MATLAB and SPM support 64bit architectures and
are nowadays recommended.

# Memory

The more RAM you have the better but SPM should accommodate with any
amount of memory you have (provided there is enough for MATLAB itself).
Remember that a 32 bit Operating System might not be able to make use of
the full amount of memory.

For more information, see:

- <http://www.mathworks.com/support/tech-notes/1100/1106.html>
- <http://www.mathworks.com/support/tech-notes/1100/1107.html>

# Hard Disk

Neuroimaging datasets can be quite large so a slow [hard
disk](w:Hard_disk_drive "wikilink") can be a bottleneck for SPM
processing. (I.e. if you\'ve got the choice, choose the hard disk with
the highest RPM. For even more performance, use [ multiple hard drives
in a RAID1](The_Computer_Revolution/Hardware/RAID "wikilink") or other
RAID setup). Concerning the capacity, it\'s also obviously the more the
better.

## Reviews of hard disks

<http://www.storagereview.com>

## Network File System

It is certainly possible to store your datasets on an NFS server. If the
server is located in another building or section of your building, make
sure the entire connection to your SPM processing workstation is at
least 100MB/s.

# Graphic Card

At current time little use is made of the GPU in SPM so the graphic card
should not have any impact in SPM processing.
