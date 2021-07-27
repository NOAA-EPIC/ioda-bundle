(C) Copyright 2017-2021 UCAR

This software is licensed under the terms of the Apache Licence Version 2.0
which can be obtained at http://www.apache.org/licenses/LICENSE-2.0.

----------------
# Building the IODA  bundle
```

    cd /somewhere/to/store/source/code
    git clone https://github.com/JCDSA/ioda-bundle.git

    cd /somewhere/to/build
    ecbuild /somewhere/to/store/source/code/ioda-bundle
    make -j4
```

The `ecbuild` command above will clone all the source code for the projects defined in the
`CMakeLists.txt` in the bundle and the make command will build them all.

To work with a different branch than the default for a given project, the branch must be
modified in the CMakeLists.txt for the bundle.

Note: `ecbuild` accepts all cmake flags, for example, compilers can be selected with:
    `ecbuild -DCMAKE_CXX_COMPILER=/path/to/gcc-7.2/bin/g++ ${SRC}`

For testing (from build directory):

```
    ctest
```

## Build options
All of the following options can be used individually or together in a single build sequence.

### IODA Converters
By default, the ioda-converters portion of ioda-bundle is not built. To enable this, use the following ecbuild option:
```
    ecbuild -DBUILD_IODA_CONVERTERS=ON /somewhere/to/store/source/code/ioda-bundle
```
This will build the ioda-converters code, plus add in the ioda-converter tests when running `ctest`.

### IODA Python API
By default, the ioda Python API is not built. To enable this, use the following option:
```
    ecbuild -DBUILD_PYTHON_BINDINGS=ON /somewhere/to/store/source/code/ioda-bundle
```
This will build the ioda Python API code, plus add in the associated tests. Note that this option requires python3 to be installed on your system, and access to the pybind11 libraray which is available in the containers or can be built and installed using jedi-stack.

### Doxygen html documentation
By default, the ioda Doxygen documentation is not built. To enable the build of the html version of the Doxygen documentation, use the follwing option:
```
    ecbuild -DBUILD_IODA_BUNDLE_DOC=ON /somewhere/to/store/source/code/ioda-bundle
```
This creates the html website version of the Doxygen documentation in your build area. This is located in:
```
    /somewhere/to/build/Documentation/html
```
and can be viewed with the following URL in your browser:
```
    file:///somewhere/to/build/Documentation/html/index.html
```

--------
# Working with the code

The `CMakeLists.txt` file in this directory contains the list of the repositories included
in the bundle and the branch to be used. The branch specified in the `CMakeLists.txt` is
the one that will be compiled. When working with you own branch, the should be changed in
the `CMakeLists.txt` file but it is not necessary to re-run `ecbuild`, make is enough.

After the first build, changes in the code can be tested by re-running only
(from build directory):

```
    make -j4
    ctest
```

By default, make will not update your local repository from the remote. To update all repositories
in the bundle, run:
```
    make update
```

The update will fail for repositories that contain uncommitted code. This is a safety mechanism to avoid losing your work.

-------
# Note about using git

It is recommended that you create a `.gitconfig` file in your home directory (inside the container if working from a container) with the following content:

```
    $ cat .gitconfig
    [user]
        name = Your Name
        email = yourname@somewhere.something

    [credential]
        helper = cache --timeout=3600
```

Since the bundle accesses many repositories, it can be tedious to enter your username and
password for every operation.  With the last line the system will remember your password for
a given time (defined in seconds by the timeout parameter).

