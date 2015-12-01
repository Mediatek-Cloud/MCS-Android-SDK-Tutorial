# Get Device List

To get the list of your IoT devices stored in MCS, you will need to make a request to fetch these data from MCS API server. This section shows you how to make a proper request, get the desired response and parse it right.

## Send a Request

If you are familiar with [Volley][volley], the open source network request library created by Google, hooray! It's almost the same to use MCS SDK. 

```java
// Default method is GET 
int method = McsJsonRequest.Method.GET;
String url = RequestApi.DEVICES;
McsResponse.SuccessListener<JSONObject> successListener =
    new McsResponse.SuccessListener<JSONObject>() {
      @Override public void onSuccess(JSONObject response) {
        DeviceSummaryEntity[] summary = new Gson().fromJson(
            response.toString(), DeviceSummaryEntity.class).getResults();
            
        // ...
      }
};

/**
 * Optional.
 * Default error message shows in log.
 */
McsResponse.ErrorListener errorListener = new McsResponse.ErrorListener() {
  @Override public void onError(Exception e) {
    // network request failed
  }
};

McsJsonRequest request = new McsJsonRequest(method, url, successListener, errorListener);
RequestManager.sendInBackground(request);
```

### `RequestApi`

To see what API is available, you can check [API References of MCS API server][mcs-api] on our official website. Also, we provide `RequestApi` class that integrates all the APIs that has opened for you.

### Response and Entities 

Every success response of `McsJsonRequest` is of type `JSONObject`. We have provided a set of entities to simplify the serialization and deserialization of these network requests.

With [Gson][gson], you can get the object by specifing the correct class: 

```java
DeviceSummaryEntity[] summary = new Gson().fromJson(
    response.toString(), DeviceSummaryEntity.class).getResults();
```

Check [Entities - MCS SDK Android Guide][sdk-guide-entities] for detailed explaination.



[mcs-api]: https://mcs.mediatek.com/resources/latest/api_references/
[sdk-guide-entities]: https://mtk-mcs.gitbooks.io/mcs-sdk-android-guide/content/entities.html

[volley]: https://android.googlesource.com/platform/frameworks/volley/
[gson]: https://github.com/google/gson