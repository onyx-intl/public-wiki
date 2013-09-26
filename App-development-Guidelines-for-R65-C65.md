App development Guidelines for R65/C65

Generally speaking, App development for R65 with E-Ink screen is just like normal ways on LCD screen, some attentions are needed for the characters of E-Ink screen and device specific features.
1. Use 16-level grayscale for your UI.
2. Avoid dynamic UI elements, such as animation.
3. Avoid scrollable UI components, use paged ListView/GridView instead.
4. Hold PARTIAL_WAKE_LOCK when you excute time consuming work, or else device will go to sleep which can be waken up only by RTC timer (in precision of minute) or Power button. Hold FULL_WAKE_LOCK to prevent device from going to standby. Be attention, even FULL_WAKE_LOCK is helden, device can still go to sleep, but will not go to standby. So PARTIAL_WAKE_LOCK is suggested, only use FULL_WAKE_LOCK when you are aware what you are wanting.
5. You can access device specific features in package com.onyx.android.sdk.device, some wrapper classes are provided for convenience, such as EpdController to interfere with E-Ink screen, and EnvironmentUtil as a supplement of Android's android.os.Environment, etc.