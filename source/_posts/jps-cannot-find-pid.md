---
title: jps_cannot_find_pid
date: 2016-11-10 23:42:38
tags: jps, hsperfdata, jstack, jmap, jhat, systemctl
---

# JPS Cannot Find PID

JPS (Java Process Status) is a very useful to to list all the java process.

However, it is very common that we cannot list the java process which is actually running. This is because, when java process is running in JVM, it will create a pid file under /tmp/hsperfdata_$(USER)

Therefore if `jps -l` doesn't contains the process id as you expect there would be:

1. you don't call the jps as the user same as java process, so switch to the user.
1. your java process tmp file was set to other dir

For second scenario, I solve my problem. I user systemctl to maintain my java process. In the service file, I define a item as followed:

```
...
PrivateTmp=true
...
```

This item means that java tmp file will be locate to `systemd-private-****-$(service_name)-****`, therefore jps cannot locate the pid file.

after remove this item, everything works.

 
Potential Error

```
$ jps 24360
RMI Registry not available at 24360:1099
Exception creating connection to: 24360; nested exception is:
    java.net.SocketException: Invalid argument or cannot assign requested address
```

```
$ sudo jstack 24360
24360: Unable to open socket file: target process not responding or HotSpot VM not loaded
The-F option can be used when the target process is not responding
```

```
$ sudo jmap -heap 24360
Attaching to process ID 24360, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.102-b14

using thread-local object allocation.
Parallel GC with 8 thread(s)

    Heap Configuration:
       MinHeapFreeRatio         = 0
          MaxHeapFreeRatio         = 100
             MaxHeapSize              = 3875536896 (3696.0MB)
       NewSize                  = 80740352 (77.0MB)
       MaxNewSize               = 1291845632 (1232.0MB)
       OldSize                  = 162529280 (155.0MB)
       NewRatio                 = 2
          SurvivorRatio            = 8
             MetaspaceSize            = 21807104 (20.796875MB)
       CompressedClassSpaceSize = 1073741824 (1024.0MB)
       MaxMetaspaceSize         = 17592186044415 MB
          G1HeapRegionSize         = 0 (0.0MB)

    Heap Usage:
    Exception in thread "main" java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at sun.tools.jmap.JMap.runTool(JMap.java:201)
        at sun.tools.jmap.JMap.main(JMap.java:130)
    Caused by: java.lang.RuntimeException: unknown CollectedHeap type : class sun.jvm.hotspot.gc_interface.CollectedHeap
        at sun.jvm.hotspot.tools.HeapSummary.run(HeapSummary.java:144)
        at sun.jvm.hotspot.tools.Tool.startInternal(Tool.java:260)
        at sun.jvm.hotspot.tools.Tool.start(Tool.java:223)
        at sun.jvm.hotspot.tools.Tool.execute(Tool.java:118)
        at sun.jvm.hotspot.tools.HeapSummary.main(HeapSummary.java:49)
        ... 6 more
```
        
