# Linux

## Prerequisites

You need Python 3.5 and from your package manager's repository.
The other main dependencies can be found in requirements.txt

*For Debian/Ubuntu*

    sudo apt-get install python3-pip python3-dev python3-virtualenv python3-gi libasound2-dev

*For openSUSE*

    sudo zypper install python3-pip python3-virtualenv python3-gi

*All Distributions*

To be able to communicate with your Arduino, it might be necessary to add the rights for USB communication by adding your user to some groups.

    sudo usermod -a -G tty [userName]
    sudo usermod -a -G dialout [userName]

## Installation

Checkout the git repository

    git clone https://github.com/AllYarnsAreBeautiful/ayab-desktop

Create a virtual enviroment in the cloned repository

    cd ayab-desktop
    virtualenv -p python3 --system-site-packages venv/
    source venv/bin/activate
    pip3 install -r requirements.txt

Now start ayab with

    python3 -m fbs run

# Windows

Requires Windows 10 or Windows 7

The Windows setup is available at 
[ayab-knitting.com](https://ayab-knitting.com/ayab-software/).  
Run the setup, install AYAB and run it with the icon on your Desktop.

Important:

- When choosing the installation directory, make sure that you do not overwrite any previous versions. Remove them or use another folder for installation.
- Currently, no spaces in the installation path are allowed.

# macOS

Requires macOS 10.12 or newer

Please make sure that you have installed the [SiLabs CP210x "VCP" Driver](http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx) - it is required for use with most new boards.

Download the DMG image from [ayab-knitting.com](https://ayab-knitting.com/ayab-software/), open the DMG image and drag&drop the app to your Application folder.
Then run AYAB from your Application folder.

Important:

- In case macOS tells you the application can't be opened because it's from
  an unidentified developer, just Ctrl+Click it and choose "Open".
