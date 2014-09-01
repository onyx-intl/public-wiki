# 流程

基本分成两个过程 activate 和 download

# init reader for drm 

```
reader.selectDRMPluginByFile(acsmFilePath);
```

# 激活activation

设备只有经过activate之后才能正常使用drm功能 

```
    reader.sendRequest(this, ReaderRequest.drmActivateRequest(userId, password), new ReaderCallback() {
                @Override
                public void done(ReaderRequest request, ReaderException e) {
                    if (e != null) {
                        showActivationErrorMessage(e.getMessage());
                        return;
                    }

                    mIsActivationSucceeded = true;
                    activateDialog.dismiss();
                    changeUI(userId);
                }
            });
```
