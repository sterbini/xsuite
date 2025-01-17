.. _installation-page:

============
Installation
============

We assume that you have a recent python installation (python 3.8+). It this is not the case you can make one following the dedicated section on :ref:`how to get a miniconda installation<miniconda>`.

.. contents:: Table of Contents
    :depth: 3

Basic installation
==================

The Xsuite packages can be installed using pip:

.. code-block:: bash

    pip install xsuite

This installation allows using Xsuite on CPU. To use Xsuite on GPU, with the cupy and/or pyopencl you need to install the corresponding packages, as described in the :ref:`dedicated section<gpuinst>`.

After the installation, you can choose to precompile some often-used kernels, in
order to reduce the waiting time spent on running the simulations later on. This
can be accomplished simply by running the following command:

.. code-block:: bash

    xsuite-prebuild


Developer installation
======================

If you need to develop Xsuite, you can clone the packages from GitHub and install them with pip in editable mode:

.. code-block:: bash

    git clone https://github.com/xsuite/xobjects
    pip install -e xobjects

    git clone https://github.com/xsuite/xdeps
    pip install -e xdeps

    git clone https://github.com/xsuite/xpart
    pip install -e xpart

    git clone https://github.com/xsuite/xtrack
    pip install -e xtrack

    git clone https://github.com/xsuite/xfields
    pip install -e xfields


This installation allows using Xsuite on CPU. To use Xsuite on GPU, with the cupy and/or pyopencl you need to install the corresponding packages, as described in the :ref:`dedicated section<gpuinst>`.


Optional dependencies
=====================

To import MAD-X lattices you will need the cpymad package, which can be installed as follow:

.. code-block:: bash

    $ pip install cpymad

To import lattices from a set of sixtrack input files (fort.2, fort.3, etc.) you will need the sixtracktools package, which can be installed as follow:

.. code-block:: bash

    $ git clone https://github.com/sixtrack/sixtracktools
    $ pip install -e sixtracktools

Some of the tests rely on pyheadtail to test the corresponding interface:

.. code-block:: bash

    $ git clone https://github.com/pycomplete/pyheadtail
    $ pip install cython
    $ pip install -e pyheadtail

.. _gpuinst:

GPU support
===========

In the following section we describe the steps to install the two supported GPU platforms, i.e. cupy and pyopencl.

Installation of cupy
--------------------

In order to use the :doc:`cupy context<contexts>`, the cupy package needs to be installed.
In Anaconda, Miniconda, or Miniforge (if you don't have either of those, see the
dedicated section on :ref:`how to get a miniconda installation<miniconda>`)
this can be done as follows for example for CUDA version 10.1.243:

.. code-block:: bash

    $ conda install mamba -n base -c conda-forge
    $ pip install cupy-cuda101
    $ mamba install cudatoolkit=10.1.243

Remember to check your CUDA version e.g. via ``$ nvcc --version`` and use the appropriate tag.


Installation of PyOpenCL
------------------------

In order to use the :doc:`pyopencl context<contexts>`, the PyOpenCL package needs to be installed.
In Anacoda or Miniconda this can be done as follows:

.. code-block:: bash

    $ conda config --add channels conda-forge  # not needed for Miniforge
    $ conda install pyopencl


Check that there is an OpenCL installation in the system:

.. code-block:: bash

    $ ls /etc/OpenCL/vendors


Make the OpenCL installation visible to pyopencl:

.. code-block:: bash

    $ conda install ocl-icd-system


For the PyOpenCL context we will need the `gpyfft <https://github.com/geggo/gpyfft>`_ and the `clfft <https://github.com/clMathLibraries/clFFT>`_ libraries.
For this purpose we need to install cython.

.. code-block:: bash

    $ pip install cython


Then we can install clfft.

.. code-block:: bash

    $ conda install -c conda-forge clfft


We locate the library and headers here:

.. code-block:: bash

    $ ls ~/miniconda3/pkgs/clfft-2.12.2-h83d4a3d_1/
    # gives: include  info  lib

(Or locate the directory via ``find $(dirname $(dirname $(type -P conda)))/pkgs -name "clfft*" -type d`` .)

We obtain gpyfft from github:

.. code-block:: bash

    $ git clone https://github.com/geggo/gpyfft

and we install gpyfft with pip providing extra flags as follows:

.. code-block:: bash

     $ pip install --global-option=build_ext --global-option="-I/home/giadarol/miniconda3/pkgs/clfft-2.12.2-h83d4a3d_1/include" --global-option="-L/home/giadarol/miniconda3/pkgs/clfft-2.12.2-h83d4a3d_1/lib" gpyfft/

Alternatively (if the command above does not work) we can edit the ``setup.py`` of gpyfft to provide the right paths to your clfft installation (and potentially the OpenCL directory of your platform):

.. code-block:: python

    if 'Linux' in system:
        CLFFT_DIR = os.path.expanduser('~/miniforge3/pkgs/clfft-2.12.2-h83d4a3d_1/')
        CLFFT_LIB_DIRS = [r'/usr/local/lib64']
        CLFFT_INCL_DIRS = [os.path.join(CLFFT_DIR, 'include'), ] # remove the 'src' part
        CL_INCL_DIRS = ['/opt/rocm-4.0.0/opencl/include']

And install gpyfft locally.

.. code-block:: bash

    $ pip install -e gpyfft/


.. _miniconda:

Install Miniforge
=================

If you don't have a miniforge (or miniconda) installation, you can quickly get
one ready for xsuite installation with the following steps.
`Miniforge <https://github.com/conda-forge/miniforge>`__ is conda package
manager preconfigured to use ``conda-forge``.

