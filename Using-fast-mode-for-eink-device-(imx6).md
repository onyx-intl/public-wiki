# using fast mode

* enter fast mode 

```
// function prototype
// clear means if caller wants to clear screen before animation.  by using clear = true, device will clear device to white before animation with less ghosting
public static boolean applyApplicationFastMode(final String application, boolean enable, boolean clear)
```


```
EpdController.applyApplicationFastMode("your application name", true, true);
```

* exit from fast mode

```
EpdController.applyApplicationFastMode("your application name", false, true);
```


