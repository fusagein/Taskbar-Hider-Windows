# TaskBar Hider

A lightweight, standalone Windows GUI application to **disable and hide the TaskBar** while running, with automatic restoration on exit.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Platform](https://img.shields.io/badge/platform-Windows%20x64-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Features

- **Disable TaskBar** — Completely hides and disables input to the Windows TaskBar
- **Enable TaskBar** — Restores the TaskBar instantly
- **System Tray** — Minimizes to tray and keeps running in the background
- **Startup Support** — Optional auto-start with Windows (`-tray` flag)
- **Boot Persistence** — If TaskBar was disabled before reboot, it auto-disables on next boot (only when startup is enabled)
- **Single Instance** — Only one instance runs at a time
- **Dark UI** — Clean, modern dark interface using GDI+
- **Zero Dependencies** — Single portable `.exe` file (~850 KB)

---

## Screenshot

![TaskBar Hider Screenshot](assets/TaskBarHider.png)

---

## Download

Get the latest release from the [Releases](../../releases) page.

No installation required. Just download `TaskBarHider.exe` and run.

---

## How to Use

1. Run `TaskBarHider.exe`
2. Click **Disable TaskBar** to hide the TaskBar
3. Click **Enable TaskBar** to bring it back
4. Click **Close (X)** to minimize to system tray
5. Right-click the tray icon to **Restore** or **Exit**

> **Note:** The TaskBar is automatically restored when you exit the application.

---

## Build from Source

### Requirements

- MinGW-w64 toolchain (`x86_64-w64-mingw32-g++`)
- ImageMagick (optional, for icon generation)

### Compile

```bash
# Compile resources
x86_64-w64-mingw32-windres resource.rc -O coff -o resource.o

# Build executable
x86_64-w64-mingw32-g++ -O2 -static-libgcc -static-libstdc++ -municode \
  TaskBarHider.cpp resource.o -o TaskBarHider.exe \
  -lgdiplus -ldwmapi -lshell32 -lole32 -luser32 -lkernel32 -lgdi32 -lcomctl32 -ladvapi32 -mwindows
```

### One-liner

```bash
x86_64-w64-mingw32-windres resource.rc -O coff -o resource.o && \
x86_64-w64-mingw32-g++ -O2 -static-libgcc -static-libstdc++ -municode \
  TaskBarHider.cpp resource.o -o TaskBarHider.exe \
  -lgdiplus -ldwmapi -lshell32 -lole32 -luser32 -lkernel32 -lgdi32 -lcomctl32 -ladvapi32 -mwindows
```

---

## Project Structure

```
TaskBarHider/
├── TaskBarHider.cpp    # Main source code
├── github_icon.h       # Embedded GitHub logo (PNG byte array)
├── resource.rc         # Windows resource file (icon)
├── app_icon.ico        # Application icon (multi-size)
├── assets/
│   └── TaskBarHider.png    # Screenshot / icon source
├── .gitignore
├── LICENSE
└── README.md
```

---

## Technical Details

- **Language:** C++ (Win32 API + GDI+)
- **Architecture:** x64
- **UI Framework:** Custom GDI+ rendering (no external UI libs)
- **Registry:** Uses `HKCU\Software\TaskBarHider` for state persistence
- **Auto-hide Methods:**
  - `StuckRects3` / `MMStuckRects3` registry modification
  - `SHAppBarMessage(ABM_SETSTATE, ABS_AUTOHIDE)`
  - `EnableWindow(FALSE)` + `SetWindowPos(SWP_HIDEWINDOW)`
  - 100ms enforcement timer while disabled

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Author

**fusagein**

- GitHub: [@fusagein](https://github.com/fusagein)

If you found any issues, please report them on [GitHub Issues](../../issues).

