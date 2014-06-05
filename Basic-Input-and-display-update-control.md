# device input event. 

Onyx device uses standard Android input framework. So basically, developer can use MotionEvent to receive input from device.  You can setup a touch listener to your view by using following code

```
        yourView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent event) {
                onTouchEvent(this, event);
                return true;
            }
        });

    @Override
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


