# What's new in AYAB 1.0

Apart from a number of bug fixes, the following are the main changes in AYAB 1.0 compared to AYAB 0.95:

## User interface
- A **Preferences** dialog has been introduced, allowing you to set defaults for most settings. It is opened through the **Preferences** menu (on Windows and Linux), or the **Preferences** menu item in the **AYAB** menu on macOS. **Make sure you select the correct machine type in this menu before knitting.**
- The user interface has been translated into several non-English languages. Open the **Preferences** dialog to select a language.
- The **Configure** button has been removed, you can now directly click the **Knit** button.
- Needles on the left and right sides of the bed are now called **Left** and **Right** instead of **Orange** and **Green**.
- A **Knit progress** panel has been added below the main panel. It shows needle selection for each row as it is sent to the machine. The **Preferences** dialog lets you adjust its zoom level.
- The aspect ratio of the main view can be changed (in the **Preferences** dialog) from square (**1:1**) to slightly widened (**4:5**) to reflect the appearance of actual stitches.
- In addition to the hardware beeper, the AYAB desktop application now also emits **sounds** to signal the start of knitting, end of row and end of knitting. Both the hardware beeper and desktop sounds can be disabled independently in the **Preferences** dialog.

## Knitting Machine support
- The **Garter carriage** is now supported. Any two-color pattern can be used to select between knit and purl stitches.
- The **Brother KH-270** bulky knitting machine is now supported.

## Features
- AYAB no longer automatically flips images horizontally. Now, the pixels you see on the left of your screen correspond to needles on the left of the machine as you knit. If you want your image to appear "as-is" on the knit ("right") side of the work, if it contains text for example, you need to flip it first, which you can easily do with the new **Knit Side Image** checkbox. You can set this option to be checked by default in the **Preferences** dialog if you want to default to the previous AYAB behavior.
- On most supported knitting machine models (see Known Issues for details), you can now start knitting with the carriage at the right end of the machine, not just the left end.
- Communications between the AYAB desktop application and the machine have been optimized, such that the **initial 2-second delay** has now been **removed**, and you also no longer have to wait at the end of each row — a beep will sound almost as soon as the carriage is past the last needle, and you can turn around as quickly as you like.
- A **Simulation** mode now lets you preview the needle selections that the machine will do while knitting the pattern, even when no machine is connected. It can be accessed by selecting **Simulation** as the **Port** in the settings side panel.
- You can now load patterns from **DesignAKnit** files in the `.pat` and `.stp` formats. Note that only the image content is loaded, no shaping or other instructions.
- Two new **Image Actions** have been added: **Stretch** and **Reflect**.
- In multicolor (double-bed Jacquard) modes, the colors are now ordered by **frequency** (number of pixels) instead of lightness.

## System and packaging
- AYAB now requires **Windows 10 or newer**, **macOS 11 or newer**, or **Ubuntu Linux 22.04 or equivalent/newer**.
- A Linux **AppImage** is now available, so that Linux users no longer have to build AYAB from source.
- The AYAB firmware is now shared between all Brother machine models. You no longer select a machine model in the **Firmware Flashing** dialog; it is selected in the **Preferences** dialog and can be changed without having to flash the firmware again.
- **Arduino Mega** boards are no longer supported. Current supported hardware is the **[AYAB Shield](https://www.ayab-knitting.com/ayab-shield/)** used with an **Arduino Uno R3** board, and the EMSL **[AYAB Interface](https://www.ayab-knitting.com/ayab-interface/)**.
- A new **Hardware Test** dialog is available from the **Tools** > **Test AYAB Device** menu item. It lets you check communication with the AYAB hardware.
- The AYAB application now checks for **new releases** on startup and will prompt you to upgrade if a new release is available.

## Known issues
- Starting on the right with the lace and garter carriages does not work on KH-910 and KH-950, due to a hardware limitation in the AYAB Shield/Interface. We're looking into ways to upgrade existing hardware to support detection of all carriages on the right.
- Unplugging the USB cable during usage may cause a crash of the application.
- The application does not prevent the activation of a screensaver, make sure to disable it before starting to knit.

## Get the latest software
You can find the latest release on [GitHub](https://github.com/AllYarnsAreBeautiful/ayab-desktop/releases/latest). Choose the correct binary for your operating system (`.exe` for Windows, `.dmg` for macOS, `.AppImage` for Linux).
