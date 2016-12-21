# BluetoothSerial

Java class for Android framework.
BluetoothSerial class provides basic functionality to communicate with the remote serial interface via Bluetooth channel. Below is example, how to extend and use this class:
1. BluetoothSerial provide handler events for connecting/disconnecting. To implement your own custom events based on received or send data, extend BluetoothSerial class: 
2. Implement Handler, which will handle incoming events and passes it to host activity or fragment:
