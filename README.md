# BluetoothSerial

**Java class for Android framework.**
*BluetoothSerial class provides basic functionality to communicate with the remote serial interface via Bluetooth channel. Below is example, how to extend and use this class:*

1. BluetoothSerial provide handler events for connecting/disconnecting. To implement your own custom events based on received or send data, extend BluetoothSerial class:



    public class BluetoothService extends  BluetoothSerial {
    
        // implement your custom messages based on OnReceive data
        public static final int CUSTOM_MESSAGE = 2;
    
    
        public BluetoothService(Handler handler) {
            super(handler);
        }
    
        @Override
        protected void onReceived(byte[] data) {
    
        }
    
        public void sendData(byte[] data){
            super.write( data );
        }
    
    
    }
         

 
2. Implement Handler, which will handle incoming events and passes it to host activity or fragment:

    public class BluetoothSerialFragment extends Fragment {
    
      
        private BluetoothService mBluetoothService;
  
    
     
    
        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
    
            
	  
            Handler serviceHandler = new BtServiceHandler(this);
            mBluetoothService = new BluetoothService(serviceHandler);
        }
    
     
        @Override
        public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
            super.onViewCreated(view, savedInstanceState);
    
        
            view.findViewById(R.id.btnSend).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    mBluetoothService.sendData(new byte[]{0x01, 0x00});
                }
            });
   
        }
    
        // Static handler with weak reference to target activity
        private static class BtServiceHandler extends Handler {
    
            private WeakReference<BluetoothSerialFragment> mTarget;
    
            public BtServiceHandler(BluetoothSerialFragment context) {
                mTarget = new WeakReference<>(context);
            }
    
            @Override
            public void handleMessage(Message msg) {
    
                BluetoothSerialFragment target = mTarget.get();
    
                if (target != null) {
                    switch (msg.what) {
    
                        case BluetoothService.CUSTOM_MESSAGE:
                            // perform custom actions
                            break;
    
                        case BluetoothService.MESSAGE_STATE_CHANGE:
    
                            switch (msg.arg1) {
                                case BluetoothSerial.STATE_CONNECTED:
                                    target.setState("Connected!");
                                    break;
                                case BluetoothSerial.STATE_CONNECTING:
                                    target.setState("Connecting..");
                                    break;
                                case BluetoothSerial.STATE_NONE:
                                    target.setState("Disconnected.");
                                    break;
                            }
                            break;
    
                    }
                }
            }
        }
    }


