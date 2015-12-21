# Get Data Channels

Upon the previous section, we get a list of device. But, the response is `DeviceSummaryEntity`, which contains limited information only for you to display in listview. We need to make another request to get detailed information of single device.


## Get Device Info

Replace `{deviceId}` of `RequestApi.DEVICE` with certain deviceId to get the device info.

```java
/**
 * GET device info.
 */
private void requestDeviceInfo(String deviceId) {
    McsJsonRequest request = new McsJsonRequest(
        RequestApi.DEVICE
            .replace("{deviceId}", deviceId),
        new McsResponse.SuccessListener<JSONObject>() {
            @Override public void onSuccess(JSONObject response) {
                mDeviceInfo = UIUtils.getFormattedGson()
                    .fromJson(response.toString(), DeviceInfoEnitity.class)
                    .getResults()[0];
                    
                // ...
            }
        }
    );

    RequestManager.sendInBackground(request);
}

```


`UIUtils.getFormattedGson()` returns a gson with standard time format.


## `McsDataChannel`

Now you have `DeviceInfoEntity`, use 

```
List<DataChannel> dataChannels = mDeviceInfo.getDataChannels();
```

to get all the data channels of this device. Every single data channel is in format of `DataChannelEntity`.

Next, create an `McsDataChannel`:

```java
/**
 * Optional.
 * Default message of socket update shows in log.
 */
McsSocketListener socketListener = new McsSocketListener(
    new McsSocketListener.OnUpdateListener() {
      @Override public void onUpdate(JSONObject response) {
        // Socket message received
      }
    }
);
DataChannelEntity channelEntity = deviceInfo.getDataChannels().get(0);

mDataChannel = new McsDataChannel(mDeviceInfo, channelEntity, socketListener);
```

Check [Data Channels - MCS Android SDK API Reference][sdk-api-data-channels] to know how to use `McsDataChannel`.

Note that `McsDataChannel` contains `DataChannelEntity`. `DataChannelEntity` is only an entity / data model that describes the format of response. `McsDataChannel` is a wrapper of `DataChannelEntity` with extra funcitons: 

+ [Web Socket](web_socket.md)
+ [Event Emit](event_emit.md)



[sdk-api-data-channels]: https://mtk-mcs.gitbooks.io/mcs-android-sdk-api-reference/content/data_channels.html