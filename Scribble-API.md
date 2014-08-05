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

scribble with shape support
=============================

Developer may need to call enterScribbleMode and leaveScribbleMode() during scribbling. 

```
EpdController.enterScribbleMode();
EpdController.leaveScribbleMode();

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
                    dst = paintView.mapPoint(e.getX(), e.getY());
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
