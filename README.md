# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the client/server services using AIDL”.
Developed by: Naveen Kumar V
Registeration Number : 212223220068
*/
```
## ColorService.java (server):
```
package com.example.server;

import android.app.Service;
import android.content.Intent;
import android.graphics.Color;
import android.os.IBinder;
import android.os.RemoteException;

import java.util.Random;

public class ColorService extends Service {
    private final Random random = new Random();

    private final IColorService.Stub binder = new IColorService.Stub() {
        @Override
        public int getRandomColor() throws RemoteException {
            return Color.rgb(
                    random.nextInt(256),
                    random.nextInt(256),
                    random.nextInt(256)
            );
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}

```
## MainActivity.java(client):
```
package com.example.client;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import com.example.server.IColorService;

public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";

    private IColorService colorService;
    private boolean isBound = false;
    private View mainLayout;
    private Button colorButton;

    private final ServiceConnection serviceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            colorService = IColorService.Stub.asInterface(service);
            isBound = true;
            Toast.makeText(MainActivity.this, "Connected to Color Service", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            colorService = null;
            isBound = false;
            Toast.makeText(MainActivity.this, "Disconnected from Color Service", Toast.LENGTH_SHORT).show();
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        mainLayout = findViewById(R.id.main);
        colorButton = findViewById(R.id.colorButton);

        ViewCompat.setOnApplyWindowInsetsListener(mainLayout, (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        colorButton.setOnClickListener(v -> {
            if (isBound && colorService != null) {
                try {
                    colorButton.setBackgroundColor(colorService.getRandomColor());
                } catch (RemoteException e) {
                    Log.e(TAG, "Error calling remote service", e);
                    Toast.makeText(MainActivity.this, "Error calling remote service", Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(MainActivity.this, "Service not bound", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();
        Intent intent = new Intent("com.example.server.IColorService");
        intent.setPackage("com.example.server");
        bindService(intent, serviceConnection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();
        if (isBound) {
            unbindService(serviceConnection);
            isBound = false;
        }
    }
}

```
## IColorService.aidl:
```
package com.example.server;

interface IColorService {
    int getRandomColor();
}
```
## OUTPUT

## server
<img width="540" height="1133" alt="WhatsApp Image 2026-11-02 at 9 11 11 AM" src="https://github.com/user-attachments/assets/4603ac26-2007-4c33-825f-fcbea28ac33c" />

## client
<img width="540" height="1200" alt="WhatsApp Image 2026-11-02 at 8 14 07 AM" src="https://github.com/user-attachments/assets/21b36bc1-2c3e-47cd-b199-f935d0b42b51" />
<img width="540" height="1200" alt="WhatsApp Image 2026-11-02 at 8 14 08 AM" src="https://github.com/user-attachments/assets/e129162a-00b1-42b0-9c49-d75e4fcc2c87" />


## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
