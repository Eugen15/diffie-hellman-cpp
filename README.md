# Diffie-Hellman CPP

Diffie-Hellman key exchange between OpenSSL 3, LibreSSL 3 and a custom C++ implementation. The project is just for fun. Also, you can use the project as **an example of OpenSSL/LibreSSL DH/ECDH key exchange**. The project can be compiled with either OpenSSL 3 or LibreSSL 3 support. A custom implementation is not available for Elliptic-curve Diffie�Hellman yet!

## Source code
See below for the main classes.
* ``DiffieHellman`` is an interface for the DH key exchange implementations. Also, it provides the factory method to create an appropriate instance.
* ``DiffieHellmanOpenSSL`` is a typical OpenSSL DH key exchange implementation.
* ``DiffieHellmanLibreSSLDH`` is  a typical LibreSSL DH key exchange implementation.
* ``DiffieHellmanLibreSSLBN`` is an intermediate DH key exchange implementation which is based on LibreSSL BIGNUM without using the DH functions.
* ``DiffieHellmanBoost`` is a C++ DH key exchange implementation which uses boost::multiprecision::cpp_int for big number operations.
* ``ECDiffieHellman`` is an interface for ECDH key exchange implementations. Also, it provides the factory method to create an appropriate instance. 
* ``ECDiffieHellmanOpenSSL`` is a typical OpenSSL ECDH key exchange implementation.
* ``ECDiffieHellmanLibreSSL`` is a typical LibreSSL ECDH key exchange implementation.
* ``DHTester`` is a DH key exchange tester class.
* ``ECDHTester`` is a ECDH key exchange tester class.

## Building
The project uses ``std::format``, ``using enum`` and other modern C++ features so you need latest compilers for it. See the links below.

* https://en.cppreference.com/w/cpp/compiler_support
* https://docs.microsoft.com/en-us/cpp/overview/visual-cpp-language-conformance
* https://clang.llvm.org/cxx_status.html

You also need CMake, OpenSSL (or LibreSSL) and Boost C++ (headers only). By default, LibreSSL will be used. Use ``cmake .. -DUSE_OPENSSL=ON`` If you want to compile the project with OpenSSL support.

### Windows

#### Visual Studio
VS 2019 v16.10 & v16.11 or later is required. Personally, I use VS 2022. The download link is here https://visualstudio.microsoft.com/vs/community/.

#### CMake
CMake is available as a part of VS 2022 installation. In my case it is located in the following folder: ``C:/Program Files/Microsoft Visual Studio/2022/Community/Common7/IDE/CommonExtensions/Microsoft/CMake/CMake/bin/``. Install CMake separately https://cmake.org/ if your VS does not include it. 

#### OpenSSL
Download OpenSSL 3.x.x from https://www.openssl.org, unpack it.\
Download the Strawberry Perl portable edition from https://strawberryperl.com and unpack it to a folder next to unpacked OpenSSL.

Open x64 Native Tools Command Prompt for VS 2022\
Use any of the following commands depending on how you want to compile **diffie-hellman-cpp** eventually:
``�..\strawberry\perl\bin\perl� Configure no-asm debug-VC-WIN64A`` Debug, x64, /MD\
``�..\strawberry\perl\bin\perl� Configure no-asm VC-WIN64A`` Release, x64, /MD\
``�..\strawberry\perl\bin\perl� Configure no-asm debug-VC-WIN64A no-shared -DMT`` Release, x64, /MT\
``�..\strawberry\perl\bin\perl� Configure no-asm VC-WIN64A no-shared -DMT`` Release, x64, /MT

When it completes, run the following command: ``nmake``
The OpenSSL header folder can be taken from the include subfolder. The compiled libraries can be taken from the root of your OpenSSL compilation folder.

See ``NOTES-WINDOWS.md`` if the instruction above does not work.

#### LibreSSL
If you want to test the project with LibreSSL support instead of OpenSSL, download the latest sources from https://www.libressl.org/ and do the):
Go to the root of the unpacked LibreSSL and execute the following commands:

