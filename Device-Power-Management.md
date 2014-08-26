# disable standby timeout

```
   
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

# change shutdown timeout

# change wifi idle tiemout
