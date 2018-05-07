
ビルド手順:
```
mkdir mingw32
cd mingw32
cmake -G "MSYS Makefiles"  -D BUILD_CPP_TESTS:BOOL=OFF -D BUILD_GTK_TESTS:BOOL=OFF -D BUILD_QT5_TESTS:BOOL=OFF -D ENABLE_QT5:BOOL=OFF -D ENABLE_GLIB:BOOL=OFF  ..
make
```

環境:
```
$ uname -a
MINGW32_NT-10.0-WOW DD36 2.6.0(0.304/5/3) 2016-09-07 21:23 i686 Msys
```

構築:
```
$ pacman -S mingw32/mingw-w64-i686-freetype
$ pacman -S mingw32/mingw-w64-i686-gcc
$ pacman -S mingw32/mingw-w64-i686-make
$ pacman -S msys/make
$ pacman -S mingw32/mingw-w64-i686-libjpeg-turbo
$ pacman -S mingw32/mingw-w64-i686-openjpeg2
$ pacman -S mingw32/mingw-w64-i686-cmake
$ pacman -S mingw32/mingw-w64-i686-pkg-config
```

事前になきものにする:
```
"C:\msys32\mingw32\lib\gcc\i686-w64-mingw32\6.2.0\-libstdc++.dll.a"
"C:\msys32\mingw32\i686-w64-mingw32\lib\-libpthread.dll.a" 
"C:\msys32\mingw32\i686-w64-mingw32\lib\-libwinpthread.dll.a"
```
