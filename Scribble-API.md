# init scribble 

Before using scribble feature, developer needs to call 

```
EpdController.enterScribbleMode();
```
to request framework to change to scribble mode. When scribble is finished, developer could call

```
EpdController.leaveScribbleMode();
```

Note: During scribble mode, all other screen update request will be ignored.


# moveTo and lineTo

When using scribble, Onyx Android SDK provides two functions to draw line on screen

```
EpdController.moveTo(x, y, lineWidth);
EpdController.lineTo(x, y, UpdateMode.DW);
```

The x and y must be in screen coordinates. For M96, developer could convert view coordinate to screen coordinates by using following code

```
 Matrix matrix = new Matrix();
 matrix.postRotate(270);
 matrix.postTranslate(0, 825);
 
 // view location on screen.
 int viewLocation[] = {0, 0};
 getLocationOnScreen(viewLocation);
 float screenPoints[] = {viewLocation[0] + touchX, viewLocation[1] + touchY};
 float dst[] = {0, 0};
 matrix.mapPoints(dst, screenPoints);
 EpdController.moveTo(dst[0], dst[1], width);
```

