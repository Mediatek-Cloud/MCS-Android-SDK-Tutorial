## Extra function - Event Emit (optional)

Another mechanism inside `McsDataChannel` is event emitting. Whenever there is an update on the data point, the emitter will emit a `DataPointEvent`, anyone who is interested in this event could listen to it.

we wraps [EventBus][eventbus] to achieve event emitting. All you need to do is prepare subscribers in Android life cyle.

### Setup

```java
@Override protected void onStart() {
    super.onStart();
    EventBus.getDefault().register(this);
}

@Override protected void onStop() {
    EventBus.getDefault().unregister(this);
    super.onStop();
}

/**
 * must follow the same signature: 
 *
 * public void onEvent(DataPointEvent event)
 */
public void onEvent(DataPointEvent event) {
    McsLog.d(event.channelId 
        + " has changed with data point: " 
        + event.dataPointEntity
        + ". You may want to update view now.");
}
```


Note: `DataPointEvent` was emitted when `mcsDataChannel.setDataPointEntity()`: 

```java
// McsDataChannel.java

public void setDataPointEntity(DataPointEntity dp) {
    if (dp != null) {
      mDataChannelEntity.setDataPoint(dp);
      EventBus.getDefault().post(new DataPointEvent(getChannelId(), dp));
    }
}
```


## Proguard

To keep Event Emit works well, add the following lines to your project’s `proguard.cfg` file:

```
-keepclassmembers class ** { public void onEvent*(**); }
```


[eventbus]: https://github.com/greenrobot/EventBus