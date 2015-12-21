# Get & Submit Data Point

Upon [API Reference][mcs-api], a data point contains:

+ `recordedAt`: Unix timestamp (time in millis) of the data point.
+ `Values`: Values object of the data point.

## Get Data Point

```java
DataPointEntity datapoint = mDataChannel.getDataPointEntity();
```

## Submit Data Point

```java
String data = et_submit_data_point.getText().toString();
mDataChannel.submitDataPoint(new DataPointEntity.Values(data));
```

## `Values`

Currently, there are 3 kinds of `Values` 

+ `Values(String value)`
+ `Values(String value, String period)`
+ `Values(McsGeoPoint geoPoint)`

Every data channel has its own type of data point. It's necessary to put `Values` in the right format to submit data points successfully.

Check [Data Points - MCS Android SDK API Reference][sdk-api-data-points] to know how to put correct `Values` into the data point.



[mcs-api]: https://mcs.mediatek.com/resources/latest/api_references/
[sdk-api-data-points]: https://mtk-mcs.gitbooks.io/mcs-android-sdk-api-reference/content/data_points.html