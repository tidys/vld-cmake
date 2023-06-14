# vld with cmake

使用cmake编译vld，目前仅仅适配了win32

项目的原始代码来自： https://github.com/KindDragon/vld

cmake kit建议使用vs2015

vs2019也可以，记得不要升级项目，因为vld的vs版本不能高于vs2015，cmake生成的vld项目指定使用的就是vs2015

其他vs版本没有验证过
### 增加的功能
vld.ini配置文件增加选项

- ReportFilter

    对结果过滤结果
- ReportFilterLessBytes

    过滤小于指定大小的结果，当ReportFilter为on时有效
- ReportMaxLeakSizeRankingsCount

  展示内存消耗排行数量，方便排查修复泄露热点


vld.ini里面有更详细的使用介绍


## 示例
```c++
#include "vld/vld.h"
void main()
{
    int *p = new int();
}
```

```
Visual Leak Detector read settings from: H:\proj\vld-cmake\build\Debug\vld.ini
Visual Leak Detector Version 2.5.1 installed.
    Aggregating duplicate leaks.
    Suppressing data dumps.
    Outputting the report to the debugger and to H:\proj\vld-cmake\build\memory_leak_report.txt
    ReportMaxLeakSizeRankingsCount: 10

WARNING: Visual Leak Detector detected memory leaks!
---------- Block 1 at 0x00CA3390: 4 bytes ----------
  CRT Alloc ID: 287
  Leak Hash: 0xBC63EABE, Count: 1, Total 4 bytes / 0.004 kb / 0.00000 mb
  Call Stack (TID 22188):
    D:\a\_work\1\s\src\vctools\crt\vcstartup\src\heap\new_scalar.cpp (35): vld-cmake.exe!operator new() + 0x9 bytes
    H:\proj\vld-cmake\test\main.cpp (4): vld-cmake.exe!main() + 0x10 bytes


------------------------------------------------------------------------------
Rank 1 : Leak Hash 0xBC63EABE, Total      4 bytes / 0.004 kb / 0.00000 mb

Visual Leak Detector detected 1 memory leak (40 bytes / 0.039 kb / 0.00004 mb).
Largest number used: 40 bytes.
Total allocations: 40 bytes.
Visual Leak Detector is now exiting.

```