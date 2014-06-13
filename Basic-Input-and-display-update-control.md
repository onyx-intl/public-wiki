device input event
===================

Onyx device uses standard Android input framework. So basically, developer can use MotionEvent to receive input from device.  You can setup a touch listener to your view by using following code

```
    private void setupListener() {
        yourView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent event) {
                onTouchEvent(this, event);
                return true;
            }
        });
    }

    public boolean onTouchEvent(ReaderActivity activity, MotionEvent e) {
        // ignore multi touch
        if (e.getPointerCount() > 1) {
            return false;
        }

        switch (e.getAction() & MotionEvent.ACTION_MASK) {
            case (MotionEvent.ACTION_DOWN):
                startScribble(e.getX(), e.getY());
                return true;
            case (MotionEvent.ACTION_CANCEL):
            case (MotionEvent.ACTION_OUTSIDE):
                break;
            case MotionEvent.ACTION_UP:
                finishScribble();
                return true;
            case MotionEvent.ACTION_MOVE:
                int n = e.getHistorySize();
                for (int i = 0; i < n; i++) {
                    addStrokePoints(e.getHistoricalX(i), e.getHistoricalY(i));
                }
                addStrokePoints(e.getX(), e.getY());
                addStrokePointsFinished();
                return true;
            default:
                break;
        }
        return true;
    }

```

control screen update
=====================

eInk screen provides some update mode, there are

* GC update, it flashes the screen to black at first and then flash the screen again to get the best display quality. Usually it takes about 600ms to finish the whole procedure. 
* GU update. different from GC update, it does not update screen to black at first, instead, it flash the screen only once. With GU update, user does not feel flashing with some ghosting. it takes about 600ms to update screen when using GU update.
* DW update. Direct Waveform, you can use DW waveform for scribble. Usually it takes about 200ms to finish one update. Another advantage is that when using DW waveform you may be able to continuous submitting display update request without waiting previous update finished. 

Onyx Android SDK provide very easy way to control screen update, you can ask the device to use DW waveform by following code

```
EpdController.setViewDefaultUpdateMode(view, EpdController.UpdateMode.DW);
``` 

Once switch back from scribble mode, you can use following code to restore update mode
```
EpdController.resetViewUpdateMode(view);
```

lock screen update mode
=========================

Onyx Android SDK also provides a simple way to set the system update mode for all application, you can simply call 
```
EpdController.setSystemUpdateModeAndScheme(EpdController.UpdateMode.DW, EpdController.UpdateScheme.QUEUE_AND_MERGE, Integer.MAX_VALUE);
```
when you're using scribble-style application.


fast screen scribble 
=================

in order to redraw recorded points as fast as possible, you could use dedicated line drawing interface
```
EpdController.moveTo(screenX, screenY, lineWidth);
EpdController.lineTo(screenX, screenY, UpdateMode.DW);
```