## ESP32-S3-N16R8-CAM(OV3660) - Camera Initialization & Capture Test using OV3660 Camera | No WiFi | No WebServer | Beginner-Friendly

While working with the ESP32-S3 CAM (N16R8) module using the OV3660 camera, I noticed that very limited beginner-friendly resources are available—especially for basic Initialization & Capture testing without WiFi, Internet, or CameraWebServer examples, and I realized that jumping directly into complex use cases (like AI, TinyML, Edge Impulse pipelines or streaming) can be confusing when something goes wrong.

This minimal test approach helped me clearly understand:
- Where the problem is (camera, PSRAM, or code)
- Whether the board is actually ready for advanced applications
That’s why I’m sharing this—for anyone starting out with this device.

There’s no need to worry about pin numbers or board model configuration. The code has already been prepared and validated for this specific board and camera module, so no additional modifications are required. This is usually the most confusing step, and mismatches here often cause incorrect results—but that part is already handled.

## What This Code Does (Very Simply)
- Initializes the OV3660 camera
- Checks whether PSRAM is detected
- Captures an image frame
- Prints camera status and image format info on the Serial Monitor
- Does NOT use:
    - WiFi
    - Internet
    - CameraWebServer
    - Image display or streaming

## Note: 
The captured image is not displayed. Instead, the Serial output confirms:
- Frame buffer allocation
- Image resolution
- Image format
This is enough to verify that capture is working correctly.

## Hardware & Software Tools
Hardware
- ESP32-S3 CAM (N16R8) OV3660 Camera Module
- USB cable

Software Tools
- Arduino IDE
- ESP32 Board Core (Arduino)
- Serial Monitor

## Test Flow (Step-by-Step)
Follow these steps to properly test the ESP32-S3 CAM (N16R8) camera setup:
  1. Power Up the Board
      - Connect the ESP32-S3 CAM (N16R8) to your PC using a USB cable(use USB to UART port)
      - Ensure the board is detected by your operating system
        
  2. Arduino IDE Board & Tools Configuration
     Open Arduino IDE and configure the following:
     - Go to Tools → Board
          → Select ESP32S3 Dev Module
     - Go to Tools → Port
          → Select the correct COM port
     - Set the following important options:
          - PSRAM → OPI PSRAM
          - Flash Mode → QIO
          - Flash Frequency → 80 MHz
          - Flash Size → 16 MB
          - Partition Scheme → Default
          - Upload Speed → 921600 (or lower if upload fails)
          - Baud Rate (Serial Monitor) → 115200
            
  3. Compile and Upload the Code
      - Click Verify (Compile) to ensure there are no errors
      - Click Upload to flash the code to the board
      - Wait until the upload completes successfully
  
  4. Open Serial Monitor
      - Set the baud rate to 115200
  
  5. Observe Serial Logs
     Check the Serial Monitor output for:
        - Camera initialization success or failure
        - PSRAM detected and enabled
        - Frame buffer allocation success
        - Image capture confirmation (resolution / format info)
          
  6. Result Validation
        - If the image capture is successful and no errors appear
            → Your camera, PSRAM, and board configuration are correct
        - This confirms the hardware is ready for advanced use cases

 This test helps isolate hardware + configuration issues early, saving a lot of debugging time later.


  ## Code
```cpp
     //ESP32-S3 Initialization & Camera test

    #include <Arduino.h>
    #include "esp_camera.h"
    
    //============= CAMERA MODEL ==============
    #define CAMERA_MODEL_CUSTOM
    
    
    // ESP32-S3 N16R8 OV3660 Camera Pins 
    #define PWDN_GPIO_NUM     -1
    #define RESET_GPIO_NUM    -1
    #define XCLK_GPIO_NUM     15
    #define SIOD_GPIO_NUM     4
    #define SIOC_GPIO_NUM     5
    
    #define Y9_GPIO_NUM       16
    #define Y8_GPIO_NUM       17
    #define Y7_GPIO_NUM       18
    #define Y6_GPIO_NUM       12
    #define Y5_GPIO_NUM       10
    #define Y4_GPIO_NUM       8
    #define Y3_GPIO_NUM       9
    #define Y2_GPIO_NUM       11
    
    #define VSYNC_GPIO_NUM    6
    #define HREF_GPIO_NUM     7
    #define PCLK_GPIO_NUM     13
    

    void setup() {
      Serial.begin(115200);
      delay(2000);
    
      Serial.println("\nESP32-S3 Initialization & Camera test");
    
    
      // ---------- PSRAM CHECK ----------
      if (psramFound()) {
        Serial.println("PSRAM detected successfully");
      } else {
        Serial.println("PSRAM NOT detected");
      }
      // --------------------------------

      camera_config_t config;
      config.ledc_channel = LEDC_CHANNEL_0;
      config.ledc_timer   = LEDC_TIMER_0;
      config.pin_d0       = Y2_GPIO_NUM;
      config.pin_d1       = Y3_GPIO_NUM;
      config.pin_d2       = Y4_GPIO_NUM;
      config.pin_d3       = Y5_GPIO_NUM;
      config.pin_d4       = Y6_GPIO_NUM;
      config.pin_d5       = Y7_GPIO_NUM;
      config.pin_d6       = Y8_GPIO_NUM;
      config.pin_d7       = Y9_GPIO_NUM;
      config.pin_xclk     = XCLK_GPIO_NUM;
      config.pin_pclk     = PCLK_GPIO_NUM;
      config.pin_vsync    = VSYNC_GPIO_NUM;
      config.pin_href     = HREF_GPIO_NUM;
      config.pin_sccb_sda = SIOD_GPIO_NUM;
      config.pin_sccb_scl = SIOC_GPIO_NUM;
      config.pin_pwdn     = PWDN_GPIO_NUM;
      config.pin_reset    = RESET_GPIO_NUM;
    
      config.xclk_freq_hz = 20000000;        //  10 MHz or 20 MHz
      config.pixel_format = PIXFORMAT_RGB565; //  OV3660 SAFE
    
      config.frame_size   = FRAMESIZE_QVGA;  // 320x240
      config.jpeg_quality = 15;
      config.fb_count     = 1;
    
      esp_err_t err = esp_camera_init(&config);
      if (err != ESP_OK) {
        Serial.printf(" Camera init failed: 0x%x\n", err);
        return;
      }
    
      sensor_t *s = esp_camera_sensor_get();
      Serial.print("Sensor PID: 0x");
      Serial.println(s->id.PID, HEX);
    
      // ===== OV3660 specific tuning =====
      s->set_vflip(s, 1);
      s->set_brightness(s, 1);
      s->set_saturation(s, -2);
      // =================================
    
      Serial.println("Camera init OK");
    
      delay(1000);
    
      camera_fb_t *fb = esp_camera_fb_get();
      if (!fb) {
        Serial.println(" Failed to capture image");
        return;
      }

    Serial.println(" Image captured successfully!");
    Serial.print("Image size (bytes): ");
    Serial.println(fb->len);

    esp_camera_fb_return(fb);
    Serial.println("Frame buffer released");
      }
    
      void loop() {
        // nothing
      }
```
          
  
## Conclusion
This project focuses on doing the basics right first.
A properly tested camera setup is the foundation for AI at the edge, computer vision, and real-world embedded applications.



                           --------------------------------------------

                           
[ M. Siratun Nahar , LinkedIn- https://www.linkedin.com/in/most-siratun-nahar-688625299/ ]



