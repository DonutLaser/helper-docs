# INSTALL

For use with go bindings:
1. Install mingw-w64 from [http://mingw-w64.org/doku.php/download/mingw-builds](http://mingw-w64.org/doku.php/download/mingw-builds)
    * Version: latest
    * Architecture: `x86_64`
    * Threads: win32
    * Exception: she
    * Build revision: highest
    * Destination folder: `C:/mingw-w64` (must be a path without spaces)
2. Install SDL2 Current Runtime Binaries for win32-x64 from [http://libsdl.org/download-2.0.php](http://libsdl.org/download-2.0.php)
3. Copy Runtime Binaries files into `C:\mingw-w64\mingw64\lib` folder
4. Install SDL2 Current Development Libraries for mingw from [http://libsdl.org/download-2.0.php](http://libsdl.org/download-2.0.php)
5. Copy `x86_64-w64-mingw32` from Current Development Libraries into `C:\mingw-w64\mingw64`
6. Copy `x86_64-w64-mingw32\include` from Current Development Libraries into `C:\mingw-w64\mingw64\include`
7. Copy `x86_64-w64-mingw32\lib` from Current Development Libraries into `C:\mingw-w64\mingw64\lib`
8. Add environment variable `CGO_CFLAGS` with value `-I C:\mingw-w64\mingw64\include`
9. Add to PATH: `C:\mingw-w64\mingw64\bin`
10. Add to PATH: `C:\mingw-w64\mingw64\lib`