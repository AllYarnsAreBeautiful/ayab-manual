# Linux

## Prerequisites

You need Python 3.5 and from your package manager's repository.
The other main dependencies can be found in requirements.txt

*For Debian/Ubuntu*

    sudo apt-get install python3-pip python3-dev python3-virtualenv python3-gi

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

The Windows version which is available at [ayab-knitting.com](https://ayab-knitting.com) has been packed with
PyInstaller and should not require any additional dependencies.
Just unzip the archive or use the Installer and run

    C:\AYAB\AYAB.exe

# macOS

Download the DMG image from [ayab-knitting.com](https://ayab-knitting.com), open the DMG image and copy the app to your Application folder.
Then just run

    AYAB
