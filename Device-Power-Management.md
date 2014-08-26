# change power management timeout

```
   
public static final String CLOSE_WIFI_DELAY = "close_wifi_delay";
public static final String SCREEN_OFF_TIMEOUT = "screen_off_timeout";
public static final String AUTO_POWEROFF_TIMEOUT = "auto_poweroff_timeout";

private static final int FALLBACK_SCREEN_TIMEOUT_DEFAULT_VALUE = -1;
private void disableSystemPMSettings() {
    ContentResolver resolver = YourActivity.this.getContentResolver();
    int currentTimeout = Settings.System.getLong(resolver, SCREEN_OFF_TIMEOUT, FALLBACK_SCREEN_TIMEOUT_DEFAULT_VALUE);
    if (currentTimeout != FALLBACK_SCREEN_TIMEOUT_DEFAULT_VALUE) {
        try {
            Settings.System.putLong(getContentResolver(), SCREEN_OFF_TIMEOUT, FALLBACK_SCREEN_TIMEOUT_DEFAULT_VALUE);
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }
    }
}
```
