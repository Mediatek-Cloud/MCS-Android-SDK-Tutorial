## Extra function - Web Socket (optional)

`McsDataChannel` has data point inside, and it can change over time. If you want to get the data point updated instantly, you can use socket to achieve that. 

`SocketManager` wraps [Socket.io][socket.io]. 

### Setup web socket 

```java
McsDataChannel mDataChannel;

private void turnOnSocket() {
    SocketManager.connectSocket();  // connect before register
    SocketManager.registerSocket(mDataChannel, mDataChannel.getMcsSocketListener());
}

private void turnOffSocket() {
    SocketManager.unregisterSocket(mDataChannel, mDataChannel.getMcsSocketListener());
    SocketManager.disconnectSocket();   // disconnect after unregister
}
```

Note that `McsDataChannel` has a default `McsSocketListener`. But, you still **NEED to manually connect the socket** in the life cycle (`onStart()`, `onStop()`, etc.) of your Android application. If you do not, then the socket will not consume any bandwidth. It's up to you when to turn the socket on and off.

### `McsSocketListener`

`McsSocketListener` extends `Emitter.Listener` from socket.io. You have to define an `OnUpdateListener` for `McsSocketListener` when the socket is receiving an object. 

```java
McsSocketListener defaultSocketListener = new McsSocketListener(
    new OnUpdateListener() {
        @Override public void onUpdate(JSONObject data) {
            try {
                JSONObject data = response.getJSONObject("updateDatapoint");
                DataPointEntity dp = new Gson().fromJson(data.toString(), DataPointEntity.class);

                setDataPointEntity(dp);
            } catch (JSONException e) {
                McsLog.e(e.toString());
            }
        }
    }
);
```

### Multiple data channels

If you have multiple data channels, which is often the case, you'll have to register/unregister them separately. 
For example, you may need to maintain a list of `McsDataChannel`:

```java
List<McsDataChannel> channels;

private void turnOnSocket() {
    SocketManager.connectSocket();
    for (McsDataChannel channel : channels) {
        SocketManager.registerSocket(channel, channel.getMcsSocketListener());
    }
}

private void turnOffSocket() {
    for (McsDataChannel channel : channels) {
        SocketManager.registerSocket(channel, channel.getMcsSocketListener());
    }
    SocketManager.disconnectSocket();
}
```



[socket.io]: https://github.com/socketio/socket.io-client-java