On Linux
--------

.. code-block:: bash

    $ cd ~
    $ wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
    $ bash Miniforge3-Linux-x86_64.sh
    $ source miniforge3/bin/activate
    $ pip install numpy scipy matplotlib pandas ipython pytest

On MacOS
--------

To install miniforge on Apple silicon, follow the instructions in the
:ref:`dedicated chapter below<apple_silicon>`, or simply replace `x86_64`
with `arm64` in the link below.

.. code-block:: bash

    $ cd ~
    $ curl -OL https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh
    $ bash Miniforge3-MacOSX-x86_64.sh
    $ source miniforge3/bin/activate
    $ conda install clang_osx-64
    $ pip install numpy scipy matplotlib pandas ipython pytest

.. _apple_silicon:

Install on Apple silicon
========================

Native installation
-------------------

Conda (Miniforge)
~~~~~~~~~~~~~~~~~

First, we install miniforge. Do say yes to shell initialisation, or, otherwise,
run the command suggested by the installer to initialise ``conda`` in the
current terminal session.

.. code:: bash

   curl -OL https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh
   bash Miniforge3-MacOSX-arm64.sh

Let us now create a conda environment for xsuite. This will be beneficial if you
want to have multiple separate projects (or indeed the native and the emulated
x86 versions of xsuite side-by-side).

.. code:: bash

   conda create -n xsuite-arm python=3.10
   conda activate xsuite-arm

Some prerequisites need to be installed, notably compilers. While xsuite
itself requires a working C compiler, we will also need to build other
dependencies from scratch, for these we will also need ``gfortran``. We
can install compilers supplied by ``conda-forge``:

.. code:: bash

   conda install compilers cmake

Building MAD-X and cpymad (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First we build ``MAD-X`` and ``cpymad`` (largely following the
recommendations found
`here <https://github.com/hibtc/cpymad/pull/114>`__ and
`here <https://hibtc.github.io/cpymad/installation/macos.html>`__):

.. code:: bash

   git clone https://github.com/MethodicalAcceleratorDesign/MAD-X
   pip install --upgrade cmake cython wheel setuptools delocate
   mkdir MAD-X/build && cd MAD-X/build

   cmake .. \
       -DCMAKE_POLICY_DEFAULT_CMP0077=NEW \
       -DCMAKE_POLICY_DEFAULT_CMP0042=NEW \
       -DCMAKE_OSX_ARCHITECTURES=arm64 \
       -DCMAKE_C_COMPILER=clang \
       -DCMAKE_CXX_COMPILER=clang++ \
       -DCMAKE_Fortran_COMPILER=gfortran \
       -DBUILD_SHARED_LIBS=OFF \
       -DMADX_STATIC=OFF \
       -DCMAKE_INSTALL_PREFIX=../dist \
       -DCMAKE_BUILD_TYPE=Release \
       -DMADX_INSTALL_DOC=OFF \
       -DMADX_ONLINE=OFF \
       -DMADX_FORCE_32=OFF \
       -DMADX_X11=OFF
   # Verify in the output of the above command that libraries
   # for BLAS and LAPACK have been found. For this, you may need
   # the macOS SDK, installable with `xcode-select --install`.
   cmake --build . --target install

   cd ../..
   export MADXDIR="$(pwd)"/MAD-X/dist
   git clone https://github.com/hibtc/cpymad.git
   cd cpymad
   export CC=clang
   python setup.py build_ext -lblas -llapack
   python setup.py bdist_wheel
   delocate-wheel dist/*.whl
   pip install dist/cpymad-*.whl

   # Optionally, verify the installation of cpymad:
   pip install pandas pytest
   python -m pytest test

Sixtracktools (optional)
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: bash

   git clone https://github.com/sixtrack/sixtracktools
   pip install ./sixtracktools

PyHEADTAIL (optional)
~~~~~~~~~~~~~~~~~~~~~

.. code:: bash

   git clone https://github.com/pycomplete/pyheadtail
   pip install --upgrade cython scipy h5py
   pip install -e ./pyheadtail

Xsuite
~~~~~~

Finally, we can install xsuite:

.. code:: bash

   mkdir xsuite && cd xsuite

   git clone https://github.com/xsuite/xobjects
   pip install -e xobjects

   git clone https://github.com/xsuite/xdeps
   pip install -e xdeps

   git clone https://github.com/xsuite/xpart
   pip install -e xpart

   git clone https://github.com/xsuite/xtrack
   pip install -e xtrack

   git clone https://github.com/xsuite/xfields
   pip install -e xfields

If all of the optional dependencies have also been installed, we can
verify our installation. To install test dependencies for an xsuite
package, one can replace the ``pip install -e some_package`` commands in
the above snippet with ``pip install -e 'some_package[tests]'``. Once
the test dependecies are also installed, we can run the tests to check
if xsuite works correctly:

.. code:: bash

   cd ..
   PKGS=(xobjects xdeps xpart xtrack xfields)
   for PKG in ${PKGS[@]}; do
   python -m pytest xsuite/$PKG/tests
   done

Rosetta installation (x86 emulation)
------------------------------------

Install miniforge as above, and then create an x86 conda environment,
like so:

.. code:: bash

   CONDA_SUBDIR=osx-64 conda create -n xsuite-x86 python=3.10
   conda activate xsuite-x86
   conda config --env --set subdir osx-64
   conda install compilers

.. note::

   You may get some warnings similar to
   ``activate_clang:69: read-only file system: /meson_cross_file.txt'``.
   These may be ignored.

After carrying out the above steps, you can install xsuite using the
usual commands, following either the basic or a developer installation
guide, as given at the top of this page.
