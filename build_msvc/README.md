Building Bitcoin Core with Visual Studio
========================================

Introduction
---------------------
Solution and project files to build the Bitcoin Core applications (except Qt dependent ones) with Visual Studio 2017 can be found in the build_msvc directory.

Building with Visual Studio is an alternative to the Linux based [cross-compiler build](https://github.com/bitcoin/bitcoin/blob/master/doc/build-windows.md).

Dependencies
---------------------
A number of [open source libraries](https://github.com/bitcoin/bitcoin/blob/master/doc/dependencies.md) are required in order to be able to build Bitcoin.

Options for installing the dependencies in a Visual Studio compatible manner are:

- Use Microsoft's [vcpkg](https://docs.microsoft.com/en-us/cpp/vcpkg) to download the source packages and build locally. This is the recommended approach.
- Download the source code, build each dependency, add the required include paths, link libraries and binary tools to the Visual Studio project files.
- Use [nuget](https://www.nuget.org/) packages with the understanding that any binary files have been compiled by an untrusted third party.

The external dependencies required for the Visual Studio build are (see the [dependencies doc](https://github.com/bitcoin/bitcoin/blob/master/doc/dependencies.md) for versions):

- Berkeley DB,
- OpenSSL,
- Boost,
- libevent,
- ZeroMQ

Additional dependencies required from the [bitcoin-core](https://github.com/bitcoin-core) github repository are:
- SECP256K1,
- LevelDB

Building
---------------------
The instructions below use vcpkg to install the dependencies.

- Install [`vcpkg`](https://github.com/Microsoft/vcpkg).
- Install the required packages (replace x64 with x86 as required). The list of required packages can be found in the `build_msvc\vcpkg-packages.txt` file. The PowerShell command below will work if run from the repository root directory and `vcpkg` is in the path. Alternatively the contents of the packages text file can be pasted in place of the `Get-Content` cmdlet.

```
PS >.\vcpkg install --triplet x64-windows-static $(Get-Content -Path build_msvc\vcpkg-packages.txt).split()
```

- Use Python to generate `*.vcxproj` from Makefile

```
PS >py -3 msvc-autogen.py
```

- An optional step is to adjust the settings in the build_msvc directory and the common.init.vcxproj file. This project file contains settings that are common to all projects such as the runtime library version and target Windows SDK version. The Qt directories can also be set.

- Build with Visual Studio 2017 or msbuild.

```
msbuild /m bitcoin.sln /p:Platform=x64 /p:Configuration=Release /p:PlatformToolset=v141 /t:build
>>>>>>> a6f6333ba2 (Merge #17416: Appveyor improvement - text file for vcpkg package list)
```

- Use Python to generate *.vcxproj from Makefile

```
msbuild /m bitcoin.sln /p:Platform=x64 /p:Configuration=Release /t:build
```

- Build in Visual Studio.
