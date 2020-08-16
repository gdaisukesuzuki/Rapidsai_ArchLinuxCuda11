# Rapidsai_ArchLinuxCuda11
PKGBUILD's for building rapidsai (cuML/cuDF) on Arch Linux + CUDA11
a  procedure of building NVIDIA's [RAPIDS](https://rapids.ai/) under Arch Linux with CUDA environment on  '''Single Nvidia's GPU'''.

## Prerequisites
* Hardware

** Intel or AMD's CPU

** NVIDIA's Single GPU which has architecture whose type is Pascal / Volta / Turing. (eg GeForce / TITAN / Tesla / Quadro)

* Software

** Arch Linux or its derivatives.

** AUR / yay required

** Build on CUDA11 (+CUDNN8)

* Disclaimer

Operation is not necessarily guaranteed. The author are not responsible for any damage of your environment by any of the operations described here.


## Order in build


1.  [protobuf](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/protobuf/PKGBUILD)
```
* Set ver 3.12.3 (do not build ver 3.13 or higher).
```

2.  [arrow-cuda](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/arrow-0.17.1/PKGBUILD) / [python-pyarrow-cuda](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/python-pyarrow-0.17.1/PKGBUILD)
```
* Set ver 0.17.1 (do not build ver 1.0.0 or higher).
* Build with "CUDA" (differencet from AUR).
* Apache Orc is required located in AUR.
* During compilation in Apache Flight, there is a patch so that gRPC-c++-plugin cab be easily integrated in Higher Version.
```

3.  [python-cmake-setuptools](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/tree/master/python-cmake-setuptools/PKGBUILD)
```
* Build  Python Library for enabling to run cmake during "python setup.py build"
```
4.  [Cupy](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/cupy/PKGBUILD)
```
* An implementation of NumPy-compatible  array on CUDA presented by Preferred Networks.
* AUR provides the same package. However,  AUR only contains older ver (7.2.0). which does not support CUDA 10.2 , (current ver. is 7.4.0) 
*  See [github](https://github.com/cupy/cupy)
```

5.  [python-LLVMlite](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/blob/master/python-llvmlite/PKGBUILD) / [python-Numba](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/blob/master/python-numba/PKGBUILD)
```
* An open source JIT compiler that translates a subset of Python and NumPy code into fast machine code.
* These PKGBUILDs provide llvmlite 0.33.0rc1 and Numba 0.50.0rc1, respectively.
*  See githubs. [llvmlite](https://github.com/numba/llvmlite) and [numba](https://github.com/numba/numba)
```


6.  [RMM](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-rmm/PKGBUILD)
```
* RAPIDS Memory Manager provided by  Rapids.
* See [github](https://github.com/rapidsai/rmm)
```

7.  [cuDF](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-cudf/PKGBUILD)
```
*  a GPU DataFrame taking place of pandas
* See [github](https://github.com/rapidsai/cudf)
```

8.  [cuML](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/tree/master/rapids-cuml/PKGBUILD)
```
*  A suite of libraries that implement machine learning algorithms presented by Rapids.
* See [github](https://github.com/rapidsai/cuml)
```

3. [treelite](https://github.com/gdaisukesuzuki/Rapidsai_ArchLinuxCuda11/blob/master/treelite/PKGBUILD)

```
* a model compiler for efficient deployment of decision tree ensembles 
* Builds Library and its Python-Wrapper
```

4. [Benchmark](https://github.com/gdaisukesuzuki/PKGBUILD_Rapids/blob/master/benchmark/cuml_benchmarks.ipynb)

```
* Benchmark Result 
```
See [URL](https://github.com/rapidsai/cuml/blob/branch-0.14/notebooks/tools/cuml_benchmarks.ipynb)

