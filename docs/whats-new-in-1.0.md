# What's new in AYAB 1.0

Apart from a number of bug fixes, the following are the main changes in AYAB 1.0 compared to AYAB 0.95:

## User interface
- A **Preferences** dialog has been introduced, allowing you to set defaults for most settings. It is opened through the **Preferences** menu (on Windows and Linux), or the **Preferences** menu item in the **AYAB** menu on macOS.
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
- Communications between the AYAB desktop application and the machine have been optimized, such that the **initial 2-second delay** has now been **removed**, and you also no longer have to wait at the end of each row — a beep will sound almost as soon as the carriage is past the last needle, and you can turn around as quickly as you like.
- A **Simulation** mode now lets you preview the needle selections that the machine will do while knitting the pattern, even when no machine is connected. It can be accessed by selecting **Simulation** as the **Port** in the settings side panel.
- You can now load patterns from **DesignAKnit** files in the `.pat` and `.stp` formats. Note that only the image content is loaded, no shaping or other instructions.
- Two new **Image Actions** have been added: **Stretch** and **Reflect**.

## System and packaging
- AYAB now requires **Windows 10 or newer**, **macOS 11 or newer**, or **Ubuntu Linux 22.04 or equivalent/newer**.
- A Linux **AppImage** is now available, so that Linux users no longer have to build AYAB from source.
- The AYAB firmware is now shared between all Brother machine models. You no longer select a machine model in the **Firmware Flashing** dialog; it is selected in the **Preferences** dialog and can be changed without having to flash the firmware again.
- **Arduino Mega** boards are no longer supported. Current supported hardware is the **[AYAB Shield](https://www.ayab-knitting.com/ayab-shield/)** used with an **Arduino Uno** board, and the EMSL **[AYAB Interface](https://www.ayab-knitting.com/ayab-interface/)**.
- A new **Hardware Test** dialog is available from the **Tools** > **Test AYAB Device** menu item. It lets you check communication with the AYAB hardware.
- The AYAB application now checks for **new releases** on startup and will prompt you to upgrade if a new release is available.

## Get the latest software

The latest release is [1.0.0-beta5](https://github.com/AllYarnsAreBeautiful/ayab-desktop/releases/tag/1.0.0-beta5). Choose the correct binary for your operating system (.exe for windows, .dmg for mac)
