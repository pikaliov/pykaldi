
---
**NOTE**

PyKaldi is still at alpha stage. Some features might not work yet.

---

# PyKaldi

PyKaldi is a Python wrapper for [Kaldi](http://kaldi-asr.org) exposing nearly
all of Kaldi's C++ API to Python code. It aims to bridge the gap between Kaldi
and all the nice things Python has to offer including its mature ecosystem of
high quality software for scientific computing, machine learning, interactive
data exploration and visualization.

PyKaldi is more than a collection of bindings into Kaldi libraries. It is a
scripting layer providing first class support for essential Kaldi and
[OpenFst](http://www.openfst.org) types in Python. PyKaldi vector and matrix
types are tightly integrated with [NumPy](http://www.numpy.org). They can be
seamlessly converted to NumPy arrays and vice versa without copying the
underlying memory buffers. PyKaldi FST types, including Kaldi style lattices,
are first class citizens in Python. The API for the user facing FST types and
operations is almost entirely defined in Python mimicking the API exposed by
[pywrapfst](http://www.openfst.org/twiki/bin/view/FST/PythonExtension), the
official Python wrapper for OpenFst. You can read more about the implementation of PyKaldi in our [ICASSP 2018 paper](https://github.com/pykaldi/pykaldi/blob/77758546da394fea2d8125f096babab04bdb081d/docs/pykaldi-python-wrapper.pdf).

## Features

* Near-complete coverage of Kaldi's C++ API

* First class support for Kaldi and OpenFst types in Python

* Extensible design

* Open license

* Extensive documentation

* Thorough testing

* Example scripts

* Support for both Python 2.7 and 3.5+


## Overview

- [About PyKaldi](#about-pykaldi)
  - [Coverage Status](#coverage-status)
- [Getting Started](#getting-started)
- [Installation](#installation)
  - [Docker Image](#docker-image)
  - [From Source](#from-source)
- [Contributing](#contributing)


## About PyKaldi

PyKaldi harnesses the power of [CLIF](https://github.com/google/clif) to wrap
Kaldi C++ libraries using simple API descriptions. The CPython extension modules
generated by CLIF can be imported in Python to interact with Kaldi. While CLIF
is great for exposing the existing C++ API in Python, the wrappers do not always
expose a "Pythonic" API that is easy to use from Python. To address this
concern, PyKaldi extends the raw CLIF wrappers in Python (and sometimes in C++)
to provide a more "Pythonic" API. Below figure illustrates where PyKaldi fits in
the Kaldi software architecture.

<p align="center">
<img src="docs/pykaldi.png" alt="Kaldi Software Architecture" width="400">
</p>

PyKaldi has a modular design which makes it easy to maintain and extend. Source
files are organized in a directory tree that is a replica of the Kaldi source
tree. Each directory defines a subpackage and contains only the wrapper code
written for the associated Kaldi library. The wrapper code consists of:

* CLIF C++ API descriptions defining the types and functions to be wrapped and
  their Python API,

* C++ headers defining the shims for Kaldi code that is not compliant with the
  Google C++ style expected by CLIF,

* Python modules grouping together related extension modules generated with CLIF
  and extending the raw CLIF wrappers to provide a more "Pythonic" API.

### Coverage Status

The following table shows the status of each PyKaldi package along the following
dimensions:

* __Wrapped?__: If there are enough .clif files to make the package usable in
  Python.
* __Pythonic?__: If the package API has a "Pythonic" look-and-feel.
* __Documentation?__: If there is documentation beyond what is automatically
  generated by CLIF. Single checkmark indicates that there is not much additional
  documentation (if any). Three checkmarks indicates that package documentation
  is complete (or near complete).
* __Tests?__: If there are tests for the package.

| Package    | Wrapped? | Pythonic? | Documentation?             | Tests?   |
| :--------: | :------: | :-------: | :------------------------: | :------: |
| base       | &#10004; | &#10004;  | &#10004; &#10004;          | &#10004; |
| chain      | &#10004; |           | &#10004;                   |          |
| cudamatrix | &#10004; |           | &#10004;                   | &#10004; |
| decoder    | &#10004; | &#10004;  | &#10004; &#10004; &#10004; |          |
| feat       | &#10004; | &#10004;  | &#10004;                   |          |
| fstext     | &#10004; | &#10004;  | &#10004; &#10004; &#10004; |          |
| gmm        | &#10004; | &#10004;  | &#10004; &#10004;          | &#10004; |
| hmm        | &#10004; | &#10004;  | &#10004;                   | &#10004; |
| itf        | &#10004; |           | &#10004;                   |          |
| ivector    | &#10004; |           | &#10004;                   |          |
| kws        | &#10004; | &#10004;  | &#10004; &#10004; &#10004; |          |
| lat        | &#10004; |           | &#10004; &#10004; &#10004; |          |
| lm         | &#10004; |           | &#10004;                   |          |
| matrix     | &#10004; | &#10004;  | &#10004; &#10004; &#10004; | &#10004; |
| nnet       |          |           |                            |          |
| nnet2      |          |           |                            |          |
| nnet3      | &#10004; |           | &#10004;                   | &#10004; |
| online     |          |           |                            |          |
| online2    | &#10004; |           | &#10004;                   |          |
| rnnlm      | &#10004; | &#10004;  | &#10004; &#10004; &#10004; |          |
| sgmm2      | &#10004; |           | &#10004;                   |          |
| tfrnnlm    | &#10004; |           | &#10004; &#10004;          |          |
| transform  | &#10004; | &#10004;  | &#10004;                   |          |
| tree       | &#10004; |           | &#10004;                   |          |
| util       | &#10004; | &#10004;  | &#10004; &#10004; &#10004; | &#10004; |

## Getting Started

Some places to help you get started:

* [PyKaldi Documentation](https://pykaldi.github.io)
* [Walkthrough Example](https://github.com/pykaldi/pykaldi/tree/master/examples/walkthrough.md)
* [Kaldi binaries re-implemented using PyKaldi](https://github.com/pykaldi/pykaldi/tree/master/examples)


## Installation

### Docker Image

We provide a `Dockerfile` in the `docs` directory to build a new image.

```bash
cd pykaldi/docker
docker build -t pykaldi .
```

Alternatively, a pre-built image can be downloaded from dockerhub.

```bash
docker login
docker pull vrmpx/pykaldi
```

After building/downloading the image, you can run it in interactive mode.

```bash
sudo docker run -it pykaldi
```

### From Source

To install PyKaldi from CLIF source files, first we need to install all of its
dependencies. In the following, we provide instructions for installing PyKaldi
and all of its dependencies on Ubuntu 16.04.

#### Standard Dependencies

```bash
sudo apt-get install git autoconf automake libtool curl make cmake g++ unzip \
  virtualenv libatlas3-base wget zlib1g-dev subversion pkg-config
```

#### Protobuf

CLIF depends on [Google Protobuf](https://github.com/google/protobuf.git) v3.2
or later. Both the C++ library and the Python package must be installed.

```bash
git clone https://github.com/google/protobuf.git
cd protobuf
./autogen.sh
./configure && make -j4
sudo make install
sudo ldconfig
cd python
python setup.py build
sudo python setup.py install
```

#### CLIF

To streamline PyKaldi development, we made some changes to CLIF codebase. We
are hoping to upstream these changes over time. In the meantime we provide a
[PyKaldi compatible fork of CLIF](https://github.com/pykaldi/clif/tree/pykaldi).
Run the following for a compatible CLIF installation. Don't forget to check out
the pykaldi branch.

```bash
git clone -b pykaldi https://github.com/pykaldi/clif.git
cd clif
./INSTALL.sh  # This will install CLIF under $HOME/opt
```

Note that if there is more than one Python version installed (e.g., Python 2.7
and 3.6) cmake may not be able to find the correct python libraries. To help
cmake use the correct Python, add the following options to the cmake command
inside INSTALL.sh (make sure to substitute the correct paths for your system):

```bash
cmake ... \
        -DPYTHON_INCLUDE_DIR="/usr/include/python2.7" \
        -DPYTHON_LIBRARY="/usr/lib/x86_64-linux-gnu/libpython2.7.so" \
        -DPYTHON_EXECUTABLE="/usr/bin/python2.7" \
        "${CMAKE_G_FLAGS[@]}" "$LLVM_DIR/llvm"
```

#### Kaldi

To comply with CLIF requirements we had to make some changes to Kaldi codebase.
We are hoping to upstream these changes over time. In the meantime we provide a
[PyKaldi compatible fork of Kaldi](https://github.com/pykaldi/kaldi/tree/pykaldi).
Run the following commands for a compatible Kaldi installation. Don't forget to
check out the pykaldi branch.

```bash
git clone -b pykaldi https://github.com/pykaldi/kaldi.git
cd kaldi/tools
./extras/check_dependencies.sh && make -j4
cd ../src
./configure --shared && make clean -j4 && make depend -j4 && make -j4
```

#### PyKaldi

Set the following environment variables.

```bash
export KALDI_DIR=<directory where Kaldi is installed, e.g. "$HOME"/kaldi>
export CLIF_DIR=<directory where CLIF is installed, e.g. "$HOME"/opt/clif>

# This is needed for finding a compatible limits.h on some systems.
CLANG_RESOURCE_DIR=$(echo '#include <limits.h>' | \
                     "$CLIF_DIR"/clang/bin/clang -xc -v - 2>&1 | \
                     tr ' ' '\n' | grep -A1 resource-dir | tail -1)
export CLIF_CXX_FLAGS="-I${CLANG_RESOURCE_DIR}/include"
```

Download and install [PyKaldi](https://github.com/pykaldi/pykaldi).

```bash
git clone https://github.com/pykaldi/pykaldi.git
cd pykaldi
sudo python setup.py install
```

## Citing
If you use PyKaldi for research, please cite our [ICASSP 2018 paper](https://github.com/pykaldi/pykaldi/blob/77758546da394fea2d8125f096babab04bdb081d/docs/pykaldi-python-wrapper.pdf), as follows:

```
@inproceedings{pykaldi,
  title = {PyKaldi: A python wrapper for Kaldi},
  author = {Doğan Can and Victor R. Martinez and Pavlos Papadopoulos and Shrikanth S. Narayanan},
  booktitle={Acoustics, Speech and Signal Processing (ICASSP), 2018 IEEE International Conference on},
  year = {2018},
  organization = {IEEE}
}
```


## Contributing

We appreciate all contributions! If you find a bug, feel free to open an issue
or a pull request. If you would like to add, extend or request a feature,
please open an issue for discussion.
