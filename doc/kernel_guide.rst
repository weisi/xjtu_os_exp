Guide on Compiling a Custom Kernel on Ubuntu 11.10
=======================================================

:Author:    multiple1902 <multiple1902@gmail.com>
:Copyright: `Creative Commons BY-NC-SA 2.0 License <http://creativecommons.org/licenses/by-nc-sa/2.0/>`_

#. Install Ubuntu 11.10 on your computer, and boot into it. Install the essential parts::

    sudo apt-get install build-essential libncurses5-dev

#. Download linux-x-y-z.tar.bz2 from kernel.org::

    linux-3.2.9.tar.bz2

#. eXtract the archive::

    tar jxf linux-3.2.9.tar.bz2
    cd linux-3.2.9

#. Copy the effective `.config` file here::

    cp /boot/config-`uname -r` ./.config

#. Do some modifications (remember to SAVE before exit)::

    make menuconfig

#. Build it (assume we have 4 CPU threads here)::

    make modules bzImage -j5

#. Install it::

    sudo make modules_install install

#. Load it::

    sudo reboot
