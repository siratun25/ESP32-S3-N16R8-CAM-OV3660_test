# ESP32-S3-N16R8-CAM(OV3660)_test
Camera Initialization & Capture Test  Using OV3660 Camera | No WiFi | No WebServer | Beginner-Friendly


While working with the ESP32-S3 CAM (N16R8) module using the OV3660 camera, I noticed that very limited beginner-friendly resources are available—especially for basic camera testing without WiFi, Internet, or CameraWebServer examples and I realized that jumping directly into CameraWebServer or AI examples can be confusing when something goes wrong.
This minimal test approach helped me clearly understand:
- Where the problem is (camera, PSRAM, or code)
- Whether the board is actually ready for advanced applications
That’s why I’m sharing this—for anyone starting out with this device.


So, before moving into complex use cases (like AI, TinyML, Edge Impulse pipelines or streaming), I wrote a minimal and clean camera test code to verify:
- Camera initialization status
- Image capture success
- PSRAM availability and proper usage
This repository shares that simple testing process and code, so that beginners can confidently validate their hardware setup first.

# What This Code Does (Very Simply)
- nitializes the OV3660 camera
- Checks whether PSRAM is detected
- Captures an image frame
- Prints camera status and image format info on the Serial Monitor
- Does NOT use:
    - WiFi
    - Internet
    - CameraWebServer
    - Image display or streaming

# Note: 
The captured image is not displayed. Instead, the Serial output confirms:
- Frame buffer allocation
- Image resolution
- Image format
This is enough to verify that capture is working correctly.

# Hardware & Software Tools
Hardware
- ESP32-S3 CAM (N16R8) OV3660 Camera Module
- USB cable

Software Tools
- Arduino IDE
- ESP32 Board Core (Arduino)
- Serial Monitor


# Test Flow (Step-by-Step)
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


# What You Can Learn From This
By running this code, beginners can learn:
- How ESP32-S3 camera initialization works internally
- How PSRAM is involved in image capture
- How to debug camera issues using Serial logs
- How to confirm hardware readiness before advanced projects


# Conclusion
This project focuses on doing the basics right first.
A properly tested camera setup is the foundation for AI at the edge, computer vision, and real-world embedded applications.




