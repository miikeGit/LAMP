# MediaPlayer

A modern, hardware-accelerated C++ WinUI 3 media player featuring integrated magnet/torrent streaming, direct ZIP archive playback, real-time shader effects, and advanced audio processing.

Built on top of **Windows App SDK (WinUI 3)** and **C++20**, this application bypasses standard media engine limitations by employing a custom **FFmpeg** and **Direct3D 11** decoding and rendering pipeline.

---

## 🚀 Key Features

* **Custom Decoding Pipeline:** Advanced video and audio decoding using **FFmpeg**, supporting custom stream selectors, subtitle extraction, and asynchronous multi-threaded frame processing.
* **Hardware-Accelerated Rendering:** Smooth, high-performance video rendering powered by **Direct3D 11** with custom vertex and pixel shaders (supporting real-time effects like Grayscale).
* **Torrent & Magnet Streaming:** Stream and play video files directly from magnet links or `.torrent` tracker files sequentially without downloading the entire package first, using **libtorrent**.
* **Direct ZIP Playback:** Play media files stored directly inside compressed `.zip` packages on-the-fly without manual extraction, utilizing **libarchive** and FFmpeg's `AVIOContext`.
* **Advanced Audio Processing:** Adjust playback speed/tempo in real-time without modifying pitch, and apply environment effects (like Reverb) via **SoundTouch** and **XAudio2**.
* **Cloud Integration:** Integrated authentication and direct URL streaming for files hosted on **Microsoft OneDrive** via Microsoft Graph API.
* **A-B Repeat & Progress Saving:** Set specific loops for repeat playback and automatically cache playback progress to resume videos where you left off.

---

## 🛠️ Technology Stack & Dependencies

The project manages its native C++ dependencies using **vcpkg** (Manifest Mode). The dependencies include:

* **FFmpeg (v6.1+):** For video, audio, and subtitle decoding.
* **libtorrent (v2.0+):** For magnet link and torrent swarm handling.
* **libarchive:** For ZIP container streaming and parsing.
* **SoundTouch:** For pitch-preserving playback speed modifications.
* **DirectXTK:** DirectX Tool Kit for Direct3D 11 resource management and utility functions.
* **Windows Implementation Library (WIL):** For convenient C++/WinRT helper structures.

---

## 📋 Requirements

* **Operating System:** Windows 10 (version 1809, build 17763) or newer.
* **IDE:** Visual Studio 2022 (with the **Desktop Development with C++** and **Windows App SDK** workloads installed).
* **Compiler:** MSVC with C++20 standard support.
* **Package Manager:** `vcpkg` integrated with Visual Studio.

---

## 🔨 Build & Package Instructions

### 1. Restore Dependencies
Make sure you have `vcpkg` installed and integrated with Visual Studio:
```powershell
vcpkg integrate install
```
Opening the solution or project in Visual Studio will automatically trigger `vcpkg` manifest restoration for the dependencies listed in `vcpkg.json`.

### 2. Build the Project
1. Open the solution folder or project file in Visual Studio.
2. Select the desired configuration (e.g., **Release**) and target platform (e.g., **x64**).
3. Build the solution (**Build -> Build Solution**).

### 3. Deploy & MSIX Packaging
This application is configured as a packaged **Single-project MSIX** desktop bridge application. 
During packaging, a custom MSBuild Target automatically collects the dynamic DLL dependencies from `vcpkg` and injects them into the MSIX package payload so they are correctly bundled inside the application container:

* **Sideload Package Generation:** To generate an unsigned `.msixbundle` installer, run the following command or use the Visual Studio Packaging wizard:
  ```powershell
  msbuild MediaPlayer.vcxproj /t:Build /p:Configuration=Release /p:Platform=x64 /p:GenerateAppxPackageOnBuild=true
  ```
  The generated installer will be located in the `AppPackages/` folder.

---

## 📂 Project Structure

```
D:\LAMP
│   app.manifest                # Application assembly manifest
│   App.xaml / App.xaml.cpp     # WinUI 3 App entry point and setup
│   MainWindow.xaml / .cpp      # Main application window, UI layout, and event handlers
│   MediaPlayer.vcxproj         # Main MSBuild C++ project file
│   Package.appxmanifest        # MSIX packaging manifest
│   vcpkg.json                  # C++ dependency declarations
│
├───include/                    # C++ Header files
│       ArchiveClient.h         # ZIP extraction helper
│       FFmpegPlayer.h          # Custom FFmpeg playback controller
│       IPlayer.h               # Core player abstract interface
│       PacketQueue.h           # Thread-safe decoding packet queue
│       srtparser.h             # External subtitle parser
│       TorrentClient.h         # Torrent sequential downloader
│
├───src/                        # C++ Source implementations
│       ArchiveClient.cpp
│       FFmpegPlayer.cpp
│       IPlayer.cpp
│       PacketQueue.cpp
│       TorrentClient.cpp
│
├───shaders/                    # D3D11 HLSL Shader source files
│       NormalShader.hlsl
│       GrayscaleShader.hlsl
│       VertexShader.hlsl
│
└───Assets/                     # Icons, app tiles, and helper binaries (like yt-dlp)
```
