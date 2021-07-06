## 错误复现
- 拉取最新的TNN(2020年底的版本没有问题，新版本才出现问题)
                

        git clone https://github.com/Tencent/TNN
        copy -r TNN 3rdparty/
    
- `bash build.sh`
- 修改`CMakeLists.txt`设置TNN_OPENMP_ENABLE=OFF,正常编译使用
        
        set(TNN_OPENMP_ENABLE OFF CACHE BOOL "" FORCE)  # Multi-Thread
        
- 修改`CMakeLists.txt`设置TNN_OPENMP_ENABLE=ON，出错'__pthread_atfork'被多次定义
        
        set(TNN_OPENMP_ENABLE ON CACHE BOOL "" FORCE)  # Multi-Thread
        
```bash
[100%] Linking CXX executable Detector
/usr/lib/x86_64-linux-gnu/libpthread_nonshared.a(pthread_atfork.oS)：在函数‘__pthread_atfork’中：
/build/glibc-S9d2JN/glibc-2.27/nptl/pthread_atfork.c:51: `__pthread_atfork'被多次定义
/usr/lib/x86_64-linux-gnu/libpthread_nonshared.a(pthread_atfork.oS):/build/glibc-S9d2JN/glibc-2.27/nptl/pthread_atfork.c:51：第一次在此定义
/usr/lib/x86_64-linux-gnu/libpthread_nonshared.a(pthread_atfork.oS)：在函数‘__pthread_atfork’中：
/build/glibc-S9d2JN/glibc-2.27/nptl/pthread_atfork.c:51: `pthread_atfork'被多次定义
/usr/lib/x86_64-linux-gnu/libpthread_nonshared.a(pthread_atfork.oS):/build/glibc-S9d2JN/glibc-2.27/nptl/pthread_atfork.c:51：第一次在此定义
collect2: error: ld returned 1 exit status
CMakeFiles/Detector.dir/build.make:100: recipe for target 'Detector' failed
make[2]: *** [Detector] Error 1
CMakeFiles/Makefile2:105: recipe for target 'CMakeFiles/Detector.dir/all' failed
make[1]: *** [CMakeFiles/Detector.dir/all] Error 2
Makefile:83: recipe for target 'all' failed
make: *** [all] Error 2
build.sh: 行 12: ./Detector: 没有那个文件或目录
```



