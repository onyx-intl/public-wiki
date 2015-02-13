# init scribble 

Before using scribble feature, developer needs to call 

```
EpdController.enterScribbleMode(view);
```
to request framework to change to scribble mode. When scribble is finished, developer could call

```
EpdController.leaveScribbleMode(view);
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

# stroke color 

you can change stroke color by 

```
 // so far, only black and white are supported due to eink display limit.
 EpdController.setStrokeColor(0xff000000);  // black
 EpdController.setStrokeColor(0xffffffff);  // white

```

# stroke style
You can change the stroke style by 

```
 // only pen style and brush style supported.
 EpdController.setStrokeStyle(0);  // pen style, the line width will not be changed
 EpdController.setStrokeStyle(1);  // brush style, line width will be changed when pressure or speed changed.

```

# Painter style

You can change the painter style by 

```

EpdController.setPainterStyle(true,   // antiAlias or not
 Paint.Style.FILL_AND_STROKE,         // stroke style
 Paint.Join.ROUND,                    // join style
 Paint.Cap.ROUND);                    // cap style

```

smooth scribble
=============================

Developer may need to call enterScribbleMode and leaveScribbleMode() during scribbling. 

```
EpdController.enterScribbleMode(view);
EpdController.leaveScribbleMode(view);

```
```

public class PaintView extends View {

    float [] mapPoint(float x, float y) {
        int viewLocation[] = {0, 0};
        getLocationOnScreen(viewLocation);
        float screenPoints[] = {viewLocation[0] + x, viewLocation[1] + y};
        float dst[] = {0, 0};
        matrix.mapPoints(dst, screenPoints);
        return dst;
    }
}


public class ScribbleActivity extends Activity {

    public boolean onTouchEvent(ScribbleActivity activity, MotionEvent e) {
        // ignore multi touch
        if (e.getPointerCount() > 1) {
            return false;
        }

        final float baseWidth = 5;
        paintView.init(paintView.getWidth(), paintView.getHeight());

        switch (e.getAction() & MotionEvent.ACTION_MASK) {
            case (MotionEvent.ACTION_DOWN):
                float dst[] = paintView.mapPoint(e.getX(), e.getY());
                EpdController.startStroke(baseWidth, dst[0], dst[1], e.getPressure(), e.getSize(), e.getEventTime());
                return true;
            case (MotionEvent.ACTION_CANCEL):
            case (MotionEvent.ACTION_OUTSIDE):
                break;
            case MotionEvent.ACTION_UP:
                dst = paintView.mapPoint(e.getX(), e.getY());
                EpdController.finishStroke(baseWidth, dst[0], dst[1], e.getPressure(), e.getSize(), e.getEventTime());
                return true;
            case MotionEvent.ACTION_MOVE:
                int n = e.getHistorySize();
                for (int i = 0; i < n; i++) {
                    dst = paintView.mapPoint(e.getHistoricalX(i), e.getHistoricalY(i));
                    EpdController.addStrokePoint(baseWidth,  dst[0], dst[1], e.getPressure(), e.getSize(), e.getEventTime());
                }
                dst = paintView.mapPoint(e.getX(), e.getY());
                EpdController.addStrokePoint(baseWidth, dst[0], dst[1], e.getPressure(), e.getSize(), e.getEventTime());
                return true;
            default:
                break;
        }
        return true;
    }

```

# scribble line width

```
public static float startStroke(float baseWidth, float x, float y, float pressure, float size, float time);
public static float addStrokePoint(float baseWidth, float x, float y, float pressure, float size, float time);
public static float finishStroke(float baseWidth, float x, float y, float pressure, float size, float time) ;

Above functions return line width currently calculated. Application may need to change the line width to proper value and save into persistent storage.

```


# handle eraser and quick start

```
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        switch (keyCode) {
            case KeyEvent.KEYCODE_ALT_LEFT:
            case KeyEvent.KEYCODE_ALT_RIGHT:
                // eraser has been pressed
                return false;
            case KeyEvent.KEYCODE_BUTTON_START:
                // the quick start button has been pressed.
                return false;
            default:
                return super.onKeyDown(activity,keyCode,event);
        }
    }
```
