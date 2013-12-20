App development Guidelines for R65/C65

Generally speaking, App development for R65 with E-Ink screen is just like normal ways on LCD screen, some attentions are needed for the characters of E-Ink screen and device specific features: 

1. Use 16-level grayscale for your UI. 
2. Avoid dynamic UI elements, such as animation. 
3. Avoid scrollable UI components, use paged ListView/GridView instead. 
4. Because we've adjusted standard android power management policy to fit the device, so you need use IDeviceController.newWakeLock() to get corresponding WakeLock to prevent device from sleep.
5. You can access device specific features in package com.onyx.android.sdk.device, some wrapper classes are provided for convenience, such as EpdController to interfere with E-Ink screen, and EnvironmentUtil as a supplement of Android's android.os.Environment, etc.