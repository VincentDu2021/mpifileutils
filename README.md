# mpiFileUtils
mpiFileUtils provides both a library called [libmfu](src/common/README.md) and a suite of MPI-based tools to manage large datasets, which may vary from large directory trees to large files. High-performance computing users often generate large datasets with parallel applications that run with many processes (millions in some cases). However those users are then stuck with single-process tools like cp and rm to manage their datasets. This suite provides MPI-based tools to handle typical jobs like copy, remove, and compare for such datasets, providing speedups of up to 20-30x.  It also provides a library that simplifies the creation of new tools or can be used in applications.

Documentation is available on [ReadTheDocs](http://mpifileutils.readthedocs.io).

## DAOS Support

mpiFileUtils supports a DAOS backend for dcp, dsync, and dcmp. Custom serialization and deserialization for DAOS containers to and from a POSIX filesystem is provided with daos-serialize and daos-deserialize. Details and usage examples are provided in [DAOS Support](DAOS-Support.md).

## Contributors
We welcome contributions to the project.  For details on how to help, see our [Contributor Guide](CONTRIBUTING.md)

### Copyrights

Copyright (c) 2013-2015, Lawrence Livermore National Security, LLC.
  Produced at the Lawrence Livermore National Laboratory
  CODE-673838

Copyright (c) 2006-2007,2011-2015, Los Alamos National Security, LLC.
  (LA-CC-06-077, LA-CC-10-066, LA-CC-14-046)

Copyright (2013-2015) UT-Battelle, LLC under Contract No.
DE-AC05-00OR22725 with the Department of Energy.

Copyright (c) 2015, DataDirect Networks, Inc.

All rights reserved.

## Build Status
The current status of the mpiFileUtils master branch is [![Build Status](https://travis-ci.org/hpc/mpifileutils.png?branch=master)](https://travis-ci.org/hpc/mpifileutils).


## Build notes on Ubuntu 20.24 (2024)
There are some missing dependencies.
These 3 needs to be built from source:

```
libattr
libbz2
libcap
```

The 3 source tarballs are here committed, which can be extracted and build the libraries. The libbz2 lib needs to run:
```
make -f Makefile-libbz2_so

```
so that -fPIC flag is included.

mpich and libarchive can be installed directly with apt util:
```
sudo apt install mpich
sudo apt install libarchive-dev

# change the install destination
cmake -DCMAKE_INSTALL_PREFIX=$HOME/.local/ ..
```

## MPI run sammple:

On a single host with 112 CPU, seems 96 processes can saturate :

```
vdu:~/tools/mpifileutils/releases/mpifileutils-v0.11.1/build/> mpirun -np 96 --bind-to socket dwalk /mnt/weka/weka-csi/datasets/research/LDM3D_data
[2024-02-01T22:33:39] Walking /mnt/weka/weka-csi/datasets/research/LDM3D_data
[2024-02-01T22:33:49] Walked 5039090 items in 10.347 secs (487006.019 items/sec) ...
[2024-02-01T22:34:00] Walked 10059559 items in 20.826 secs (483033.089 items/sec) ...
[2024-02-01T22:34:10] Walked 15171116 items in 30.834 secs (492019.448 items/sec) ...
[2024-02-01T22:34:20] Walked 20188250 items in 40.867 secs (493997.150 items/sec) ...
[2024-02-01T22:34:30] Walked 25201954 items in 51.097 secs (493213.099 items/sec) ...
[2024-02-01T22:34:40] Walked 30253807 items in 60.870 secs (497021.972 items/sec) ...
[2024-02-01T22:34:50] Walked 35276096 items in 70.942 secs (497251.346 items/sec) ...
[2024-02-01T22:35:00] Walked 40309355 items in 81.217 secs (496317.260 items/sec) ...
[2024-02-01T22:35:10] Walked 45304199 items in 91.217 secs (496664.039 items/sec) ...
[2024-02-01T22:35:20] Walked 50377567 items in 101.223 secs (497687.565 items/sec) ...
[2024-02-01T22:35:30] Walked 55470785 items in 111.221 secs (498741.694 items/sec) ...
[2024-02-01T22:35:40] Walked 60620850 items in 121.225 secs (500070.659 items/sec) ...
[2024-02-01T22:35:51] Walked 65768306 items in 131.458 secs (500299.356 items/sec) ...
[2024-02-01T22:36:00] Walked 70984976 items in 141.224 secs (502642.535 items/sec) ...
[2024-02-01T22:36:11] Walked 76244301 items in 151.639 secs (502801.904 items/sec) ...
[2024-02-01T22:36:21] Walked 81821985 items in 161.906 secs (505366.536 items/sec) ...
[2024-02-01T22:36:32] Walked 87237729 items in 172.537 secs (505618.658 items/sec) ...
[2024-02-01T22:36:35] [79] [/home/net/vdu/tools/mpifileutils/releases/mpifileutils-v0.11.1/mpifileutils/src/common/mfu_flist_walk.c:476] ERROR: Failed to open directory with opendir: ...
[2024-02-01T22:36:42] Walked 92224335 items in 182.474 secs (505409.916 items/sec) ...
[2024-02-01T22:36:52] Walked 96525011 items in 192.442 secs (501580.430 items/sec) ...
[2024-02-01T22:36:54] Walked 97517366 items in 194.541 secs (501268.257 items/sec) ...
[2024-02-01T22:36:54] Walked 97517366 items in 194.972 seconds (500161.034 items/sec)
[2024-02-01T22:36:54] Items: 97517366
[2024-02-01T22:36:54]   Directories: 9628
[2024-02-01T22:36:54]   Files: 97507655
[2024-02-01T22:36:54]   Links: 83
[2024-02-01T22:36:54] Data: 9.117 TiB (100.395 KiB per file)

```
