# poppler 自炊につきまして

Windows でのみ有用です。
MSYS2 を使用します。

## ビルド手順

```sh
mkdir mingw32
cd mingw32
cmake -G "MSYS Makefiles"  -D BUILD_CPP_TESTS:BOOL=OFF -D BUILD_GTK_TESTS:BOOL=OFF -D BUILD_QT5_TESTS:BOOL=OFF -D ENABLE_QT5:BOOL=OFF -D ENABLE_GLIB:BOOL=OFF  ..
make
```

## 手始めに環境構築から

https://www.msys2.org/ にて `msys2-i686-20161025.exe` をダウンロードしてセットアップします。

### MinGW 環境構築

```sh
pacman -S mingw32/mingw-w64-i686-freetype
pacman -S mingw32/mingw-w64-i686-gcc
pacman -S mingw32/mingw-w64-i686-make
pacman -S msys/make
pacman -S mingw32/mingw-w64-i686-libjpeg-turbo
pacman -S mingw32/mingw-w64-i686-openjpeg2
pacman -S mingw32/mingw-w64-i686-cmake
pacman -S mingw32/mingw-w64-i686-pkg-config
pacman -S mingw32/mingw-w64-i686-jsoncpp
```

### openjpeg ≠ openjpeg2

CMakeLists.txt にて、あたかも `OpenJPEG` が `openjpeg2` と同一であるかのような仮定がなされているので、その対策です。

問題の箇所:

```txt
if(ENABLE_LIBOPENJPEG STREQUAL "openjpeg2")
  find_package(OpenJPEG)
  set(WITH_OPENJPEG ${OpenJPEG_FOUND})
  if(NOT OpenJPEG_FOUND OR OPENJPEG_MAJOR_VERSION VERSION_LESS 2)
    message(FATAL_ERROR "Install libopenjpeg2 before trying to build poppler. You can also decide to use the internal unmaintained JPX decoder or none at all.")
  endif()
  set(HAVE_JPX_DECODER ON)
```

エラーメッセージ:

```txt
CMake Error at CMakeLists.txt:207 (message):
  Install libopenjpeg2 before trying to build poppler.  You can also decide
  to use the internal unmaintained JPX decoder or none at all.
```

対策:

```sh
cp /mingw32/lib/pkgconfig/libopenjp2.pc /mingw32/lib/pkgconfig/libopenjpeg.pc
```

### `*.notdll.a` 作成

```sh
./notdll.sh /mingw32/lib/*.dll.a
```

参考: [xxx.dll.a ではなく xxx.notdll.a へ](http://dd-kaihatsu-room.blogspot.jp/2018/04/xxxdlla-xxxnotdlla.html)

事前になきものにする:
```
"C:\msys32\mingw32\lib\gcc\i686-w64-mingw32\6.2.0\-libstdc++.dll.a"
"C:\msys32\mingw32\i686-w64-mingw32\lib\-libpthread.dll.a" 
"C:\msys32\mingw32\i686-w64-mingw32\lib\-libwinpthread.dll.a"
```

### `undefined reference to `_imp____acrt_iob_func'` につきまして