```
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=your_libressl_installation_folder
cmake --build . --config Debug
cmake --install . --config Debug
```
(you can choose either Debug or Release).

See ``README.windows`` if the instruction above does not work.

#### Boost
Download and unpack the latest Boost C++ if you do not have it on your machine. You can compile the library if you like but it is not necessary.

#### diffie-hellman-cpp
Download the project, go to the root and run the following commands:
```
mkdir build
cd build
:: For LibreSSL
cmake .. -DINCLUDE_DIRS="your_boost_root_folder;your_libressl_installation_folder\include" -DLIBRARY_DIRS="your_libressl_installation_folder\lib"
:: For OpenSSL
cmake .. -DUSE_OPENSSL=ON -DINCLUDE_DIRS="your_boost_root_folder;your_libressl_installation_folder\include" -DLIBRARY_DIRS="your_libressl_installation_folder\lib"
:: Start compilation
cmake --build . --config Debug
```
(again, choose either Debug or Release according to the library builds).

### Ubuntu 20.x.x

#### CMake
If CMake is not installed, run the following command: ``sudo apt install cmake``.

#### OpenSSL
If not installed, run ``sudo apt  install openssl openssl-dev``

#### LibreSSL
Download the latest LibreSSL https://www.libressl.org/ if you want to test it instead of OpenSSL. Go to the root of the unpacked LibreSSL and execute the following commands:
```
cmake .. -DCMAKE_INSTALL_PREFIX=your_libressl_installation_dir_prefix
cmake --build .
cmake --install .
```
See ``README.md`` if the instruction above does not work.

#### Boost
If you do not have Boost C++ installed on your machine, you have the following options:
* Install the whole Boost C++ : ``sudo apt install libboost-all-dev``
* Install boost::multiprecision only (the package name may be different): ``sudo apt install libboost-mpi1.71-dev``
* Download and unpack Boost C++ to access its headers.

#### Clang 14
You have to install Clang 14 to compile the project (see https://apt.llvm.org/ for updates if the instruction below does not work).
* Add the following lines to your /etc/apt/sources.list:
``deb http://apt.llvm.org/focal/ llvm-toolchain-focal main``  
``deb-src http://apt.llvm.org/focal/ llvm-toolchain-focal main``
* Add signatures for these repos (otherwise apt-get update will complain in the next step):
``wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -``
* Run ``sudo apt update`` to add these repos to the apt.
* Install Clang 14.
``sudo apt install clang-14``
``sudo apt install libc++-14-dev libc++abi-14-dev``
* Make sure now Clang 14 is used by CMake.
``export CXX=clang++-14``

#### diffie-hellman-cpp
Download the project, go to the root and run the following commands:

```
mkdir build
cd build
# To compile with OpenSSL support; no parameters if installed to the system 
cmake .. -DUSE_OPENSSL=ON
# To compile with LibreSSL
cmake .. -DINCLUDE_DIRS="your_boost_root_folder;your_libressl_installation_dir_prefix\include" -DLIBRARY_DIRS="your_libressl_installation_dir_prefix\lib"
# Start compilation
cmake --build .
```
You might not need to specify the Boost C++, OpenSSL/LibreSSL folders if the libraries are installed into your system standard location.

## Testing
Run the program to start the tests. You can explicitly specify the prime length in bits and the generator. See examples below:
* ``diffie-hellman-cpp dh``  DH key exchange. The prime length is 512; the generator is 2.
* ``diffie-hellman-cpp dh 512 5`` DH key exchange. The prime length is 512; the generator is 5.
* ``diffie-hellman-cpp dh 256`` DH key exchange. The prime length is 256 (too small for OpenSSL!); the generator is 2.
* ``diffie-hellman-cpp ecdh`` ECDH key exchange. The curve name is ``secp384r1``.
* ``diffie-hellman-cpp ecdh sect571r1`` ECDH key exchange. The curve name is ``sect571r1``.
