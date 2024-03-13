---
title: ЧЗВ для Windows
weight: 40
---

## vcpkg download package failed

### Ошибка

```
 -- Fetching https://chromium.googlesource.com/libyuv/libyuv 287158925b0e03ea4499a18b4e08478c5781541b...
   CMake Error at scripts/cmake/vcpkg_execute_required_process.cmake:127 (message):
       Command failed: D:/program/Git/mingw64/bin/git.exe fetch https://chromium.googlesource.com/libyuv/libyuv 287158925b0e03ea4499a18b4e08478c5781541b --depth 1 -n
```

### Решение

Используйте браузер для загрузки `https://chromium.googlesource.com/libyuv/libyuv/+archive/287158925b0e03ea4499a18b4e08478c5781541b.tar.gz`, затем переместите файл в `vcpkg/downloads` и переустановите.



## Package in Cargo.lock not exist

### Ошибка

```
$ cargo run
       Updating git repository `https://github.com/open-trade/confy`
   warning: spurious network error (2 tries remaining): failed to receive response: Operation Timeout
   ; class=Os (2)
   error: failed to get `confy` as a dependency of package `hbb_common v0.1.0 (D:\rustdesk\rustdesk\rustdesk\libs\hbb_common)`

   Caused by:
     failed to load source for dependency `confy`

   Caused by:
     Unable to update https://github.com/open-trade/confy#27fa1294

   Caused by:
     object not found - no match for id (27fa12941291b44ccd856aef4a5452c1eb646047); class=Odb (9); code=NotFound (-3)
```

Возможно, автор использовал `git force push`, и предыдущий коммит был перезаписан.

### Решение

`cargo update`, чтобы принудительно обновить пакет



## VCPKG_ROOT not set

### Ошибка

```
thread 'main' panicked at 'Failed to find package: VcpkgNotFound("No vcpkg installation found. Set the VCPKG_ROOT environment variable or run 'vcpkg integrate install'")', libs\scrap\build.rs:7:45
```

### Решение

Добавьте переменную окружения `VCPKG_ROOT` или запустите `VCPKG_ROOT=<vcpkg_dir> cargo run`



## clang not installed, or  LIBCLANG_PATH  not set

### Ошибка

```
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['clang.dll', 'libclang.dll'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', C:\Users\selfd\.cargo\registry\src\mirrors.ustc.edu.cn-61ef6e0cd06fb9b8\bindgen-0.59.2\src/lib.rs:2144:31
```

### Решение

Установите [llvm](https://releases.llvm.org/download.html), добавьте переменную среды `LIBCLANG_PATH` как `llvm_install_dir/bin`
