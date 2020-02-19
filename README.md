# arma-gtest-dont-behave
I can't bring both Armadillo and GTest into a CMake project at the same time.  It looks like there is a naming clash for `Int32`.

cmake gives the following:

```
> build $ cd build/
> build $ cmake ..
-- The CXX compiler identification is AppleClang 11.0.0.11000033
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Armadillo: /usr/local/lib/libarmadillo.dylib (found version "9.850.1") 
-- Enable testing: ON
-- The C compiler identification is AppleClang 11.0.0.11000033
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Found PythonInterp: /anaconda3/bin/python (found version "3.7.3") 
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/aeastmond/arma-gtest-dont-behave/build
```
That looks ok.  But when I build:

```
> build $ cmake --build .
Scanning dependencies of target armadillo_library
[  5%] Building CXX object CMakeFiles/armadillo_library.dir/using_armadillo.cpp.o
[ 11%] Linking CXX static library libarmadillo_library.a
[ 11%] Built target armadillo_library
Scanning dependencies of target gtest
[ 17%] Building CXX object _deps/googletest-build/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
In file included from /Users/aeastmond/arma-gtest-dont-behave/build/_deps/googletest-src/googletest/src/gtest-all.cc:42:
In file included from /Users/aeastmond/arma-gtest-dont-behave/build/_deps/googletest-src/googletest/src/gtest.cc:142:
/Users/aeastmond/arma-gtest-dont-behave/build/_deps/googletest-src/googletest/src/gtest-internal-inl.h:132:40: error: unknown type name
      'Int32'
    const char* str, const char* flag, Int32* value);
                                       ^
/Users/aeastmond/arma-gtest-dont-behave/build/_deps/googletest-src/googletest/src/gtest-internal-inl.h:136:34: error: unknown type name
      'Int32'
inline int GetRandomSeedFromFlag(Int32 random_seed_flag) {

... snip ...

/Users/aeastmond/arma-gtest-dont-behave/build/_deps/googletest-src/googletest/src/gtest.cc:311:25: error: unknown type name 'UInt32'
UInt32 Random::Generate(UInt32 range) {
                        ^
fatal error: too many errors emitted, stopping now [-ferror-limit=]
20 errors generated.
make[2]: *** [_deps/googletest-build/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o] Error 1
make[1]: *** [_deps/googletest-build/googlemock/gtest/CMakeFiles/gtest.dir/all] Error 2
make: *** [all] Error 2
```

Can anybody help?
