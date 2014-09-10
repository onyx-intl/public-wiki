# using resolution to check device type

````
public class MyApp extends Application{

    public static int displayWidth, displayHeight;
    public static int INCH_8_WIDTH = 1200;
    public static int INCH_97_WIDTH = 825;
    public static int INCH_68_WIDTH = 1080;
    public static int INCH_68_HEIGHT = 1440;


    @Override
    public void onCreate() {
        super.onCreate();
        SingletonSharedPreference.init(this);
        ReaderDeviceInfo.init(this);

        WindowManager wm = (WindowManager)getSystemService(Context.WINDOW_SERVICE);
        Display display = wm.getDefaultDisplay();
        displayWidth = display.getWidth();
        displayHeight = display.getHeight();
    }

    static public boolean is8Inch() {
        return displayWidth == INCH_8_WIDTH;
    }

    static public boolean is97Inch() {
        return displayWidth == INCH_97_WIDTH;
    }

    static public boolean is68Inch() {
        return displayWidth == INCH_68_WIDTH;
    }
}
```

# using device model

```
String model = Build.MODEL;
model = model.toLowerCase();
```

