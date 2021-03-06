# How to Build NVIDIA's RAPIDS on Arch Linux on CUDA11
PKGBUILD's for building NVIDIA's RAPIDS (cuML/cuDF) on Arch Linux + CUDA11
a  procedure of building NVIDIA's [RAPIDS](https://rapids.ai/) under Arch Linux with CUDA environment on  '''Single Nvidia's GPU'''.

## Prerequisites
* Hardware

** Intel or AMD's CPU

** NVIDIA's Single GPU which has architecture whose type is Pascal / Volta / Turing / Ampere. (eg GeForce / TITAN / Tesla / Quadro)

* Software

** Arch Linux or its derivatives.

** AUR / yay required

** Build on CUDA11.1 (+CUDNN8 + NCCL)

* Disclaimer

Operation is not necessarily guaranteed. The author are not responsible for any damage of your environment by any of the operations described here.


## Order in build


1.  [protobuf-static](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/protobuf-static/PKGBUILD)
```
* Set ver 3.12.4 (do not build ver 3.13 or higher because its incompatible with cuDF).
* static library required for building treelite
```

2.  [arrow-cuda](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/arrow-0.17.1/PKGBUILD) / [python-pyarrow-cuda](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/python-pyarrow-0.17.1/PKGBUILD)
```
* Set ver 0.17.1 (do not build ver 1.0.0 or higher because its incompatible with cuDF).
* Build with "CUDA" (differencet from AUR).
* Apache Orc is required located in AUR.
* During compilation in Apache Flight, there is a patch so that gRPC-c++-plugin cab be easily integrated in Higher Version.
```

3.  [python-cmake-setuptools](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/python-cmake-setuptools/PKGBUILD)
```
* Build  Python Library for enabling to run cmake during "python setup.py build"
```
4.  [Cupy](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/cupy/PKGBUILD)
```
* An implementation of NumPy-compatible  array on CUDA presented by Preferred Networks.
* AUR provides the same package. However,  AUR only contains older ver (7.2.0). which does not support CUDA 11.0 , (current ver. is 8.0.0) 
*  See [github](https://github.com/cupy/cupy)
```

5.  [Openuxc](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/openucx) / [python-ucx-py](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/rapids-ucx-py/PKGBUILD)
```
* an optimized communication layer for Message Passing (MPI), PGAS/OpenSHMEM libraries and RPC/data-centric applications. and wrapper for python.
* See [OpenUCX's https://github.com/openucx/ucx)
* See [ucx-py's github](https://github.com/rapidsai/ucx-py)
```


6. [XGBoost](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/xgboost/PKGBUILD)

```
* an optimized distributed gradient boosting library 
* Builds Library and its Python-Wrapper
* See [github](https://github.com/dmlc/xgboost)
```

7. [treelite](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/treelite/PKGBUILD)

```
* a model compiler for efficient deployment of decision tree ensembles 
* Builds Library and its Python-Wrapper
* See [github](https://github.com/dmlc/treelite)
```


8  [RMM](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-rmm/PKGBUILD)
```
* RAPIDS Memory Manager provided by  Rapids.
* See [github](https://github.com/rapidsai/rmm)
* amend PKGBUILD and setup.py.patch to avoid missing libraries (cudart fmt...) (2020.8.21. 2300GMT)
```
9  [dask-cuda](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/rapids-dask-cuda/PKGBUILD)
```
* Utilities for CUDA and DASK.
* See [github](https://github.com/rapidsai/dask-cuda)
```
10  [nvtx](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/nvtx/PKGBUILD),  [python-nvtx](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/python-nvtx/PKGBUILD)
```
* NVIDIA Tool Extension Library 
```

11  [cuDF](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-cudf/PKGBUILD)
```
*  a GPU DataFrame taking place of pandas
* See [github](https://github.com/rapidsai/cudf)
```

12 [cuML](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-cuml/PKGBUILD)
```
*  A suite of libraries that implement machine learning algorithms presented by Rapids.
* See [github](https://github.com/rapidsai/cuml)
* amend PKGBUILD and setup.py.patch to avoid missing libraries (cublas cusuparse cusolver ...) (2020.9.19. 1400GMT)
```

13.  [libcypher-parse](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/libcypher-parser/PKGBUILD)
```
* required for building cuGraph
```


14.  [cuSignal](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/rapids-cusignal/PKGBUILD)
```
* GPU accelerated signal processing which may replace scipy signal?
* See [github](https://github.com/rapidsai/cusignal)
```

15. [gdal, python-gdal](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/gdal/PKGBUILD)

```
* a bug-fix patch required to build gdal under poppler upgraded to 20.08.1
```


16. [rapids-cuSpatial](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/rapids-cuspatial/PKGBUILD)
```
* a library for handling spatial and trajectory data.
* gdal and python-gdal are needed
* See [github](https://github.com/rapidsai/cuspatial)
```

17. [rapids-cuGraph](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/rapids-cugraph/PKGBUILD)
```
* a library for Graph Analytics
* See [github](https://github.com/rapidsai/cugraph)
```

18. [rapids-cuxfilter](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/rapids-cuxfilter/PKGBUILD)
```
* cuxfilter ( ku-cross-filter ) is a RAPIDS framework to connect web visualizations to GPU accelerated crossfiltering.
* See [github](https://github.com/rapidsai/cuxfilter)
```


