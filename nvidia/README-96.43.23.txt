NVIDIA Accelerated Linux Driver Set README and Installation Guide

    NVIDIA Corporation
    Last Updated: 2010/11/22
    Most Recent Driver Version: 96.43.23
______________________________________________________________________________

Preface
______________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set brings accelerated 2D functionality
and high-performance OpenGL support to Linux x86 with the use of NVIDIA
graphics processing units (GPUs).

These drivers provide optimized hardware acceleration for OpenGL and X
applications and support nearly all recent NVIDIA graphics chips (please see
Appendix A for a complete list of supported chips). TwinView, TV-Out and flat
panel displays are also supported.

This README describes how to install, configure, and use the NVIDIA
Accelerated Linux Driver Set. Answers to frequently asked questions and
problem diagnoses for common issues are also provided. These pages are posted
on NVIDIA's web site (http://www.nvidia.com), and are installed in
'/usr/share/doc/NVIDIA_GLX-1.0/'.

______________________________________________________________________________

Introduction
______________________________________________________________________________

This document provides instructions for the installation and use of the NVIDIA
Accelerated Linux Driver Set. Chapter 1, Chapter 2 and Chapter 3 walk the user
through the process of downloading, installing and configuring the driver.
Chapter 4 addresses frequently asked questions about the installation process,
and Chapter 5 provides solutions to common problems.

In case additional information is required, Chapter 6 provides contact
information for NVIDIA Linux driver resources, and Chapter 7 provides a brief
listing of external resources.

It is assumed that the user has at least a basic understanding of Linux
techniques and terminology. However, Chapter 8 provides details on parts of
the installation process that new users may find helpful.

Additional information is presented in several Appendices. These include
supported hardware and system requirements, comprehensive lists of options for
various utilities associated with the driver, setup details for specific
configurations, and advanced topics and features.

CONTENTS:

    Preface
    Introduction
    I. Installation Instructions
        1. Selecting and Downloading the NVIDIA Packages for Your System
        2. Installing the NVIDIA Driver
        3. Configuring X for the NVIDIA Driver
    II. Additional Information
        4. Frequently Asked Questions
        5. Common Problems
        6. NVIDIA Contact Info
        7. Additional Resources
        8. Tips for New Linux Users
        9. Acknowledgements
    III. Appendices
        A. Supported NVIDIA Graphics Chips
        B. Minimum Software Requirements
        C. Installed Components
        D. X Config Options
        E. OpenGL Environment Variable Settings
        F. Configuring AGP
        G. Configuring TwinView
        H. Configuring TV-Out
        I. Configuring a Laptop
        J. Programming Modes
        K. Flipping and UBB
        L. Known Issues
        M. Proc Interface
        N. XvMC Support
        O. GLX Support
        P. Configuring Multiple X Screens on One Card
        Q. Power Management Support
        R. Display Device Names
        S. The X Composite Extension
        T. The nvidia-settings Utility
        U. The XRandR Extension
        V. Support for GLX in Xinerama
        W. SLI and MultiGPU FrameRendering
        X. Frame Lock and Genlock
        Y. Dots Per Inch
        Z. i2c Bus Support
        AA. Allocating DMA Buffers on 64-bit Platforms

______________________________________________________________________________

Chapter 1. Selecting and Downloading the NVIDIA Packages for Your System
______________________________________________________________________________

NVIDIA drivers can be downloaded from the NVIDIA website
(http://www.nvidia.com).

The NVIDIA driver follows a Unified Architecture Model in which a single
driver set is used for all supported NVIDIA graphics chips (please see
Appendix A for a list of supported chips). The burden of selecting the correct
driver is removed from the user, and the driver set is downloaded as a single
file named

     'NVIDIA-Linux-x86-96.43.23-pkg1.run'

The package suffix ('-pkg#') is used to distinguish between packages
containing the same driver, but with different precompiled kernel interfaces.
The file with the highest package number is suitable for most installations.

Support for "legacy" GPUs has been removed from the unified driver. These
legacy GPUs will continue to be maintained through special legacy GPU driver
releases. Please see Appendix A for a list of legacy GPUs.

The downloaded file is a self-extracting installer, and you may place it
anywhere on your system.

______________________________________________________________________________

Chapter 2. Installing the NVIDIA Driver
______________________________________________________________________________

This chapter provides instructions for installing the NVIDIA driver. Note that
after installation, but prior to using the driver, you must complete the steps
described in Chapter 3. Additional details that may be helpful for the new
Linux user are provided in Chapter 8.

BEFORE YOU BEGIN

Prior to beginning the installation, you should exit the X server and kill all
OpenGL applications (note that it is possible that some OpenGL applications
persist even after the X server has stopped). You should also set the default
run level on your system such that it will boot to a VGA console, and not
directly to X. Doing so will make it easier to recover if there is a problem
during the installation process. Please see Chapter 8 for details.

STARTING THE INSTALLER

After you have downloaded the file 'NVIDIA-Linux-x86-96.43.23-pkg#.run',
change to the directory containing the downloaded file, and as the 'root' user
run the executable:

    # cd yourdirectory
    # sh NVIDIA-Linux-x86-96.43.23-pkg#.run

The '.run' file is a self-extracting archive. When executed, it extracts the
contents of the archive and runs the contained 'nvidia-installer' utility,
which provides an interactive interface to walk you through the installation.

 'nvidia-installer' will also install itself to '/usr/bin/nvidia-installer',
which may be used at some later time to uninstall drivers, auto-download
updated drivers, etc. The use of this utility is detailed later in this
chapter.

You may also supply command line options to the '.run' file. Some of the more
common options are listed below.

Common '.run' Options

--info

    Print embedded info about the '.run' file and exit.

--check

    Check integrity of the archive and exit.

--extract-only

    Extract the contents of './NVIDIA-Linux-x86-96.43.23.run', but do not run
    'nvidia-installer'.

--help

    Print usage information for the common commandline options and exit.

--advanced-options

    Print usage information for common command line options as well as the
    advanced options, and then exit.


INSTALLING THE KERNEL INTERFACE

The NVIDIA kernel module has a kernel interface layer that must be compiled
specifically for each kernel. NVIDIA distributes the source code to this
kernel interface layer, as well as precompiled versions for many of the
kernels provided by popular Linux distributions.

When the installer is run, it will determine if it has a precompiled kernel
interface for the kernel you are running. If it does not have one, the
installer will check your system for the required kernel sources and compile
the interface for you. You must have the source code for your kernel installed
for compilation to work. On most systems, this means that you will need to
locate and install the correct kernel-source, kernel-headers, or kernel-devel
package; on some distributions, no additional packages are required (e.g.
Fedora Core 3, Red Hat Enterprise Linux 4).

Note that linking of the kernel interface requires you to have a linker
installed on your system. The linker, usually '/usr/bin/ld', is part of the
binutils package. If a precompiled kernel interface is not found, you must
install a linker prior to installing the NVIDIA driver.

FEATURES OF THE INSTALLER

Without options, the '.run' file executes the installer after unpacking it.
The installer can be run as a separate step in the process, or can be run at a
later time to get updates, etc. Some of the more important commandline options
of 'nvidia-installer' are:

'nvidia-installer' options

--uninstall

    During installation, the installer will make backups of any conflicting
    files and record the installation of new files. The uninstall option
    undoes an install, restoring the system to its pre-install state.

--latest

    Connect to NVIDIA's FTP site, and report the latest driver version and the
    url to the latest driver file.

--update

    Connect to NVIDIA's FTP site, download the most recent driver file, and
    install it.

--ui=none

    The installer uses an ncurses-based user interface if it is able to locate
    the correct ncurses library. Otherwise, it will fall back to a simple
    commandline user interface. This option disables the use of the ncurses
    library.


______________________________________________________________________________

Chapter 3. Configuring X for the NVIDIA Driver
______________________________________________________________________________

The X configuration file provides a means to configure the X server. This
section describes the settings necessary to enable the NVIDIA driver. A
comprehensive list of parameters is provided in Appendix D.

The NVIDIA Driver includes a utility called nvidia-xconfig, which is designed
to make editing the X configuration file easy. You can also edit it by hand.

USING NVIDIA-XCONFIG TO CONFIGURE THE X SERVER

nvidia-xconfig will find the X configuration file and modify it to use the
NVIDIA X driver. In most cases, you can simply answer "Yes" when the installer
asks if it should run it. If you need to reconfigure your X server later, you
can run nvidia-xconfig again from a terminal. nvidia-xconfig will make a
backup copy of your configuration file before modifying it.

Note that the X server must be restarted for any changes to its configuration
file to take effect.

More information about nvidia-xconfig can be found in the nvidia-xconfig
manual page by running.

    % man nvidia-xconfig



EDITING THE CONFIGURATION FILE BY HAND

In April 2004 the X.Org Foundation released an X server based on the XFree86
server. While your release may use the X.Org X server, rather than XFree86,
the differences between the two should have no impact on NVIDIA Linux users
with two exceptions:

   o The X.Org configuration file is '/etc/X11/xorg.conf' while the XFree86
     configuration file is '/etc/X11/XF86Config'. The files use the same
     syntax. This document refers to both files as "the X config file".

   o The X.Org log file is '/var/log/Xorg.#.log' while the XFree86 log file is
     '/var/log/XFree86.#.log' (where '#' is the server number -- usually 0).
     The format of the log files is nearly identical. This document refers to
     both files as "the X log file".

In order for any changes to be read into the X server, you must edit the file
used by the server. While it is not unreasonable to simply edit both files, it
is easy to determine the correct file by searching for the line

    (==) Using config file:

in the X log file. This line indicates the name of the X config file in use.

If you do not have a working X config file, there are a few different ways to
obtain one. A sample config file is included both with the XFree86
distribution and with the NVIDIA driver package (at
'/usr/share/doc/NVIDIA_GLX-1.0/'). Tools for generating a config file (such as
'xf86config') are generally included with Linux. Additional information on the
X config syntax can be found in the XF86Config manual page (`man XF86Config`
or `man xorg.conf`).

If you have a working X config file for a different driver (such as the "nv"
or "vesa" driver), then simply edit the file as follows.

Remove the line:

      Driver "nv"
  (or Driver "vesa")
  (or Driver "fbdev")

and replace it with the line:

    Driver "nvidia"

Remove the following lines:

    Load "dri"
    Load "GLCore"

In the "Module" section of the file, add the line (if it does not already
exist):

    Load "glx"

If the X config file does not have a "Module" section, you can safely skip the
last step if the X server installed on your system is an X.Org X server or an
XFree86 X release version 4.4.0 or greater. If you are using an older XFree86
X server, add the following to your X config file:

Section "Module"
    Load "extmod"
    Load "dbe"
    Load "type1"
    Load "freetype"
    Load "glx"
EndSection

There are numerous options that may be added to the X config file to tune the
NVIDIA X driver. Please see Appendix D for a complete list of these options.

Once you have completed these edits to the X config file, you may restart X
and begin using the accelerated OpenGL libraries. After restarting X, any
OpenGL application should automatically use the new NVIDIA libraries. If you
encounter any problems, please see Chapter 5 for common problem diagnoses.

______________________________________________________________________________

Chapter 4. Frequently Asked Questions
______________________________________________________________________________

This section provides answers to frequently asked questions associated with
the NVIDIA Linux x86 Driver and its installation. Common problem diagnoses can
be found in Chapter 5 and tips for new users can be found in Chapter 8. Also,
detailed information for specific setups is provided in the Appendices.


NVIDIA-INSTALLER

Q. How do I extract the contents of the '.run' without actually installing the
   driver?

A. Run the installer as follows:
   
       # sh NVIDIA-Linux-x86-96.43.23-pkg1.run --extract-only
   
   This will create the directory NVIDIA-Linux-x86-96.43.23-pkg1, containing
   the uncompressed contents of the '.run' file.


Q. How can I see the source code to the kernel interface layer?

A. The source files to the kernel interface layer are in the usr/src/nv
   directory of the extracted .run file. To get to these sources, run:
   
       # sh NVIDIA-Linux-x86-1.0-6629-pkg1.run --extract-only
       # cd NVIDIA-Linux-x86-1.0-6629-pkg1/usr/src/nv/
   
   

Q. How and when are the the NVIDIA device files created?

A. Depending on the target system's configuration, the NVIDIA device files
   used to be created in one of three different ways:
   
      o at installation time, using mknod
   
      o at module load time, via devfs (Linux device file system)
   
      o at module load time, via hotplug/udev
   
   With current NVIDIA driver releases, device files are created or modified
   by the X driver when the X server is started.

   By default, the NVIDIA driver will attempt to create device files with the
   following attributes:
   
         UID:  0     - 'root'
         GID:  0     - 'root'
         Mode: 0666  - 'rw-rw-rw-'
   
   Existing device files are changed if their attributes don't match these
   defaults. If you want the NVIDIA driver to create the device files with
   different attributes, you can specify them with the "NVreg_DeviceFileUID"
   (user), "NVreg_DeviceFileGID" (group) and "NVreg_DeviceFileMode" NVIDIA
   Linux kernel module parameters.

   For example, the NVIDIA driver can be instructed to create device files
   with UID=0 (root), GID=44 (video) and Mode=0660 by passing the following
   module parameters to the NVIDIA Linux kernel module:
   
         NVreg_DeviceFileUID=0 
         NVreg_DeviceFileGID=44 
         NVreg_DeviceFileMode=0660
   
   The "NVreg_ModifyDeviceFiles" NVIDIA kernel module parameter will disable
   dynamic device file management, if set to 0.


Q. I just upgraded my kernel, and now the NVIDIA kernel module will not load.
   What is wrong?

A. The kernel interface layer of the NVIDIA kernel module must be compiled
   specifically for the configuration and version of your kernel. If you
   upgrade your kernel, then the simplest solution is to reinstall the driver.

   ADVANCED: You can install the NVIDIA kernel module for a non running kernel
   (for example: in the situation where you just built and installed a new
   kernel, but have not rebooted yet) with a command line such as this:
   
       # sh NVIDIA-Linux-x86-96.43.23-pkg1.run --kernel-name='KERNEL_NAME'
   
   
   Where 'KERNEL_NAME' is what 'uname -r' would report if the target kernel
   were running.


Q. Why does NVIDIA not provide RPMs anymore?

A. Not every Linux distribution uses RPM, and NVIDIA wanted a single solution
   that would work across all Linux distributions. As indicated in the NVIDIA
   Software License, Linux distributions are welcome to repackage and
   redistribute the NVIDIA Linux driver in whatever package format they wish.


Q. Can the nvidia-installer use a proxy server?

A. Yes, because the FTP support in nvidia-installer is based on snarf, it will
   honor the 'FTP_PROXY', 'SNARF_PROXY', and 'PROXY' environment variables.


Q. What is the significance of the 'pkg#' suffix on the '.run' file?

A. The 'pkg#' suffix is used to distinguish between '.run' files containing
   the same driver, but different sets of precompiled kernel interfaces. If a
   distribution releases a new kernel after an NVIDIA driver is released, the
   current NVIDIA driver can be repackaged to include a precompiled kernel
   interface for that newer kernel (in addition to all the precompiled kernel
   interfaces that were included in the previous package of the driver).

    '.run' files with the same version number, but different pkg numbers, only
   differ in what precompiled kernel interfaces are included. Additionally,
   '.run' files with higher pkg numbers will contain everything the '.run'
   files with lower pkg numbers contain.


Q. I have already installed NVIDIA-Linux-x86-96.43.23-pkg1.run, but I see that
   NVIDIA-Linux-x86-96.43.23-pkg2.run was just posted on the NVIDIA Linux
   driver download page. Should I download and install
   NVIDIA-Linux-x86-96.43.23-pkg2.run?

A. This is not necessary. The driver contained within all 96.43.23 '.run'
   files will be identical. There is no need to reinstall.


Q. Can I add my own precompiled kernel interfaces to a '.run' file?

A. Yes, the --add-this-kernel  '.run' file option will unpack the '.run' file,
   build a precompiled kernel interface for the currently running kernel, and
   repackage the '.run' file, appending '-custom' to the filename. This may be
   useful, for example. if you administer multiple Linux machines, each
   running the same kernel.


Q. Where can I find the source code for the 'nvidia-installer' utility?

A. The 'nvidia-installer' utility is released under the GPL. The latest source
   code for it is available at:
   ftp://download.nvidia.com/XFree86/nvidia-installer



NVIDIA DRIVER

Q. Where should I start when diagnosing display problems?

A. One of the most useful tools for diagnosing problems is the X log file in
   '/var/log'. Lines that begin with "(II)" are information, "(WW)" are
   warnings, and "(EE)" are errors. You should make sure that the correct
   config file (i.e. the config file you are editing) is being used; look for
   the line that begins with:
   
       (==) Using config file:
   
   Also make sure that the NVIDIA driver is being used, rather than the "nv"
   or "vesa" driver. Search for
   
       (II) LoadModule: "nvidia"
   
   Lines from the driver should begin with:
   
       (II) NVIDIA(0)
   
   

Q. How can I increase the amount of data printed in the X log file?

A. By default, the NVIDIA X driver prints relatively few messages to stderr
   and the X log file. If you need to troubleshoot, then it may be helpful to
   enable more verbose output by using the X command line options -verbose and
   -logverbose, which can be used to set the verbosity level for the 'stderr'
   and log file messages, respectively. The NVIDIA X driver will output more
   messages when the verbosity level is at or above 5 (X defaults to verbosity
   level 1 for 'stderr' and level 3 for the log file). So, to enable verbose
   messaging from the NVIDIA X driver to both the log file and 'stderr', you
   could start X with the verbosity level set to 5, by doing the following
   
       % startx -- -verbose 5 -logverbose 5
   
   

Q. Where can I get 'gl.h' or 'glx.h' so I can compile OpenGL programs?

A. Most systems come with these header files preinstalled. However, NVIDIA
   provides its own 'gl.h' and 'glx.h' files, which get installed by default
   as part of driver installation. If you prefer that the NVIDIA-distributed
   OpenGL header files not be installed, you can pass the --no-opengl-headers
   option to the 'NVIDIA-Linux-x86-96.43.23-pkg1.run' file during
   installation.


Q. Can I receive email notification of new NVIDIA Accelerated Linux Driver Set
   releases?

A. Yes. Fill out the form at: http://www.nvidia.com/view.asp?FO=driver_update


Q. What is NVIDIA's policy towards development series Linux kernels?

A. NVIDIA does not officially support development series kernels. However, all
   the kernel module source code that interfaces with the Linux kernel is
   available in the 'usr/src/nv/' directory of the '.run' file. NVIDIA
   encourages members of the Linux community to develop patches to these
   source files to support development series kernels. A web search will most
   likely yield several community supported patches.


Q. Why does X use so much memory?

A. When measuring any application's memory usage, you must be careful to
   distinguish between physical system RAM used and virtual mappings of shared
   resources. For example, most shared libraries exist only once in physical
   memory but are mapped into multiple processes. This memory should only be
   counted once when computing total memory usage. In the same way, the video
   memory on a graphics card or register memory on any device can be mapped
   into multiple processes. These mappings do not consume normal system RAM.

   This has been a frequently discussed topic on XFree86 mailing lists; see,
   for example:

    http://marc.theaimsgroup.com/?l=xfree-xpert&m=96835767116567&w=2

   The 'pmap' utility described in the above thread is available here:
   http://web.hexapodia.org/~adi/pmap.c and is a useful tool in distinguishing
   between types of memory mappings. For example, while 'top' may indicate
   that X is using several hundred MB of memory, the last line of output from
   pmap:
   
       mapped:   287020 KB writable/private: 9932 KB shared: 264656 KB
   
   reveals that X is really only using roughly 10MB of system RAM (the
   "writable/private" value).

   Note, also, that X must allocate resources on behalf of X clients (the
   window manager, your web browser, etc); X's memory usage will increase as
   more clients request resources such as pixmaps, and decrease as you close X
   applications.


Q. Where can I find the tarballs?

A. Plain tarballs are no longer available. The '.run' file is a tarball with a
   shell script prepended. You can execute the '.run' file with the
   --extract-only option to unpack the tarball.


Q. Where can I find older driver versions?

A. Please visit ftp://download.nvidia.com/XFree86_40/


Q. What is SELinux and how does it interact with the NVIDIA driver ?

A. Security-Enhanced Linux (SELinux) is a set of modifications applied to the
   Linux kernel and utilities that implement a security policy architecture.
   When in use it requires that the security type on all shared libraries be
   set to 'shlib_t'. The installer detects when to set the security type, and
   sets it on all shared libraries it installs. The option --force-selinux
   passed to the '.run' file overrides the detection of when to set the
   security type.


Q. Using GNOME configuration utilities, I am unable to get a resolution above
   800x600. What is wrong?

A. The installation of GNOME provided in distributions such as Red Hat
   Enterprise Linux 4 contain several competing interfaces for specifying
   resolution:
   
   
       'System Settings' -> 'Display'
   
   
   which will update the X configuration file, and
   
   
       'Applications' -> 'Preferences' -> 'Screen Resolution'
   
   
   which will update the per-user screen resolution using the XRandR
   extension. Your desktop resolution will be limited to the smaller of the
   two settings. Please be sure to check the setting of each.


Q. Why do applications that use DGA graphics fail?

A. The NVIDIA driver does not support the graphics component of the
   XFree86-DGA (Direct Graphics Access) extension. Applications can use the
   XDGASelectInput() function to acquire relative pointer motion, but
   graphics-related functions such as XDGASetMode() and XDGAOpenFramebuffer()
   will fail.

   The graphics component of XFree86-DGA is not supported because it requires
   a CPU mapping of framebuffer memory. As graphics boards ship with
   increasing quantities of video memory, the NVIDIA X driver has had to
   switch to a more dynamic memory mapping scheme that is incompatible with
   DGA. Furthermore, DGA does not cooperate with other graphics rendering
   libraries such as Xlib and OpenGL because it accesses GPU resources
   directly.

   It is recommended that applications use OpenGL or Xlib, rather than DGA,
   for graphics rendering. Using rendering libraries other than DGA will yield
   better performance and improve interoperability with other X applications.


Q. My kernel log contains messages that are prefixed with "Xid"; what do these
   messages mean?

A. "Xid" messages indicate that a general GPU error occurred, most often due
   to the driver misprogramming the GPU or to corruption of the commands sent
   to the GPU. These messages provide diagnostic information that can be used
   by NVIDIA to aid in debugging reported problems.


Q. On what NVIDIA hardware is the EXT_framebuffer_object OpenGL extension
   supported?

A. EXT_framebuffer_object is supported on GeForce FX, Quadro FX, and newer
   GPUs.


Q. I use the Coolbits overclocking interface to adjust my graphics card's
   clock frequencies, but the defaults are reset whenever X is restarted. How
   do I make my changes persistent?

A. Clock frequency settings are not saved/restored automatically by default to
   avoid potential stability and other problems that may be encountered if the
   chosen frequency settings differ from the defaults qualified by the
   manufacturer. You can use the command line below in '~/.xinitrc' to
   automatically apply custom clock frequency settings when the X server is
   started:
   
       # nvidia-settings -a GPUOverclockingState=1 -a
   GPU2DClockFreqs=<GPU>,<MEM> -a GPU3DClockFreqs=<GPU>,<MEM>
   
   Here '<GPU>' and '<MEM>' are the desired GPU and video memory frequencies
   (in MHz), respectively.


Q. Why is the refresh rate not reported correctly by utilities that use the
   XRandR X extension (e.g., the GNOME "Screen Resolution Preferences" panel,
   `xrandr -q`, etc)?

A. The XRandR X extension is not presently aware of multiple display devices
   on a single X screen; it only sees the MetaMode bounding box, which may
   contain one or more actual modes. This means that if multiple MetaModes
   have the same bounding box, XRandR will not be able to distinguish between
   them.

   In order to support DynamicTwinView, the NVIDIA X driver must make each
   MetaMode appear to be unique to XRandR. Presently, the NVIDIA X driver
   accomplishes this by using the refresh rate as a unique identifier.

   You can use `nvidia-settings -q RefreshRate` to query the actual refresh
   rate on each display device.

   This behavior can be disabled by setting the X configuration option
   "DynamicTwinView" to FALSE.

   For details, see Appendix G.


Q. Why does starting certain applications result in Xlib error messages
   indicating extensions like "XFree86-VidModeExtension" or "SHAPE" are
   missing?

A. If your X config file has a "Module" section that does not list the
   "extmod" module, some X server extensions may be missing, resulting in
   error messages of the form:
   
   Xlib: extension "SHAPE" missing on display ":0.0"
   Xlib: extension "XFree86-VidModeExtension" missing on display ":0.0"
   Xlib: extension "XFree86-DGA" missing on display ":0.0"
   
   You can solve this problem by adding the line below to your X config file's
   "Module" section:
   
       Load "extmod"
   
   

______________________________________________________________________________

Chapter 5. Common Problems
______________________________________________________________________________

This section provides solutions to common problems associated with the NVIDIA
Linux x86 Driver.

Q. My X server fails to start, and my X log file contains the error:
   
   (EE) NVIDIA(0): The NVIDIA kernel module does not appear to
   (EE) NVIDIA(0):      be receiving interrupts generated by the NVIDIA
   graphics
   (EE) NVIDIA(0):      device PCI:x:x:x. Please see the COMMON PROBLEMS
   (EE) NVIDIA(0):      section in the README for additional information.
   
   
A. This can be caused by a variety of problems, such as PCI IRQ routing
   errors, I/O APIC problems or conflicts with other devices sharing the IRQ
   (or their drivers).

   If possible, configure your system such that your graphics card does not
   share its IRQ with other devices (try moving the graphics card to another
   slot if applicable, unload/disable the driver(s) for the device(s) sharing
   the card's IRQ, or remove/disable the device(s)).

   Depending on the nature of the problem, one of (or a combination of) these
   kernel parameters might also help:
   
       Parameter         Behavior
       --------------    ---------------------------------------------------
       pci=noacpi        don't use ACPI for PCI IRQ routing
       pci=biosirq       use PCI BIOS calls to retrieve the IRQ routing
                         table
       noapic            don't use I/O APICs present in the system
       acpi=off          disable ACPI
   
   

Q. My X server fails to start, and my X log file contains the error:
   
   (EE) NVIDIA(0): The interrupt for NVIDIA graphics device PCI:x:x:x
   (EE) NVIDIA(0):      appears to be edge-triggered. Please see the COMMON
   (EE) NVIDIA(0):      PROBLEMS section in the README for additional
   information.
   
   
A. An edge-triggered interrupt means that the kernel has programmed the
   interrupt as edge-triggered rather than level-triggered in the Advanced
   Programmable Interrupt Controller (APIC). Edge-triggered interrupts are not
   intended to be used for sharing an interrupt line between multiple devices;
   level-triggered interrupts are the intended trigger for such usage. When
   using edge-triggered interrupts, it is common for device drivers using that
   interrupt line to stop receiving interrupts. This would appear to the end
   user as those devices no longer working, and potentially as a full system
   hang. These problems tend to be more common when multiple devices are
   sharing that interrupt line.

   This occurs when ACPI is not used to program interrupt routing in the APIC.
   This often occurs on 2.4 Linux kernels, which do not fully support ACPI, or
   2.6 kernels when ACPI is disabled or fails to initialize. In these cases,
   the Linux kernel falls back to tables provided by the system BIOS. In some
   cases the system BIOS assumes ACPI will be used for routing interrupts and
   configures these tables to incorrectly label all interrupts as
   edge-triggered. The current interrupt configuration can be found in
   /proc/interrupts.

   Available workarounds include: updating to a newer system BIOS, trying a
   2.6 kernel with ACPI enabled, or passing the 'noapic' option to the kernel
   to force interrupt routing through the traditional Programmable Interrupt
   Controller (PIC). Newer kernels also provide an interrupt polling mechanism
   to attempt to work around this problem. This mechanism can be enabled by
   passing the 'irqpoll' option to the kernel.

   Currently, the NVIDIA driver will attempt to detect edge triggered
   interrupts and X will purposely fail to start (to avoid stability issues).
   This behavior can be overridden by setting the "NVreg_RMEdgeIntrCheck"
   NVIDIA Linux kernel module parameter. This parameter defaults to "1", which
   enables the edge triggered interrupt detection. Set this parameter to "0"
   to disable this detection.


Q. X starts for me, but OpenGL applications terminate immediately.

A. If X starts but you have trouble with OpenGL, you most likely have a
   problem with other libraries in the way, or there are stale symlinks. See
   Appendix C for details. Sometimes, all it takes is to rerun 'ldconfig'.

   You should also check that the correct extensions are present;
   
       % xdpyinfo
   
   should show the "GLX" and "NV-GLX" extensions present. If these two
   extensions are not present, then there is most likely a problem loading the
   glx module, or it is unable to implicitly load GLcore. Check your X config
   file and make sure that you are loading glx (see Chapter 3). If your X
   config file is correct, then check the X log file for warnings/errors
   pertaining to GLX. Also check that all of the necessary symlinks are in
   place (refer to Appendix C).


Q. When Xinerama is enabled, my stereo glasses are shuttering only when the
   stereo application is displayed on one specific X screen. When the
   application is displayed on the other X screens, the stereo glasses stop
   shuttering.

A. This problem occurs with DDC and "blue line" stereo glasses, that get the
   stereo signal from one video port of the graphics card. When a X screen
   does not display any stereo drawable the stereo signal is disabled on the
   associated video port.

   Forcing stereo flipping allows the stereo glasses to shutter continuously.
   This can be done by enabling the OpenGL control "Force Stereo Flipping" in
   nvidia-settings, or by setting the X configuration option
   "ForceStereoFlipping" to "1".


Q. Stereo is not in sync across multiple displays.

A. There are two cases where this may occur. If the displays are attached to
   the same GPU, and one of them is out of sync with the stereo glasses, you
   will need to reconfigure your monitors to drive identical mode timings;
   please see Appendix J for details.

   If the displays are attached to different GPUs, the only way to synchronize
   stereo across the displays is with a G-Sync device, which is only supported
   by certain Quadro cards. Please see Appendix X for details. This applies to
   seperate GPUs on seperate cards as well as seperate GPUs on the same card,
   such as Quadro FX 4500 X2. Note that the Quadro FX 4500 X2 only provides a
   single DIN connector for stereo, tied to the bottommost GPU. In order to
   synchronize onboard stereo on the other GPU you must use a G-Sync device.


Q. My X server fails to start, and my X log file contains the error:
   
   (EE) NVIDIA(0): Failed to load the NVIDIA kernel module!
   
   
A. The X driver will abort with this error message if the NVIDIA kernel module
   fails to load. If you receive this error, you should check the output of
   `dmesg` for kernel error messages and/or attempt to load the kernel module
   explicitly with `modprobe nvidia`. If unresolved symbols are reported, then
   the kernel module was most likely built against a Linux kernel source tree
   (or kernel headers) for a kernel revision or configuration that doesn't
   match the running kernel.

   You can specify the location of the kernel source tree (or headers) when
   you install the NVIDIA driver using the --kernel-source-path command line
   option (see `sh NVIDIA-Linux-x86-96.43.23-pkg1.run --advanced-options` for
   details).

   Old versions of the module-init-tools include `modprobe` binaries that
   report an error when instructed to load a module that's already loaded into
   the kernel. Please upgrade your module-init-tools if you receive an error
   message to this effect.

   The X server reads '/proc/sys/kernel/modprobe' to determine the path to the
   `modprobe` utility and falls back to '/sbin/modprobe' if the file doesn't
   exist. Please make sure that this path is valid and refers to a `modprobe`
   binary compatible with the Linux kernel running on your system.

   The "LoadKernelModule" X driver option can be used to change the default
   behavior and disable kernel module auto-loading.


Q. Installing the NVIDIA kernel module gives an error message like:
   
   #error Modules should never use kernel-headers system headers
   #error but headers from an appropriate kernel-source
   
   
A. You need to install the source for the Linux kernel. In most situations you
   can fix this problem by installing the kernel-source or kernel-devel
   package for your distribution


Q. OpenGL applications crash and print out the following warning:
   
   WARNING: Your system is running with a buggy dynamic loader.
   This may cause crashes in certain applications.  If you
   experience crashes you can try setting the environment
   variable __GL_SINGLE_THREADED to 1.  For more information
   please consult the FREQUENTLY ASKED QUESTIONS section in
   the file /usr/share/doc/NVIDIA_GLX-1.0/README.txt.
   
   
A. The dynamic loader on your system has a bug which will cause applications
   linked with pthreads, and that dlopen() libGL multiple times, to crash.
   This bug is present in older versions of the dynamic loader. Distributions
   that shipped with this loader include but are not limited to Red Hat Linux
   6.2 and Mandrake Linux 7.1. Version 2.2 and later of the dynamic loader are
   known to work properly. If the crashing application is single threaded then
   setting the environment variable '__GL_SINGLE_THREADED' to "1" will prevent
   the crash. In the bash shell you would enter:
   
       % export __GL_SINGLE_THREADED=1
   
   and in csh and derivatives use:
   
       % setenv __GL_SINGLE_THREADED 1
   
   Previous releases of the NVIDIA Accelerated Linux Driver Set attempted to
   work around this problem. Unfortunately, the workaround caused problems
   with other applications and was removed after version 1.0-1541.


Q. Quake3 crashes when changing video modes.

A. You are probably experiencing a problem described above. Please check the
   text output for the "WARNING" message described in the previous hint.
   Setting '__GL_SINGLE_THREADED' to "1" as will fix the problem.


Q. I cannot build the NVIDIA kernel module, or, I can build the NVIDIA kernel
   module, but modprobe/insmod fails to load the module into my kernel. What
   is wrong?

A. These problems are generally caused by the build using the wrong kernel
   header files (i.e. header files for a different kernel version than the one
   you are running). The convention used to be that kernel header files should
   be stored in '/usr/include/linux/', but that is deprecated in favor of
   '/lib/modules/RELEASE/build/include' (where RELEASE is the result of 'uname
   -r'. The 'nvidia-installer' should be able to determine the location on
   your system; however, if you encounter a problem you can force the build to
   use certain header files by using the --kernel-include-dir option. For this
   to work you will of course need the appropriate kernel header files
   installed on your system. Consult the documentation that came with your
   distribution; some distributions do not install the kernel header files by
   default, or they install headers that do not coincide properly with the
   kernel you are running.


Q. There are problems running Heretic II.

A. Heretic II installs, by default, a symlink called 'libGL.so' in the
   application directory. You can remove or rename this symlink, since the
   system will then find the default 'libGL.so' (which our drivers install in
   '/usr/lib'). From within Heretic II you can then set your render mode to
   OpenGL in the video menu. There is also a patch available to Heretic II
   from lokigames at: http://www.lokigames.com/products/heretic2/updates.php3/


Q. My system hangs when switching to a virtual terminal if I have rivafb
   enabled.

A. Using both rivafb and the NVIDIA kernel module at the same time is
   currently broken. In general, using two independent software drivers to
   drive the same piece of hardware is a bad idea.


Q. Compiling the NVIDIA kernel module gives this error:
   
   You appear to be compiling the NVIDIA kernel module with
   a compiler different from the one that was used to compile
   the running kernel. This may be perfectly fine, but there
   are cases where this can lead to unexpected behavior and
   system crashes.
   
   If you know what you are doing and want to override this
   check, you can do so by setting IGNORE_CC_MISMATCH.
   
   In any other case, set the CC environment variable to the
   name of the compiler that was used to compile the kernel.
   
   
A. You should compile the NVIDIA kernel module with the same compiler version
   that was used to compile your kernel. Some Linux kernel data structures are
   dependent on the version of gcc used to compile it; for example, in
   'include/linux/spinlock.h':
   
           ...
           * Most gcc versions have a nasty bug with empty initializers.
           */
           #if (__GNUC__ > 2)
             typedef struct { } rwlock_t;
             #define RW_LOCK_UNLOCKED (rwlock_t) { }
           #else
             typedef struct { int gcc_is_buggy; } rwlock_t;
             #define RW_LOCK_UNLOCKED (rwlock_t) { 0 }
           #endif
   
   If the kernel is compiled with gcc 2.x, but gcc 3.x is used when the kernel
   interface is compiled (or vice versa), the size of rwlock_t will vary, and
   things like ioremap will fail. To check what version of gcc was used to
   compile your kernel, you can examine the output of:
   
       % cat /proc/version
   
   To check what version of gcc is currently in your '$PATH', you can examine
   the output of:
   
       % gcc -v
   
   

Q. X fails with error
   
   Failed to allocate LUT context DMA
   
   
A. This is one of the possible consequences of compiling the NVIDIA kernel
   interface with a different gcc version than used to compile the Linux
   kernel (see above).


Q. I recently updated various libraries on my system using my Linux
   distributor's update utility, and the NVIDIA graphics driver no longer
   works.

A. Conflicting libraries may have been installed by your distribution's update
   utility; please see Appendix C for details on how to diagnose this.


Q. I have rebuilt the NVIDIA kernel module, but when I try to insert it, I get
   a message telling me I have unresolved symbols.

A. Unresolved symbols are most often caused by a mismatch between your kernel
   sources and your running kernel. They must match for the NVIDIA kernel
   module to build correctly. Please make sure your kernel sources are
   installed and configured to match your running kernel.


Q. How do I tell if I have my kernel sources installed?

A. If you are running on a distro that uses RPM (Red Hat, Mandrake, SuSE,
   etc), then you can use 'rpm' to tell you. At a shell prompt, type:
   
       % rpm -qa | grep kernel
   
   and look at the output. You should see a package that corresponds to your
   kernel (often named something like kernel-2.6.15-7) and a kernel source
   package with the same version (often named something like
   kernel-devel-2.6.15-7 or kernel-source-2.4.18-3). If none of the lines seem
   to correspond to a source package, then you will probably need to install
   it. If the versions listed mismatch (e.g., kernel-2.6.15-7 vs.
   kernel-devel-2.6.15-10), then you will need to update the kernel-devel
   package to match the installed kernel. If you have multiple kernels
   installed, you need to install the kernel-devel package that corresponds to
   your RUNNING kernel (or make sure your installed source package matches the
   running kernel). You can do this by looking at the output of 'uname -r' and
   matching versions.


Q. I am unable to load the NVIDIA kernel module that I compiled for the Red
   Hat Linux 7.3 2.4.18-3bigmem kernel.

A. The kernel header files Red Hat Linux distributes for Red Hat Linux 7.3
   2.4.18-3bigmem kernel are misconfigured. NVIDIA's precompiled kernel module
   for this kernel can be loaded, but if you want to compile the NVIDIA kernel
   interface files yourself for this kernel, then you will need to perform the
   following:
   
       # cd /lib/modules/`uname -r`/build/
       # make mrproper
       # cp configs/kernel-2.4.18-i686-bigmem.config .config
       # make oldconfig dep
   
   Note: Red Hat Linux ships kernel header files that are simultaneously
   configured for ALL of their kernels for a particular distribution version.
   A header file generated at boot time sets up a few parameters that select
   the correct configuration. Rebuilding the kernel headers with the above
   commands will create header files suitable for the Red Hat Linux 7.3
   2.4.18-3bigmem kernel configuration only, thus making the header files for
   the other configurations unusable.


Q. OpenGL applications leak significant amounts of memory on my system!

A. If your kernel is making use of the -rmap VM, the system may be leaking
   memory due to a memory management optimization introduced in -rmap14a. The
   -rmap VM has been adopted by several popular distributions, the memory leak
   is known to be present in some of the distribution kernels; it has been
   fixed in -rmap15e.

   If you suspect that your system is affected, please try upgrading your
   kernel or contact your distribution's vendor for assistance.


Q. Some OpenGL applications (like Quake3 Arena) crash when I start them on Red
   Hat Linux 9.0.

A. Some versions of the glibc package shipped by Red Hat that support TLS do
   not properly handle using dlopen() to access shared libraries which use
   some TLS models. This problem is exhibited, for example, when Quake3 Area
   dlopen() 's NVIDIA's libGL library. Please obtain at least glibc-2.3.2-11.9
   which is available as an update from Red Hat.


Q. I have installed the driver, but my Enable 3D Acceleration checkbox is
   still grayed out.

A. Most distribution-provided configuration applets are not aware of the
   NVIDIA accelerated driver, and consequently will not update themselves when
   you install the driver. Your driver, if it has been installed properly,
   should function fine.


Q. X does not restore the VGA console when run on a TV. I get this error
   message in my X log file:
   
   Unable to initialize the X int10 module; the console may not be
   restored correctly on your TV.
   
   
A. The NVIDIA X driver uses the X Int10 module to save and restore console
   state on TV out, and will not be able to restore the console correctly if
   it cannot use the Int10 module. If you have built the X server yourself,
   please be sure you have built the Int10 module. If you are using a build of
   the X server provided by a Linux distribution, and are missing the Int10
   module, please contact your distributor.


Q. When changing settings in games like Quake 3 Arena, or Wolfenstein Enemy
   Territory, the game crashes and I see this error:
   
   ...loading libGL.so.1: QGL_Init: dlopen libGL.so.1 failed: 
   /usr/lib/tls/libGL.so.1: shared object cannot be dlopen()ed:
   static TLS memory too small
   
   
A. These games close and reopen the NVIDIA OpenGL driver (via dlopen() /
   dlclose()) when settings are changed. On some versions of glibc (such as
   the one shipped with Red Hat Linux 9), there is a bug that leaks static TLS
   entries. This glibc bug causes subsequent re-loadings of the OpenGL driver
   to fail. This is fixed in more recent versions of glibc; please see Red Hat
   bug #89692: https://bugzilla.redhat.com/bugzilla/show_bug.cgi?id=89692


Q. X crashes during 'startx', and my X log file contains this error message:
   
   (EE) NVIDIA(0): Failed to obtain a shared memory identifier.
   
   
A. The NVIDIA OpenGL driver and the NVIDIA X driver require shared memory to
   communicate; you must have 'CONFIG_SYSVIPC' enabled in your kernel.


Q. When I try to install the driver, the installer claims that X is running,
   even though I have exited X.

A. The installer detects the presence of an X server by checking for X's lock
   files: '/tmp/.Xn-lock', where 'n' is the number of the X Display (the
   installer checks for X Displays 0-7). If you have exited X, but one of
   these files has been left behind, then you will need to manually delete the
   lock file. DO NOT remove this file if X is still running!


Q. My system runs, but seems unstable. What is wrong?

A. Your stability problems may be AGP-related. See Appendix F for details.


Q. OpenGL applications are running slowly

A. The application is probably using a different library still on your system,
   rather than the NVIDIA supplied OpenGL library. Please see Appendix C for
   details.


Q. There are problems running Quake2.

A. Quake2 requires some minor setup to get it going. First, in the Quake2
   directory, the install creates a symlink called 'libGL.so' that points at
   'libMesaGL.so'. This symlink should be removed or renamed. Second, in order
   to run Quake2 in OpenGL mode, you must type
   
       % quake2 +set vid_ref glx +set gl_driver libGL.so
   
   Quake2 does not seem to support any kind of full-screen mode, but you can
   run your X server at the same resolution as Quake2 to emulate full-screen
   mode.


Q. I am using either nForce of nForce2 internal graphics, and I see warnings
   like this in my X log file:
   
   Not using mode "1600x1200" (exceeds valid memory bandwidth usage)
   
   
A. Integrated graphics have more strict memory bandwidth limitations that
   limit the resolution and refresh rate of the modes you request. To work
   around this, you can reduce the maximum refresh rate by lowering the upper
   value of the VertRefresh range in the 'Monitor' section of your X config
   file. Though not recommended, you can disable the memory bandwidth test
   with the NoBandWidthTest X config file option.


Q. X takes a long time to start (possibly several minutes).

A. Most of the X startup delay problems we have found are caused by incorrect
   data in video BIOSes about what display devices are possibly connected or
   what i2c port should be used for detection. You can work around these
   problems with the X config option IgnoreDisplayDevices (please see the
   description in Appendix D).


Q. Fonts are incorrectly sized after installing the NVIDIA driver.

A. Incorrectly sized fonts are generally caused by incorrect DPI (Dots Per
   Inch) information. You can check what X thinks the physical size of your
   monitor is, by running:
   
    % xdpyinfo | grep dimensions
   
   This will report the size in pixels, and in millimeters.

   If these numbers are wrong, you can correct them by modifying the X
   server's DPI setting. See Appendix Y for details.


Q. General problems with ALi chipsets

A. There are some known timing and signal integrity issues on ALi chipsets.
   The following tips may help stabilize problematic ALI systems:
   
      o Disable TURBO AGP MODE in the BIOS.
   
      o When using a P5A upgrade to BIOS Revision 1002 BETA 2.
   
      o When using 1007, 1007A or 1009 adjust the IO Recovery Time to 4
        cycles.
   
      o AGP is disabled by default on some ALi chipsets (ALi1541, ALi1647) to
        work around severe system stability problems with these chipsets. See
        the comments for NVreg_EnableALiAGP in 'os-registry.c' to force AGP
        on anyway.
   
   

______________________________________________________________________________

Chapter 6. NVIDIA Contact Info
______________________________________________________________________________

There is an NVIDIA Linux Driver web forum. You can access it by going to
http://www.nvnews.net and following the "Forum" and "Linux Discussion Area"
links. This is the preferable tool for seeking help; users can post questions,
answer other users' questions, and search the archives of previous postings.

If all else fails, you can contact NVIDIA for support at:
linux-bugs@nvidia.com. But please, only send email to this address after you
have explored the Chapter 4 and Chapter 5 chapters of this document, and asked
for help on the nvnews.net web forum. When emailing linux-bugs@nvidia.com,
please include the 'nvidia-bug-report.log.gz' file generated by the
'nvidia-bug-report.sh' script (which is installed as part of driver
installation).

______________________________________________________________________________

Chapter 7. Additional Resources
______________________________________________________________________________



Resources

Linux OpenGL ABI

     http://oss.sgi.com/projects/ogl-sample/ABI/

The XFree86 Project

     http://www.xfree86.org/

XFree86 Video Timings HOWTO

     http://www.tldp.org/HOWTO/XFree86-Video-Timings-HOWTO/index.html

The X.Org Foundation

     http://www.x.org/

OpenGL

     http://www.opengl.org/


______________________________________________________________________________

Chapter 8. Tips for New Linux Users
______________________________________________________________________________

This installation guide assumes that the user has at least a basic
understanding of Linux techniques and terminology. In this section we provide
tips that the new user may find helpful. While the these tips are meant to
clarify and assist users in installing and configuring the NVIDIA Linux
Driver, it is by no means a tutorial on the use or administration of the Linux
operating system. Unlike many desktop operating systems, it is relatively easy
to cause irreparable damage to your Linux system. If you are unfamiliar with
the use of Linux, we strongly recommend that you seek a tutorial through your
distributor before proceeding.

THE COMMAND PROMPT

While newer releases of Linux bring new desktop interfaces to the user, much
of the work in Linux takes place at the command prompt. If you are familiar
with the Windows operating system, the Linux command prompt is analogous to
the Windows[1] command prompt, although the syntax and use varies somewhat.
All of the commands in this section are performed at the command prompt. Some
systems are configured to boot into console mode, in which case the user is
presented with a prompt at login. Other systems are configured to start the X
window system, in which case the user must open a terminal or console window
in order to get a command prompt. This can usually be done by searching the
desktop menus for a terminal or console program. While it is customizable, the
basic prompt usually consists of a short string of information, one of the
characters '#', '$', or '%', and a cursor (possibly flashing) that indicates
where the user's input will be displayed.

NAVIGATING THE DIRECTORY STRUCTURE

Linux has a hierarchical directory structure. From anywhere in the directory
structure, the 'ls' command will list the contents of that directory. The
'file' command will print the type of files in a directory. For example,

    % file filename

will print the type of the file 'filename'. Changing directories is done with
the 'cd' command.

    % cd dirname

will change the current directory to 'dirname'. From anywhere in the directory
structure, the command 'pwd' will print the name of the current directory.
There are two special directories, '.' and '..', which refer to the current
directory and the next directory up the hierarchy, respectively. For any
commands that require a file name or directory name as an argument, you may
specify the absolute or the relative paths to those elements. An absolute path
begins with the "/" character, referring to the top or root of the directory
structure. A relative path begins with a directory in the current working
directory. The relative path may begin with '.' or '..'. Elements of a path
are separated with the "/" character. As an example, if the current directory
is '/home/jesse' and the user wants to change to the '/usr/local' directory,
he can use either of the following commands to do so:

    % cd /usr/local

or

    % cd ../../usr/local


FILE PERMISSIONS AND OWNERSHIP

All files and directories have permissions and ownership associated with them.
This is useful for preventing non-administrative users from accidentally (or
maliciously) corrupting the system. The permissions and ownership for a file
or directory can be determined by passing the -l option to the 'ls' command.
For example:

% ls -l
drwxr-xr-x     2    jesse    users    4096    Feb     8 09:32 bin
drwxrwxrwx    10    jesse    users    4096    Feb    10 12:04 pub
-rw-r--r--     1    jesse    users      45    Feb     4 03:55 testfile
-rwx------     1    jesse    users      93    Feb     5 06:20 myprogram
-rw-rw-rw-     1    jesse    users     112    Feb     5 06:20 README
% 

The first character column in the first output field states the file type,
where 'd' is a directory and '-' is a regular file. The next nine columns
specify the permissions (see paragraph below) of the element. The second field
indicates the number of files associated with the element, the third field
indicates the owner, the fourth field indicates the group that the file is
associated with, the fifth field indicates the size of the element in bytes,
the sixth, seventh and eighth fields indicate the time at which the file was
last modified and the ninth field is the name of the element.

As stated, the last nine columns in the first field indicate the permissions
of the element. These columns are grouped into threes, the first grouping
indicating the permissions for the owner of the element ('jesse' in this
case), the second grouping indicating the permissions for the group associated
with the element, and the third grouping indicating the permissions associated
with the rest of the world. The 'r', 'w', and 'x' indicate read, write and
execute permissions, respectively, for each of these associations. For
example, user 'jesse' has read and write permissions for 'testfile', users in
the group 'users' have read permission only, and the rest of the world also
has read permissions only. However, for the file 'myprogram', user 'jesse' has
read, write and execute permissions (suggesting that 'myprogram' is a program
that can be executed), while the group 'users' and the rest of the world have
no permissions (suggesting that the owner doesn't want anyone else to run his
program). The permissions, ownership and group associated with an element can
be changed with the commands 'chmod', 'chown' and 'chgrp', respectively. If a
user with the appropriate permissions wanted to change the user/group
ownership of 'README' from jesse/users to joe/admin, he would do the
following:

    # chown joe README
    # chgrp admin README

The syntax for chmod is slightly more complicated and has several variations.
The most concise way of setting the permissions for a single element uses a
triplet of numbers, one for each of user, group and world. The value for each
number in the triplet corresponds to a combination of read, write and execute
permissions. Execute only is represented as 1, write only is represented as 2,
and read only is represented as 4. Combinations of these permissions are
represented as sums of the individual permissions. Read and execute is
represented as 5, where as read, write and execute is represented as 7. No
permissions is represented as 0. Thus, to give the owner read, write and
execute permissions, the group read and execute permissions and the world no
permissions, a user would do as follows:

    % chmod 750 myprogram


THE SHELL

The shell provides an interface between the user and the operating system. It
is the job of the shell to interpret the input that the user gives at the
command prompt and call upon the system to do something in response. There are
several different shells available, each with somewhat different syntax and
capabilities. The two most common flavors of shells used on Linux stem from
the Bourne shell ('sh') and the C-shell ('csh') Different users have
preferences and biases towards one shell or the other, and some certainly make
it easier (or at least more intuitive) to do some things than others. You can
determine your current shell by printing the value of the 'SHELL' environment
variable from the command prompt with

    % echo $SHELL

You can start a new shell simply by entering the name of the shell from the
command prompt:

    % csh

or

    % sh

and you can run a program from within a specific shell by preceding the name
of the executable with the name of the shell in which it will be run:

    % sh myprogram

The user's default shell at login is determined by whoever set up his account.
While there are many syntactic differences between shells, perhaps the one
that is encountered most frequently is the way in which environment variables
are set.

SETTING ENVIRONMENT VARIABLES

Every session has associated with it environment variables, which consist of
name/value pairs and control the way in which the shell and programs run from
the shell behave. An example of an environment variable is the 'PATH'
variable, which tells the shell which directories to search when trying to
locate an executable file that the user has entered at the command line. If
you are certain that a command exists, but the shell complains that it cannot
be found when you try to execute it, there is likely a problem with the 'PATH'
variable. Environment variables are set differently depending on the shell
being used. For the Bourne shell ('sh'), it is done as:

    % export MYVARIABLE="avalue"

for the C-shell, it is done as:

    % setenv MYVARIABLE "avalue"

In both cases the quotation marks are only necessary if the value contains
spaces. The 'echo' command can be used to examine the value of an environment
variable:

    % echo $MYVARIABLE

Commands to set environment variables can also include references to other
environment variables (prepended with the "$" character), including
themselves. In order to add the path '/usr/local/bin' to the beginning of the
search path, and the current directory '.' to the end of the search path, a
user would enter

    % export PATH=/usr/local/bin:$PATH:.

in the Bourne shell, and

    % setenv PATH /usr/local/bin:${PATH}:.

in C-shell. Note the curly braces are required to protect the variable name in
C-shell.

EDITING TEXT FILES

There are several text editors available for the Linux operating system. Some
of these editors require the X window system, while others are designed to
operate in a console or terminal. It is generally a good thing to be competent
with a terminal-based text editor, as there are times when the files necessary
for X to run are the ones that must be edited. Three popular editors are 'vi',
'pico' and 'emacs', each of which can be started from the command line,
optionally supplying the name of a file to be edited. 'vi' is arguably the
most ubiquitous as well as the least intuitive of the three. 'pico' is
relatively straightforward for a new user, though not as often installed on
systems. If you don't have 'pico', you may have a similar editor called
'nano'. 'emacs' is highly extensible and fairly widely available, but can be
somewhat unwieldy in a non-X environment. The newer versions each come with
online help, and offline help can be found in the manual and info pages for
each (please see the section on Linux Manual and Info pages). Many programs
use the 'EDITOR' environment variable to determine which text editor to start
when editing is required.

ROOT USER

Upon installation, almost all distributions set up the default administrative
user with the username 'root'. There are many things on the system that only
'root' (or a similarly privileged user) can do, one of which is installing the
NVIDIA Linux Driver. WE MUST EMPHASIZE THAT ASSUMING THE IDENTITY OF 'root' IS
INHERENTLY RISKY AND AS 'root' IT IS RELATIVELY EASY TO CORRUPT YOUR SYSTEM OR
OTHERWISE RENDER IT UNUSABLE. There are three ways to become 'root'. You may
log in as 'root' as you would any other user, you may use the switch user
command ('su') at the command prompt, or, on some systems, use the 'sudo'
utility, which allows users to run programs as 'root' while keeping a log of
their actions. This last method is useful in case a user inadvertently causes
damage to the system and cannot remember what he has done (or prefers not to
admit what he has done). It is generally a good practice to remain 'root' only
as long as is necessary to accomplish the task requiring 'root' privileges
(another useful feature of the 'sudo' utility).

BOOTING TO A DIFFERENT RUNLEVEL

Runlevels in Linux dictate which services are started and stopped
automatically when the system boots or shuts down. The runlevels typically
range from 0 to 6, with runlevel 5 typically starting the X window system as
part of the services (runlevel 0 is actually a system halt, and 6 is a system
reboot). It is good practice to install the NVIDIA Linux Driver while X is not
running, and it is a good idea to prevent X from starting on reboot in case
there are problems with the installation (otherwise you may find yourself with
a broken system that automatically tries to start X, but then hangs during the
startup, preventing you from doing the repairs necessary to fix X). Depending
on your network setup, runlevels 1, 2 or 3 should be sufficient for installing
the Driver. Level 3 typically includes networking services, so if utilities
used by the system during installation depend on a remote filesystem, Levels 1
and 2 will be insufficient. If your system typically boots to a console with a
command prompt, you should not need to change anything. If your system
typically boots to the X window system with a graphical login and desktop, you
must both exit X and change your default runlevel.

On most distributions, the default runlevel is stored in the file
'/etc/inittab', although you may have to consult the guide for your own
distribution. The line that indicates the default runlevel appears as

    id:n:initdefault:

or similar, where "n" indicates the number of the runlevel. '/etc/inittab'
must be edited as root. Please read the sections on editing files and root
user if you are unfamiliar with this concept. Also, it is recommended that you
create a copy of the file prior to editing it, particularly if you are new to
Linux text editors, in case you accidentally corrupt the file:

    # cp /etc/inittab /etc/inittab.original

The line should be edited such that an appropriate runlevel is the default (1,
2, or 3 on most systems):

    id:3:initdefault:

After saving the changes, exit X. After the Driver installation is complete,
you may revert the default runlevel to its original state, either by editing
the '/etc/inittab' again or by moving your backup copy back to its original
name.

Different distributions provide different ways to exit X. On many systems, the
'init' utility will change the current runlevel. This can be used to change to
a runlevel in which X is not running.

    # init 3

There are other methods by which to exit X. Please consult your distribution.

LINUX MANUAL AND INFO PAGES

System manual or info pages are usually installed during installation. These
pages are typically up-to-date and generally contain a comprehensive listing
of the use of programs and utilities on the system. Also, many programs
include the --help option, which usually prints a list of common options for
that program. To view the manual page for a command, enter

    % man commandname

at the command prompt, where commandname refers to the command in which you
are interested. Similarly, entering

    % info commandname

will bring up the info page for the command. Depending on the application, one
or the other may be more up-to-date. The interface for the info system is
interactive and navigable. If you are unable to locate the man page for the
command you are interested in, you may need to add additional elements to your
'MANPATH' environment variable. Please see the section on environment
variables.

                                 - FOOTNOTES -

[1] Windows is a registered trademark of Microsoft Corporation in the United
    States and other countries.

______________________________________________________________________________

Chapter 9. Acknowledgements
______________________________________________________________________________

 'nvidia-installer' was inspired by the 'loki_update' tool:
http://www.lokigames.com/development/loki_update.php3/

The FTP and HTTP support in 'nvidia-installer' is based upon 'snarf 7.0':
http://www.xach.com/snarf/

The self-extracting archive (aka '.run' file) is generated using
'makeself.sh': http://www.megastep.org/makeself/

______________________________________________________________________________

Appendix A. Supported NVIDIA Graphics Chips
______________________________________________________________________________

For the most complete and accurate listing of supported GPUs, please see the
Supported Products List, available from the NVIDIA Linux x86 Graphics Driver
download page. Please go to http://www.nvidia.com/object/unix.html, follow the
Archive link under the Linux x86 heading, follow the link for the 96.43.23
driver, and then go to the Supported Products List.


    NVIDIA chip name                      Device PCI ID
    ----------------------------------    ----------------------------------
    GeForce 6800 Ultra                    0x0040
    GeForce 6800                          0x0041
    GeForce 6800 XE                       0x0043
    GeForce 6800 XT                       0x0044
    GeForce 6800 GT                       0x0045
    GeForce 6800 GT                       0x0046
    GeForce 6800 GS                       0x0047
    GeForce 6800 XT                       0x0048
    Quadro FX 4000                        0x004E
    GeForce 7800 GTX                      0x0090
    GeForce 7800 GTX                      0x0091
    GeForce 7800 GT                       0x0092
    GeForce 7800 GS                       0x0093
    GeForce Go 7800                       0x0098
    GeForce Go 7800 GTX                   0x0099
    Quadro FX 4500                        0x009D
    GeForce 6800 GS                       0x00C0
    GeForce 6800                          0x00C1
    GeForce 6800 LE                       0x00C2
    GeForce 6800 XT                       0x00C3
    GeForce Go 6800                       0x00C8
    GeForce Go 6800 Ultra                 0x00C9
    Quadro FX Go1400                      0x00CC
    Quadro FX 3450/4000 SDI               0x00CD
    Quadro FX 1400                        0x00CE
    GeForce 6800/GeForce 6800 Ultra       0x00F0
    GeForce 6600/GeForce 6600 GT          0x00F1
    GeForce 6600                          0x00F2
    GeForce 6200                          0x00F3
    GeForce 6600 LE                       0x00F4
    GeForce 7800 GS                       0x00F5
    GeForce 6800 GS                       0x00F6
    Quadro FX 3400/4400                   0x00F8
    GeForce 6800 Ultra                    0x00F9
    GeForce PCX 5750                      0x00FA
    GeForce PCX 5900                      0x00FB
    Quadro FX 330/GeForce PCX 5300        0x00FC
    Quadro NVS 280 PCI-E/Quadro FX 330    0x00FD
    Quadro FX 1300                        0x00FE
    GeForce PCX 4300                      0x00FF
    GeForce2 MX/MX 400                    0x0110
    GeForce2 MX 100/200                   0x0111
    GeForce2 Go                           0x0112
    Quadro2 MXR/EX/Go                     0x0113
    GeForce 6600 GT                       0x0140
    GeForce 6600                          0x0141
    GeForce 6600 LE                       0x0142
    GeForce 6600 VE                       0x0143
    GeForce Go 6600                       0x0144
    GeForce 6610 XL                       0x0145
    GeForce Go 6600 TE/6200 TE            0x0146
    GeForce Go 6600                       0x0148
    GeForce Go 6600 GT                    0x0149
    Quadro NVS 440                        0x014A
    Quadro FX 550                         0x014C
    Quadro FX 540                         0x014E
    GeForce 6200                          0x014F
    GeForce 6500                          0x0160
    GeForce 6200 TurboCache(TM)           0x0161
    GeForce Go 6200                       0x0164
    Quadro NVS 285                        0x0165
    GeForce Go 6400                       0x0166
    GeForce Go 6200                       0x0167
    GeForce Go 6400                       0x0168
    GeForce4 MX 460                       0x0170
    GeForce4 MX 440                       0x0171
    GeForce4 MX 420                       0x0172
    GeForce4 MX 440-SE                    0x0173
    GeForce4 440 Go                       0x0174
    GeForce4 420 Go                       0x0175
    GeForce4 420 Go 32M                   0x0176
    GeForce4 460 Go                       0x0177
    Quadro4 550 XGL                       0x0178
    GeForce4 440 Go 64M                   0x0179
    Quadro NVS                            0x017A
    Quadro4 500 GoGL                      0x017C
    GeForce4 410 Go 16M                   0x017D
    GeForce4 MX 440 with AGP8X            0x0181
    GeForce4 MX 440SE with AGP8X          0x0182
    GeForce4 MX 420 with AGP8X            0x0183
    GeForce4 MX 4000                      0x0185
    Quadro4 580 XGL                       0x0188
    Quadro NVS with AGP8X                 0x018A
    Quadro4 380 XGL                       0x018B
    Quadro NVS 50 PCI                     0x018C
    GeForce2 Integrated GPU               0x01A0
    GeForce 7300 LE                       0x01D1
    Quadro NVS 110M                       0x01D7
    GeForce Go 7300                       0x01D7
    GeForce Go 7400                       0x01D8
    Quadro NVS 110M                       0x01DA
    Quadro NVS 120M                       0x01DB
    Quadro FX 350M                        0x01DC
    Quadro FX 350                         0x01DE
    GeForce 7300 GS                       0x01DF
    GeForce4 MX Integrated GPU            0x01F0
    GeForce3                              0x0200
    GeForce3 Ti 200                       0x0201
    GeForce3 Ti 500                       0x0202
    Quadro DCC                            0x0203
    GeForce 6800                          0x0211
    GeForce 6800 LE                       0x0212
    GeForce 6800 GT                       0x0215
    GeForce 6800 XT                       0x0218
    GeForce 6150                          0x0240
    GeForce 6150 LE                       0x0241
    GeForce 6100                          0x0242
    GeForce4 Ti 4600                      0x0250
    GeForce4 Ti 4400                      0x0251
    GeForce4 Ti 4200                      0x0253
    Quadro4 900 XGL                       0x0258
    Quadro4 750 XGL                       0x0259
    Quadro4 700 XGL                       0x025B
    GeForce4 Ti 4800                      0x0280
    GeForce4 Ti 4200 with AGP8X           0x0281
    GeForce4 Ti 4800 SE                   0x0282
    GeForce4 4200 Go                      0x0286
    Quadro4 980 XGL                       0x0288
    Quadro4 780 XGL                       0x0289
    Quadro4 700 GoGL                      0x028C
    GeForce 7900 GTX                      0x0290
    GeForce 7900 GT                       0x0291
    GeForce Go 7900 GS                    0x0298
    GeForce Go 7900 GTX                   0x0299
    Quadro FX 2500M                       0x029A
    Quadro FX 1500M                       0x029B
    Quadro FX 5500                        0x029C
    Quadro FX 3500                        0x029D
    Quadro FX 1500                        0x029E
    Quadro FX 4500 X2                     0x029F
    GeForce 7600 GS                       0x02E1
    GeForce FX 5800 Ultra                 0x0301
    GeForce FX 5800                       0x0302
    Quadro FX 2000                        0x0308
    Quadro FX 1000                        0x0309
    GeForce FX 5600 Ultra                 0x0311
    GeForce FX 5600                       0x0312
    GeForce FX 5600XT                     0x0314
    GeForce FX Go5600                     0x031A
    GeForce FX Go5650                     0x031B
    Quadro FX Go700                       0x031C
    GeForce FX 5200                       0x0320
    GeForce FX 5200 Ultra                 0x0321
    GeForce FX 5200                       0x0322
    GeForce FX 5200LE                     0x0323
    GeForce FX Go5200                     0x0324
    GeForce FX Go5250                     0x0325
    GeForce FX 5500                       0x0326
    GeForce FX 5100                       0x0327
    GeForce FX Go5200 32M/64M             0x0328
    Quadro NVS 280 PCI                    0x032A
    Quadro FX 500/600 PCI                 0x032B
    GeForce FX Go53xx                     0x032C
    GeForce FX Go5100                     0x032D
    GeForce FX 5900 Ultra                 0x0330
    GeForce FX 5900                       0x0331
    GeForce FX 5900XT                     0x0332
    GeForce FX 5950 Ultra                 0x0333
    GeForce FX 5900ZT                     0x0334
    Quadro FX 3000                        0x0338
    Quadro FX 700                         0x033F
    GeForce FX 5700 Ultra                 0x0341
    GeForce FX 5700                       0x0342
    GeForce FX 5700LE                     0x0343
    GeForce FX 5700VE                     0x0344
    GeForce FX Go5700                     0x0347
    GeForce FX Go5700                     0x0348
    Quadro FX Go1000                      0x034C
    Quadro FX 1100                        0x034E
    GeForce 7600 GT                       0x0391
    GeForce 7600 GS                       0x0392
    GeForce Go 7600                       0x0398
    Quadro FX 560                         0x039E


Below are the legacy GPUs that are no longer supported in the unified driver.
These GPUs will continue to be maintained through the special legacy NVIDIA
GPU driver releases.


    NVIDIA chip name                      Device PCI ID
    ----------------------------------    ----------------------------------
    RIVA TNT                              0x0020
    RIVA TNT2/TNT2 Pro                    0x0028
    RIVA TNT2 Ultra                       0x0029
    Vanta/Vanta LT                        0x002C
    RIVA TNT2 Model 64/Model 64 Pro       0x002D
    Aladdin TNT2                          0x00A0
    GeForce 256                           0x0100
    GeForce DDR                           0x0101
    Quadro                                0x0103
    GeForce2 GTS/GeForce2 Pro             0x0150
    GeForce2 Ti                           0x0151
    GeForce2 Ultra                        0x0152
    Quadro2 Pro                           0x0153


______________________________________________________________________________

Appendix B. Minimum Software Requirements
______________________________________________________________________________



    Software Element         Supported versions       Check With...
    ---------------------    ---------------------    ---------------------
    Linux kernel             2.4.7 and newer          `cat /proc/version`
    XFree86*                 4.0.1 and newer          `XFree86 -version`
    X.Org*                   1.0, 1.1, 1.2, 1.3,      `Xorg -version`
                             1.4, 1.5, 1.6, 1.7,  
                             1.8, 1.9, 1.10, 1.11,
                             1.12                 
    Kernel modutils          2.1.121 and newer        `insmod -v`

* It is only required that you have one of XFree86 or X.Org, not both.
Sometimes very recent versions are not supported immediately following
release, but we aim to support all new versions as soon as possible.

If you need to build the NVIDIA kernel module:

    Software Element         Min Requirement          Check With...
    ---------------------    ---------------------    ---------------------
    binutils                 2.9.5                    `size --version`
    GNU make                 3.77                     `make --version`
    gcc                      2.91.66                  `gcc --version`
    glibc                    2.0                      `ls /lib/libc.so.* >
                                                      6`


If you build from source RPMs:

    Required Software Element             Check With...
    ----------------------------------    ----------------------------------
    spec-helper rpm                       `rpm -qi spec-helper`


All official stable kernel releases from 2.4.0 and up are supported;
"prerelease" versions such as "2.4.3-pre2" are not supported, nor are
development series kernels such as 2.3.x or 2.5.x. The Linux kernel can be
downloaded from http://www.kernel.org or one of its mirrors.

binutils and gcc can be retrieved from http://www.gnu.org or one of its
mirrors.

If you are using XFree86, but do not have a file '/var/log/XFree86.0.log',
then you probably have a 3.x version of XFree86 and must upgrade.

If you are setting up XFree86 4.x for the first time, it is often easier to
begin with one of the open source drivers that ships with XFree86 (either
"nv", "vga" or "vesa"). Once XFree86 is operating properly with the open
source driver, you may then switch to the NVIDIA driver.

Note that newer NVIDIA GPUs may not work with older versions of the "nv"
driver shipped with XFree86. For example, the "nv" driver that shipped with
XFree86 version 4.0.1 did not recognize the GeForce2 family and the Quadro2
MXR GPUs. This was fixed in XFree86 version 4.0.2. XFree86 can be retrieved
from http://www.xfree86.org.

These software packages may also be available through your Linux distributor.

______________________________________________________________________________

Appendix C. Installed Components
______________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set consists of the following components
(filenames in parenthesis are the full names of the components after
installation; "x.y.z" denotes the current version. In these cases appropriate
symlinks are created during installation):

   o An X driver (/usr/X11R6/lib/modules/drivers/nvidia_drv.so); this driver
     is needed by the X server to use your NVIDIA hardware.

   o A GLX extension module for X
     (/usr/X11R6/lib/modules/extensions/libglx.so.x.y.z); this module is used
     by the X server to provide server-side GLX support.

   o An OpenGL library (/usr/lib/libGL.so.x.y.z); this library provides the
     API entry points for all OpenGL and GLX function calls. It is linked to
     at run-time by OpenGL applications.

   o An OpenGL core library (/usr/lib/libGLcore.so.x.y.z); this library is
     implicitly used by libGL and by libglx. It contains the core accelerated
     3D functionality. You should not explicitly load it in your X config file
     -- that is taken care of by libglx.

   o Two XvMC (X-Video Motion Compensation) libraries: a static library and a
     shared library (/usr/X11R6/lib/libXvMCNVIDIA.a,
     /usr/X11R6/lib/libXvMCNVIDIA.so.x.y.z); please see Appendix N for
     details.

   o A kernel module (/lib/modules/`uname -r`/video/nvidia.o or
     /lib/modules/`uname -r`/kernel/drivers/video/nvidia.o); this kernel
     module provides low-level access to your NVIDIA hardware for all of the
     above components. It is generally loaded into the kernel when the X
     server is started, and is used by the X driver and OpenGL. nvidia.o
     consists of two pieces: the binary-only core, and a kernel interface that
     must be compiled specifically for your kernel version. Note that the
     Linux kernel does not have a consistent binary interface like the X
     server, so it is important that this kernel interface be matched with the
     version of the kernel that you are using. This can either be accomplished
     by compiling yourself, or using precompiled binaries provided for the
     kernels shipped with some of the more common Linux distributions.

   o OpenGL and GLX header files (/usr/include/GL/gl.h,
     /usr/include/GL/glext.h, /usr/include/GL/glx.h, and
     /usr/include/GL/glext.h); these are also installed in
     /usr/share/doc/NVIDIA_GLX-1.0/include/GL/. You can request that these
     files not be included in /usr/include/GL/ by passing the
     "--no-opengl-headers" option to the .run file during installation.

   o The nvidia-tls libraries (/usr/lib/libnvidia-tls.so.x.y.z and
     /usr/lib/tls/libnvidia-tls.so.x.y.z); these files provide thread local
     storage support for the NVIDIA OpenGL libraries (libGL, libGLcore, and
     libglx). Each nvidia-tls library provides support for a particular thread
     local storage model (such as ELF TLS), and the one appropriate for your
     system will be loaded at run time.

   o The application nvidia-installer (/usr/bin/nvidia-installer) is NVIDIA's
     tool for installing and updating NVIDIA drivers. Please see Chapter 2 for
     a more thorough description.


Problems will arise if applications use the wrong version of a library. This
can be the case if there are either old libGL libraries or stale symlinks left
lying around. If you think there may be something awry in your installation,
check that the following files are in place (these are all the files of the
NVIDIA Accelerated Linux Driver Set, as well as their symlinks):

    /usr/X11R6/lib/modules/drivers/nvidia_drv.so

    /usr/X11R6/lib/modules/extensions/libglx.so.x.y.z
    /usr/X11R6/lib/modules/extensions/libglx.so -> libglx.so.x.y.z

    (may also be in /usr/lib/modules or /usr/lib/xorg/modules)

    /usr/lib/libGL.so.x.y.z
    /usr/lib/libGL.so.x -> libGL.so.x.y.z
    /usr/lib/libGL.so -> libGL.so.x

    /usr/lib/libGLcore.so.x.y.z
    /usr/lib/libGLcore.so.x -> libGLcore.so.x.y.z

    /lib/modules/`uname -r`/video/nvidia.o, or
    /lib/modules/`uname -r`/kernel/drivers/video/nvidia.o

If there are other libraries whose "soname" conflicts with that of the NVIDIA
libraries, ldconfig may create the wrong symlinks. It is recommended that you
manually remove or rename conflicting libraries (be sure to rename clashing
libraries to something that ldconfig will not look at -- we have found that
prepending "XXX" to a library name generally does the trick), rerun
'ldconfig', and check that the correct symlinks were made. Some libraries that
often create conflicts are "/usr/X11R6/lib/libGL.so*" and
"/usr/X11R6/lib/libGLcore.so*".

If the libraries appear to be correct, then verify that the application is
using the correct libraries. For example, to check that the application
/usr/X11R6/bin/glxgears is using the NVIDIA libraries, run:

    % ldd /usr/X11R6/bin/glxgears
        linux-gate.so.1 =>  (0xffffe000)
        libGL.so.1 => /usr/lib/libGL.so.1 (0xb7ed3000)
        libXp.so.6 => /usr/lib/libXp.so.6 (0xb7eca000)
        libXext.so.6 => /usr/lib/libXext.so.6 (0xb7eb9000)
        libX11.so.6 => /usr/lib/libX11.so.6 (0xb7dd4000)
        libpthread.so.0 => /lib/libpthread.so.0 (0xb7d82000)
        libm.so.6 => /lib/libm.so.6 (0xb7d5f000)
        libc.so.6 => /lib/libc.so.6 (0xb7c47000)
        libGLcore.so.1 => /usr/lib/libGLcore.so.1 (0xb6c2f000)
        libnvidia-tls.so.1 => /usr/lib/tls/libnvidia-tls.so.1 (0xb6c2d000)
        libdl.so.2 => /lib/libdl.so.2 (0xb6c29000)
        /lib/ld-linux.so.2 (0xb7fb2000)

Check the files being used for libGL and libGLcore -- if they are something
other than the NVIDIA libraries, then you will need to either remove the
libraries that are getting in the way, or adjust your ld search path using the
'LD_LIBRARY_PATH' environment variable. You may wish to consult the man pages
for 'ldconfig' and 'ldd'.

______________________________________________________________________________

Appendix D. X Config Options
______________________________________________________________________________

The following driver options are supported by the NVIDIA X driver. They may be
specified either in the Screen or Device sections of the X config file.

X Config Options

Option "NvAGP" "integer"

    Configure AGP support. Integer argument can be one of:
    
        Value             Behavior
        --------------    ---------------------------------------------------
        0                 disable AGP
        1                 use NVIDIA's internal AGP support, if possible
        2                 use AGPGART, if possible
        3                 use any AGP support (try AGPGART, then NVIDIA's
                          AGP)
    
    Please note that NVIDIA's internal AGP support cannot work if AGPGART is
    either statically compiled into your kernel or is built as a module and
    loaded into your kernel. Please see Appendix F for details. Default: 3.

Option "NoLogo" "boolean"

    Disable drawing of the NVIDIA logo splash screen at X startup. Default:
    the logo is drawn.

Option "LogoPath" "string"

    Sets the path to the PNG file to be used as the logo splash screen at X
    startup. If the PNG file specified has a bKGD (background color) chunk,
    then the screen is cleared to the color it specifies. Otherwise, the
    screen is cleared to black. The logo file must be owned by root and must
    not be writable by a non-root group. Default: The built-in NVIDIA logo is
    used.

Option "RenderAccel" "boolean"

    Enable or disable hardware acceleration of the RENDER extension. Default:
    hardware acceleration of the RENDER extension is enabled.

Option "NoRenderExtension" "boolean"

    Disable the RENDER extension. Other than recompiling it, the X server does
    not seem to have another way of disabling this. Fortunately, we can
    control this from the driver so we export this option. This is useful in
    depth 8 where RENDER would normally steal most of the default colormap.
    Default: RENDER is offered when possible.

Option "UBB" "boolean"

    Enable or disable the Unified Back Buffer on Quadro-based GPUs (Quadro4
    NVS excluded); please see Appendix K for a description of UBB. This option
    has no effect on non-Quadro chipsets. Default: UBB is on for Quadro
    chipsets.

Option "NoFlip" "boolean"

    Disable OpenGL flipping; please see Appendix K for a description. Default:
    OpenGL will swap by flipping when possible.

Option "Dac8Bit" "boolean"

    Most Quadro products by default use a 10-bit color look-up table (LUT);
    setting this option to TRUE forces these graphics chips to use an 8-bit
    (LUT). Default: a 10-bit LUT is used, when available.

Option "Overlay" "boolean"

    Enables RGB workstation overlay visuals. This is only supported on Quadro4
    and Quadro FX chips (Quadro4 NVS excluded) in depth 24. This option causes
    the server to advertise the SERVER_OVERLAY_VISUALS root window property
    and GLX will report single- and double-buffered, Z-buffered 16-bit overlay
    visuals. The transparency key is pixel 0x0000 (hex). There is no gamma
    correction support in the overlay plane. This feature requires XFree86
    version 4.1.0 or newer, or the X.Org X server. Quadros 500 and 550 XGL
    have additional restrictions, namely, overlays are not supported in
    TwinView mode or with virtual desktops wider than 2046 pixels or taller
    than 2047. Quadro 7xx/9xx and Quadro FX will offer overlay visuals in
    these modes (TwinView, or virtual desktops larger than 2046x2047), but the
    overlay will be emulated with a substantial performance penalty. RGB
    workstation overlays are not supported when the Composite extension is
    enabled. Dynamic TwinView is disabled when Overlays are enabled. Default:
    off.

    UBB must be enabled when overlays are enabled (this is the default
    behavior).

Option "CIOverlay" "boolean"

    Enables Color Index workstation overlay visuals with identical
    restrictions to Option "Overlay" above. The server will offer visuals both
    with and without a transparency key. These are depth 8 PseudoColor
    visuals. Enabling Color Index overlays on X servers older than XFree86 4.3
    will force the RENDER extension to be disabled due to bugs in the RENDER
    extension in older X servers. Color Index workstation overlays are not
    supported when the Composite extension is enabled. Default: off.

    UBB must be enabled when overlays are enabled (this is the default
    behavior).

Option "TransparentIndex" "integer"

    When color index overlays are enabled, use this option to choose which
    pixel is used for the transparent pixel in visuals featuring transparent
    pixels. This value is clamped between 0 and 255 (Note: some applications
    such as Alias's Maya require this to be zero in order to work correctly).
    Default: 0.

Option "OverlayDefaultVisual" "boolean"

    When overlays are used, this option sets the default visual to an overlay
    visual thereby putting the root window in the overlay. This option is not
    recommended for RGB overlays. Default: off.

Option "RandRRotation" "boolean"

    Enable rotation support for the XRandR extension. This allows use of the
    XRandR X server extension for configuring the screen orientation through
    rotation. This feature is supported on GeForce2 or better hardware using
    depth 24. This requires an X.Org X 6.8.1 or newer X server. This feature
    does not work with hardware overlays, and emulated overlays will be used
    instead at a substantial performance penalty. See Appendix U for details.
    Default: off.

Option "Rotate" "string"

    Enable static rotation support. Unlike the RandRRotation option above,
    this option takes effect as soon as the X server is started and will work
    with older versions of X. This feature is supported on GeForce2 or better
    hardware using depth 24. This feature does not work with hardware
    overlays, and emulated overlays will be used instead at a substantial
    performance penalty. This option is not compatible with the RandR
    extension. Valid rotations are "normal", "left", "inverted", and "right".
    Default: off.

Option "AllowDDCCI" "boolean"

    Enables DDC/CI support in the NV-CONTROL X extension. DDC/CI is a
    mechanism for communication between your computer and your display device.
    This can be used to set the values normally controlled through your
    display device's On Screen Display. Please see the DDC/CI NV-CONTROL
    attributes in 'NVCtrl.h' and functions in 'NVCtrlLib.h' in the
    'nvidia-settings' source code. Default: off (DDC/CI is disabled).

Option "SWCursor" "boolean"

    Enable or disable software rendering of the X cursor. Default: off.

Option "HWCursor" "boolean"

    Enable or disable hardware rendering of the X cursor. Default: on.

Option "CursorShadow" "boolean"

    Enable or disable use of a shadow with the hardware accelerated cursor;
    this is a black translucent replica of your cursor shape at a given offset
    from the real cursor. Default: off (no cursor shadow).

Option "CursorShadowAlpha" "integer"

    The alpha value to use for the cursor shadow; only applicable if
    CursorShadow is enabled. This value must be in the range [0, 255] -- 0 is
    completely transparent; 255 is completely opaque. Default: 64.

Option "CursorShadowXOffset" "integer"

    The offset, in pixels, that the shadow image will be shifted to the right
    from the real cursor image; only applicable if CursorShadow is enabled.
    This value must be in the range [0, 32]. Default: 4.

Option "CursorShadowYOffset" "integer"

    The offset, in pixels, that the shadow image will be shifted down from the
    real cursor image; only applicable if CursorShadow is enabled. This value
    must be in the range [0, 32]. Default: 2.

Option "ConnectedMonitor" "string"

    Allows you to override what the NVIDIA kernel module detects is connected
    to your video card. This may be useful, for example, if you use a KVM
    (keyboard, video, mouse) switch and you are switched away when X is
    started. In such a situation, the NVIDIA kernel module cannot detect what
    display devices are connected, and the NVIDIA X driver assumes you have a
    single CRT.

    Valid values for this option are "CRT" (cathode ray tube), "DFP" (digital
    flat panel), or "TV" (television); if using TwinView, this option may be a
    comma-separated list of display devices; e.g.: "CRT, CRT" or "CRT, DFP".

    It is generally recommended to not use this option, but instead use the
    "UseDisplayDevice" option.

    NOTE: anything attached to a 15 pin VGA connector is regarded by the
    driver as a CRT. "DFP" should only be used to refer to digital flat panels
    connected via a DVI port.

    Default: string is NULL (the NVIDIA driver will detect the connected
    display devices).

Option "UseDisplayDevice" "string"

    When assigning display devices to X screens, the NVIDIA X driver by
    default assigns display devices in the order they are found (looking first
    at CRTs, then at DFPs, and finally at TVs). This option can be used to
    override this assignment. For example, if both a CRT and a DFP are
    connected, you could specify:
    
        Option "UseDisplayDevice" "DFP"
    
    to make the X screen use the DFP, even though it would have used a CRT by
    default.

    Note the subtle difference between this option and the "ConnectedMonitor"
    option: the "ConnectedMonitor" option overrides what display devices are
    actually detected, while the "UseDisplayDevice" option controls which of
    the detected display devices will be used on this X screen.

Option "UseEdidFreqs" "boolean"

    This option controls whether the NVIDIA X driver will use the HorizSync
    and VertRefresh ranges given in a display device's EDID, if any. When
    UseEdidFreqs is set to True, EDID-provided range information will override
    the HorizSync and VertRefresh ranges specified in the Monitor section. If
    a display device does not provide an EDID, or the EDID does not specify an
    hsync or vrefresh range, then the X server will default to the HorizSync
    and VertRefresh ranges specified in the Monitor section of your X config
    file. These frequency ranges are used when validating modes for your
    display device.

    Default: True (EDID frequencies will be used)

Option "UseEDID" "boolean"

    By default, the NVIDIA X driver makes use of a display device's EDID, when
    available, during construction of its mode pool. The EDID is used as a
    source for possible modes, for valid frequency ranges, and for collecting
    data on the physical dimensions of the display device for computing the
    DPI (see Appendix Y). However, if you wish to disable the driver's use of
    the EDID, you can set this option to False:
    
        Option "UseEDID" "FALSE"
    
    Note that, rather than globally disable all uses of the EDID, you can
    individually disable each particular use of the EDID; e.g.,
    
        Option "UseEDIDFreqs" "FALSE"
        Option "UseEDIDDpi" "FALSE"
        Option "ModeValidation" "NoEdidModes"
    
    Default: True (use EDID).

Option "IgnoreEDID" "boolean"

    This option is deprecated, and no longer affects behavior of the X driver.
    See the "UseEDID" option for details.

Option "NoDDC" "boolean"

    Synonym for "IgnoreEDID". This option is deprecated, and no longer affects
    behavior of the X driver. See the "UseEDID" option for details.

Option "UseInt10Module" "boolean"

    Enable use of the X Int10 module to soft-boot all secondary cards, rather
    than POSTing the cards through the NVIDIA kernel module. Default: off
    (POSTing is done through the NVIDIA kernel module).

Option "TwinView" "boolean"

    Enable or disable TwinView. Please see Appendix G for details. Default:
    off (TwinView is disabled).

Option "TwinViewOrientation" "string"

    Controls the relationship between the two display devices when using
    TwinView. Takes one of the following values: "RightOf" "LeftOf" "Above"
    "Below" "Clone". Please see Appendix G for details. Default: string is
    NULL.

Option "SecondMonitorHorizSync" "range(s)"

    This option is like the HorizSync entry in the Monitor section, but is for
    the second monitor when using TwinView. Please see Appendix G for details.
    Default: none.

Option "SecondMonitorVertRefresh" "range(s)"

    This option is like the VertRefresh entry in the Monitor section, but is
    for the second monitor when using TwinView. Please see Appendix G for
    details. Default: none.

Option "MetaModes" "string"

    This option describes the combination of modes to use on each monitor when
    using TwinView. Please see Appendix G for details. Default: string is
    NULL.

Option "NoTwinViewXineramaInfo" "boolean"

    When in TwinView, the NVIDIA X driver normally provides a Xinerama
    extension that X clients (such as window managers) can use to discover the
    current TwinView configuration, such as where each display device is
    positioned within the X screen. Some window mangers get confused by this
    information, so this option is provided to disable this behavior. Default:
    false (TwinView Xinerama information is provided).

Option "TwinViewXineramaInfoOrder" "string"

    When the NVIDIA X driver provides TwinViewXineramaInfo (see the
    NoTwinViewXineramaInfo X config option), it by default reports the
    currently enabled display devices in the order "CRT, DFP, TV". The
    TwinViewXineramaInfoOrder X config option can be used to override this
    order.

    The option string is a comma-separated list of display device names. The
    display device names can either be general (e.g, "CRT", which identifies
    all CRTs), or specific (e.g., "CRT-1", which identifies a particular CRT).
    Not all display devices need to be identified in the option string;
    display devices that are not listed will be implicitly appended to the end
    of the list, in their default order.

    Note that TwinViewXineramaInfoOrder tracks all display devices that could
    possibly be connected to the GPU, not just the ones that are currently
    enabled. When reporting the Xinerama information, the NVIDIA X driver
    walks through the display devices in the order specified, only reporting
    enabled display devices.

    Examples:
    
            "DFP"
            "TV, DFP"
            "DFP-1, DFP-0, TV, CRT"
    
    In the first example, any enabled DFPs would be reported first (any
    enabled CRTs or TVs would be reported afterwards). In the second example,
    any enabled TVs would be reported first, then any enabled DFPs (any
    enabled CRTs would be reported last). In the last example, if DFP-1 were
    enabled, it would be reported first, then DFP-0, then any enabled TVs, and
    then any enabled CRTs; finally, any other enabled DFPs would be reported.

    Default: "CRT, DFP, TV"

Option "TVStandard" "string"

    Please see Appendix H for details on configuring TV-out.

Option "TVOutFormat" "string"

    Please see Appendix H for details on configuring TV-out.

Option "TVOverScan" "Decimal value in the range 0.0 to 1.0"

    Valid values are in the range 0.0 through 1.0; Please see Appendix H for
    details on configuring TV-out.

Option "Stereo" "integer"

    Enable offering of quad-buffered stereo visuals on Quadro. Integer
    indicates the type of stereo equipment being used:
    
        Value             Equipment
        --------------    ---------------------------------------------------
        1                 DDC glasses. The sync signal is sent to the
                          glasses via the DDC signal to the monitor. These
                          usually involve a passthrough cable between the
                          monitor and video card.
        2                 "Blueline" glasses. These usually involve a
                          passthrough cable between the monitor and video
                          card. The glasses know which eye to display based
                          on the length of a blue line visible at the bottom
                          of the screen. When in this mode, the root window
                          dimensions are one pixel shorter in the Y
                          dimension than requested. This mode does not work
                          with virtual root window sizes larger than the
                          visible root window size (desktop panning).
        3                 Onboard stereo support. This is usually only found
                          on professional cards. The glasses connect via a
                          DIN connector on the back of the video card.
        4                 TwinView clone mode stereo (aka "passive" stereo).
                          On video cards that support TwinView, the left eye
                          is displayed on the first display, and the right
                          eye is displayed on the second display. This is
                          normally used in conjunction with special
                          projectors to produce 2 polarized images which are
                          then viewed with polarized glasses. To use this
                          stereo mode, you must also configure TwinView in
                          clone mode with the same resolution, panning
                          offset, and panning domains on each display.
        5                 Vertical interlaced stereo mode, for use with
                          SeeReal Stereo Digital Flat Panels.
        6                 Color interleaved stereo mode, for use with
                          Sharp3D Stereo Digital Flat Panels.
    
    Stereo is only available on Quadro cards. Stereo options 1, 2, and 3 (aka
    "active" stereo) may be used with TwinView if all modes within each
    MetaMode have identical timing values. Please see Appendix J for
    suggestions on making sure the modes within your MetaModes are identical.
    The identical ModeLine requirement is not necessary for Stereo option 4
    ("passive" stereo). Currently, stereo operation may be "quirky" on the
    original Quadro (NV10) chip and left-right flipping may be erratic. We are
    trying to resolve this issue for a future release. Default: 0 (Stereo is
    not enabled).

    UBB must be enabled when stereo is enabled (this is the default behavior).

    Stereo options 1, 2, and 3 (aka "active" stereo) are not supported on
    digital flat panels.

    Multi-GPU cards (such as the Quadro FX 4500 X2) provide a single connector
    for onboard stereo support (option 3), which is tied to the bottommost
    GPU. In order to synchronize onboard stereo with the other GPU, you must
    use a G-Sync device (see Appendix X for details).

Option "AllowDFPStereo" "boolean"

    By default, the NVIDIA X driver performs a check which disables active
    stereo (stereo options 1, 2, and 3) if the X screen is driving a DFP. The
    "AllowDFPStereo" option bypasses this check.

Option "ForceStereoFlipping" "boolean"

    Stereo flipping is the process by which left and right eyes are displayed
    on alternating vertical refreshes. Normally, stereo flipping is only
    performed when a stereo drawable is visible. This option forces stereo
    flipping even when no stereo drawables are visible.

    This is to be used in conjunction with the "Stereo" option. If "Stereo" is
    0, the "ForceStereoFlipping" option has no effect. If otherwise, the
    "ForceStereoFlipping" option will force the behavior indicated by the
    "Stereo" option, even if no stereo drawables are visible. This option is
    useful in a multiple-screen environment in which a stereo application is
    run on a different screen than the stereo master.

    Possible values:
    
        Value             Behavior
        --------------    ---------------------------------------------------
        0                 Stereo flipping is not forced. The default
                          behavior as indicated by the "Stereo" option is
                          used.
        1                 Stereo flipping is forced. Stereo is running even
                          if no stereo drawables are visible. The stereo
                          mode depends on the value of the "Stereo" option.
    
    Default: 0 (Stereo flipping is not forced). Note that active stereo is not
    supported on digital flat panels.

Option "XineramaStereoFlipping" "boolean"

    By default, when using Stereo with Xinerama, all physical X screens having
    a visible stereo drawable will stereo flip. Use this option to allow only
    one physical X screen to stereo flip at a time.

    This is to be used in conjunction with the "Stereo" and "Xinerama"
    options. If "Stereo" is 0 or "Xinerama" is 0, the "XineramaStereoFlipping"
    option has no effect.

    If you wish to have all X screens stereo flip all the time, please see the
    "ForceStereoFlipping" option.

    Possible values:
    
        Value             Behavior
        --------------    ---------------------------------------------------
        0                 Stereo flipping is enabled on one X screen at a
                          time. Stereo is enabled on the first X screen
                          having the stereo drawable.
        1                 Stereo flipping in enabled on all X screens.
    
    Default: 1 (Stereo flipping is enabled on all X screens).

Option "NoBandWidthTest" "boolean"

    As part of mode validation, the X driver tests if a given mode fits within
    the hardware's memory bandwidth constraints. This option disables this
    test. Default: false (the memory bandwidth test is performed).

Option "IgnoreDisplayDevices" "string"

    This option tells the NVIDIA kernel module to completely ignore the
    indicated classes of display devices when checking what display devices
    are connected. You may specify a comma-separated list containing any of
    "CRT", "DFP", and "TV". For example:
    
    Option "IgnoreDisplayDevices" "DFP, TV"
    
    will cause the NVIDIA driver to not attempt to detect if any digital flat
    panels or TVs are connected. This option is not normally necessary;
    however, some video BIOSes contain incorrect information about what
    display devices may be connected, or what i2c port should be used for
    detection. These errors can cause long delays in starting X. If you are
    experiencing such delays, you may be able to avoid this by telling the
    NVIDIA driver to ignore display devices which you know are not connected.
    NOTE: anything attached to a 15 pin VGA connector is regarded by the
    driver as a CRT. "DFP" should only be used to refer to digital flat panels
    connected via a DVI port.

Option "MultisampleCompatibility" "boolean"

    Enable or disable the use of separate front and back multisample buffers.
    Enabling this will consume more memory but is necessary for correct output
    when rendering to both the front and back buffers of a multisample or FSAA
    drawable. This option is necessary for correct operation of SoftImage XSI.
    Default: false (a single multisample buffer is shared between the front
    and back buffers).

Option "NoPowerConnectorCheck" "boolean"

    The NVIDIA X driver will abort X server initialization if it detects that
    a GPU that requires an external power connector does not have an external
    power connector plugged in. This option can be used to bypass this test.
    Default: false (the power connector test is performed).

Option "XvmcUsesTextures" "boolean"

    Forces XvMC to use the 3D engine for XvMCPutSurface requests rather than
    the video overlay. Default: false (video overlay is used when available).

Option "AllowGLXWithComposite" "boolean"

    Enables GLX even when the Composite X extension is loaded. ENABLE AT YOUR
    OWN RISK. OpenGL applications will not display correctly in many
    circumstances with this setting enabled.

    This option is intended for use on X.Org X servers older than X11R6.9.0.
    On X11R6.9.0 or newer X servers, NVIDIA's OpenGL implementation interacts
    properly by default with the Composite X extension and this option should
    not be needed. However, on X11R6.9.0 or newer X servers, support for GLX
    with Composite can be disabled by setting this option to False.

    Default: false (GLX is disabled when Composite is enabled on X servers
    older than X11R6.9.0).

Option "UseCompositeWrapper" "boolean"

    Enables the X server's "composite wrapper", which performs coordinate
    translations necessary for the Composite extension.

    Default: false (the NVIDIA X driver performs its own coordinate
    translation).

Option "AddARGBGLXVisuals" "boolean"

    Adds a 32-bit ARGB visual for each supported OpenGL configuration. This
    allows applications to use OpenGL to render with alpha transparency into
    32-bit windows and pixmaps. This option requires the Composite extension.
    ENABLE AT YOUR OWN RISK. Some OpenGL applications may display incorrectly
    when this setting is enabled. Default: No visuals are added.

Option "DisableGLXRootClipping" "boolean"

    If enabled, no clipping will be performed on rendering done by OpenGL in
    the root window. This is needed by some OpenGL-based composite managers to
    function correctly, as they draw the contents of redirected windows
    directly into the root window using OpenGL.

Option "DamageEvents" "boolean"

    Use OS-level events to efficiently notify X when a client has performed
    direct rendering to a window that needs to be composited. This will
    significantly improve performance and interactivity when using GLX
    applications with a composite manager running. It will also affect
    applications using GLX when rotation is enabled. This option is currently
    incompatible with SLI and MultiGPU modes and will be disabled if either
    are used. Enabled by default.

Option "ExactModeTimingsDVI" "boolean"

    Forces the initialization of the X server with the exact timings specified
    in the ModeLine. Default: false (for DVI devices, the X server initializes
    with the closest mode in the EDID list).

Option "Coolbits" "integer"

    Enables support in the NV-CONTROL X extension for manipulating GPU clock
    settings. When this option is set to "1" the nvidia-settings utility will
    contain a page labeled "Clock Frequencies" through which clock settings
    can be manipulated. Coolbits is only available on GeForce FX, Quadro FX,
    and newer GPUs. Default 0 (support is disabled).

    WARNING: this may cause system damage and void warranties. This utility
    can run your computer system out of the manufacturer's design
    specifications, including, but not limited to: higher system voltages,
    above normal temperatures, excessive frequencies, and changes to BIOS that
    may corrupt the BIOS. Your computer's operating system may hang and result
    in data loss or corrupted images. Depending on the manufacturer of your
    computer system, the computer system, hardware and software warranties may
    be voided, and you may not receive any further manufacturer support.
    NVIDIA does not provide customer service support for the Coolbits option.
    It is for these reasons that absolutely no warranty or guarantee is either
    express or implied. Before enabling and using, you should determine the
    suitability of the utility for your intended use, and you shall assume all
    responsibility in connection therewith.

Option "MultiGPU" "string"

    This option controls the configuration of MultiGPU rendering in supported
    configurations.
    
        Value                               Behavior
        --------------------------------    --------------------------------
        0, no, off, false, Single           Use only a single GPU when
                                            rendering
        1, yes, on, true, Auto              Enable MultiGPU and allow the
                                            driver to automatically select
                                            the appropriate rendering mode.
        AFR                                 Enable MultiGPU and use the
                                            Alternate Frame Rendering mode.
        SFR                                 Enable MultiGPU and use the
                                            Split Frame Rendering mode.
        AA                                  Enable MultiGPU and use
                                            antialiasing. Use this in
                                            conjunction with full scene
                                            antialiasing to improve visual
                                            quality.
    
    
Option "SLI" "string"

    This option controls the configuration of SLI rendering in supported
    configurations.
    
        Value                               Behavior
        --------------------------------    --------------------------------
        0, no, off, false, Single           Use only a single GPU when
                                            rendering
        1, yes, on, true, Auto              Enable SLI and allow the driver
                                            to automatically select the
                                            appropriate rendering mode.
        AFR                                 Enable SLI and use the Alternate
                                            Frame Rendering mode.
        SFR                                 Enable SLI and use the Split
                                            Frame Rendering mode.
        AA                                  Enable SLI and use SLI
                                            Antialiasing. Use this in
                                            conjunction with full scene
                                            antialiasing to improve visual
                                            quality.
        AFRofAA                             Enable SLI and use SLI Alternate
                                            Frame Rendering of Antialiasing
                                            mode. Use this in conjunction
                                            with full scene antialiasing to
                                            improve visual quality. This
                                            option is only valid for SLI
                                            configurations with 4 GPUs.
    
    
Option "TripleBuffer" "boolean"

    Enable or disable the use of triple buffering. If this option is enabled,
    OpenGL windows that sync to vblank and are double-buffered will be given a
    third buffer. This decreases the time an application stalls while waiting
    for vblank events, but increases latency slightly (delay between user
    input and displayed result).

Option "DPI" "string"

    This option specifies the Dots Per Inch for the X screen; for example:
    
        Option "DPI" "75 x 85"
    
    will set the horizontal DPI to 75 and the vertical DPI to 85. By default,
    the X driver will compute the DPI of the X screen from the EDID of any
    connected display devices. See Appendix Y for details. Default: string is
    NULL (disabled).

Option "UseEdidDpi" "string"

    By default, the NVIDIA X driver computes the DPI of an X screen based on
    the physical size of the display device, as reported in the EDID, and the
    size in pixels of the first mode to be used on the display device. If
    multiple display devices are used by the X screen, then the NVIDIA X
    screen will choose which display device to use. This option can be used to
    specify which display device to use. The string argument can be a display
    device name, such as:
    
        Option "UseEdidDpi" "DFP-0"
    
    or the argument can be "FALSE" to disable use of EDID-based DPI
    calculations:
    
        Option "UseEdidDpi" "FALSE"
    
    See Appendix Y for details. Default: string is NULL (the driver computes
    the DPI from the EDID of a display device and selects the display device).

Option "ConstantDPI" "boolean"

    By default on X.Org 6.9 or newer X servers, the NVIDIA X driver recomputes
    the size in millimeters of the X screen whenever the size in pixels of the
    X screen is changed using XRandR, such that the DPI remains constant.

    This behavior can be disabled (which means that the size in millimeters
    will not change when the size in pixels of the X screen changes) by
    setting the "ConstantDPI" option to "FALSE"; e.g.,
    
        Option "ConstantDPI" "FALSE"
    
    ConstantDPI defaults to True.

    On X servers older than X.Org 6.9, the NVIDIA X driver cannot change the
    size in millimeters of the X screen. Therefore the DPI of the X screen
    will change when XRandR changes the size in pixels of the X screen. The
    driver will behave as if ConstantDPI was forced to FALSE.

Option "CustomEDID" "string"

    This option forces the X driver to use the EDID specified in a file rather
    than the display's EDID. You may specify a semicolon separated list of
    display names and filename pairs. The display name is any of "CRT-0",
    "CRT-1", "DFP-0", "DFP-1", "TV-0", "TV-1". The file contains a raw EDID
    (e.g., a file generated by nvidia-settings).

    For example:
    
        Option "CustomEDID" "CRT-0:/tmp/edid1.bin; DFP-0:/tmp/edid2.bin"
    
    will assign the EDID from the file /tmp/edid1.bin to the display device
    CRT-0, and the EDID from the file /tmp/edid2.bin to the display device
    DFP-0.

Option "ModeValidation" "string"

    This option provides fine-grained control over each stage of the mode
    validation pipeline, disabling individual mode validation checks. This
    option should only very rarely be used.

    The option string is a semicolon-separated list of comma-separated lists
    of mode validation arguments. Each list of mode validation arguments can
    optionally be prepended with a display device name.
    
        "<dpy-0>: <tok>, <tok>; <dpy-1>: <tok>, <tok>, <tok>; ..."
    
    
    Possible arguments:
    
       o "AllowNon60HzDFPModes": some lower quality TMDS encoders are only
         rated to drive DFPs at 60Hz; the driver will determine when only 60Hz
         DFP modes are allowed. This argument disables this stage of the mode
         validation pipeline.
    
       o "NoMaxPClkCheck": each mode has a pixel clock; this pixel clock is
         validated against the maximum pixel clock of the hardware (for a DFP,
         this is the maximum pixel clock of the TMDS encoder, for a CRT, this
         is the maximum pixel clock of the DAC). This argument disables the
         maximum pixel clock checking stage of the mode validation pipeline.
    
       o "NoEdidMaxPClkCheck": a display device's EDID can specify the maximum
         pixel clock that the display device supports; a mode's pixel clock is
         validated against this pixel clock maximum. This argument disables
         this stage of the mode validation pipeline.
    
       o "AllowInterlacedModes": interlaced modes are not supported on all
         NVIDIA GPUs; the driver will discard interlaced modes on GPUs where
         interlaced modes are not supported; this argument disables this stage
         of the mode validation pipeline.
    
       o "NoMaxSizeCheck": each NVIDIA GPU has a maximum resolution that it
         can drive; this argument disables this stage of the mode validation
         pipeline.
    
       o "NoHorizSyncCheck": a mode's horizontal sync is validated against the
         range of valid horizontal sync values; this argument disables this
         stage of the mode validation pipeline.
    
       o "NoVertRefreshCheck": a mode's vertical refresh rate is validated
         against the range of valid vertical refresh rate values; this
         argument disables this stage of the mode validation pipeline.
    
       o "NoWidthAlignmentCheck": the alignment of a mode's visible width is
         validated against the capabilities of the GPU; normally, a mode's
         visible width must be a multiple of 8. This argument disables this
         stage of the mode validation pipeline.
    
       o "NoDFPNativeResolutionCheck": when validating for a DFP, a mode's
         size is validated against the native resolution of the DFP; this
         argument disables this stage of the mode validation pipeline.
    
       o "NoVirtualSizeCheck": if the X configuration file requests a specific
         virtual screen size, a mode cannot be larger than that virtual size;
         this argument disables this stage of the mode validation pipeline.
    
       o "NoVesaModes": when constructing the mode pool for a display device,
         the X driver uses a built-in list of VESA modes as one of the mode
         sources; this argument disables use of these built-in VESA modes.
    
       o "NoEdidModes": when constructing the mode pool for a display device,
         the X driver uses any modes listed in the display device's EDID as
         one of the mode sources; this argument disables use of EDID-specified
         modes.
    
       o "NoXServerModes": when constructing the mode pool for a display
         device, the X driver uses the built-in modes provided by the core
         XFree86/Xorg X server as one of the mode sources; this argument
         disables use of these modes. Note that this argument does not disable
         custom ModeLines specified in the X config file; see the
         "NoCustomModes" argument for that.
    
       o "NoCustomModes": when constructing the mode pool for a display
         device, the X driver uses custom ModeLines specified in the X config
         file (through the "Mode" or "ModeLine" entries in the Monitor
         Section) as one of the mode sources; this argument disables use of
         these modes.
    
       o "NoPredefinedModes": when constructing the mode pool for a display
         device, the X driver uses additional modes predefined by the NVIDIA X
         driver; this argument disables use of these modes.
    
       o "NoUserModes": additional modes can be added to the mode pool
         dynamically, using the NV-CONTROL X extension; this argument
         prohibits user-specified modes via the NV-CONTROL X extension.
    
    
    Examples:
    
        Option "ModeValidation" "NoMaxPClkCheck"
    
    disable the maximum pixel clock check when validating modes on all display
    devices.
    
        Option "ModeValidation" "CRT-0: NoEdidModes, NoMaxPClkCheck; DFP-0:
    NoVesaModes"
    
    do not use EDID modes and do not perform the maximum pixel clock check on
    CRT-0, and do not use VESA modes on DFP-0.

Option "UseEvents" "boolean"

    Enables the use of system events in some cases when the X driver is
    waiting for the hardware. The X driver can briefly spin through a tight
    loop when waiting for the hardware. With this option the X driver instead
    sets an event handler and waits for the hardware through the 'poll()'
    system call. Default: the use of the events is disabled.

Option "FlatPanelProperties" "string"

    This option requests particular properties for all or a subset of the
    connected flat panels.

    The option string is a semicolon-separated list of comma-separated
    property=value pairs. Each list of property=value pairs can optionally be
    prepended with a flat panel name.
    
        "<DFP-0>: <property=value>, <property=value>; <DFP-1>:
    <property=value>; ..."
    
    
    Recognized properties:
    
       o "Scaling": controls the flat panel scaling mode; possible values are:
         'Default' (the driver will use whichever scaling state is current),
         'Native' (the driver will use the flat panel's scaler, if possible),
         'Scaled' (the driver will use the NVIDIA GPU's scaler, if possible),
         'Centered' (the driver will center the image, if possible), and
         'aspect-scaled' (the X driver will scale with the NVIDIA GPU's
         scaler, but keep the aspect ratio correct).
    
       o "Dithering": controls the flat panel dithering mode; possible values
         are: 'Default' (the driver will decide when to dither), 'Enabled'
         (the driver will always dither, if possible), and 'Disabled' (the
         driver will never dither).
    
    
    Examples:
    
        Option "FlatPanelProperties" "Scaling = Centered"
    
    set the flat panel scaling mode to centered on all flat panels.
    
        Option "FlatPanelProperties" "DFP-0: Scaling = Centered; DFP-1:
    Scaling = Scaled, Dithering = Enabled"
    
    set DFP-0's scaling mode to centered, set DFP-1's scaling mode to scaled
    and its dithering mode to enabled.

Option "ProbeAllGpus" "boolean"

    When the NVIDIA X driver initializes, it probes all GPUs in the system,
    even if no X screens are configured on them. This is done so that the X
    driver can report information about all the system's GPUs through the
    NV-CONTROL X extension. This option can be set to FALSE to disable this
    behavior, such that only GPUs with X screens configured on them will be
    probed. Default: all GPUs in the system are probed.

Option "DynamicTwinView" "boolean"

    Enable or disable support for dynamically configuring TwinView on this X
    screen. When DynamicTwinView is enabled (the default), the refresh rate of
    a mode (reported through XF86VidMode or XRandR) does not correctly report
    the refresh rate, but instead is a unique number such that each MetaMode
    has a different value. This is to guarantee that MetaModes can be uniquely
    identified by XRandR.

    When DynamicTwinView is disabled, the refresh rate reported through XRandR
    will be accurate, but NV-CONTROL clients such as nvidia-settings will not
    be able to dynamically manipulate the X screen's MetaModes. TwinView can
    still be configured from the X config file when DynamicTwinView is
    disabled.

    Default: DynamicTwinView is enabled.

Option "IncludeImplicitMetaModes" "boolean"

    When the X server starts, a mode pool is created per display device,
    containing all the mode timings that the NVIDIA X driver determined to be
    valid for the display device. However, the only MetaModes that are made
    available to the X server are the ones explicitly requested in the X
    configuration file.

    It is convenient for fullscreen applications to be able to change between
    the modes in the mode pool, even if a given target mode was not explicitly
    requested in the X configuration file.

    To facilitate this, the NVIDIA X driver will, if only one display device
    is in use when the X server starts, implicitly add MetaModes for all modes
    in the display device's mode pool. This makes all the modes in the mode
    pool available to full screen applications that use the XF86VidMode or
    XRandR X extensions.

    To prevent this behavior, and only add MetaModes that are explicitly
    requested in the X configuration file, set this option to FALSE.

    Default: IncludeImplicitMetaModes is enabled.

Option "LoadKernelModule" "boolean"

    Normally, the NVIDIA Linux X driver module will attempt to load the NVIDIA
    Linux kernel module. Set this option to "off" to disable automatic loading
    of the NVIDIA kernel module by the NVIDIA X driver. Default: on (the
    driver loads the kernel module).


______________________________________________________________________________

Appendix E. OpenGL Environment Variable Settings
______________________________________________________________________________

FULL SCENE ANTIALIASING

Antialiasing is a technique used to smooth the edges of objects in a scene to
reduce the jagged "stairstep" effect that sometimes appears. Full-scene
antialiasing is supported on GeForce or newer hardware. By setting the
appropriate environment variable, you can enable full-scene antialiasing in
any OpenGL application on these GPUs.

Several antialiasing methods are available and you can select between them by
setting the __GL_FSAA_MODE environment variable appropriately. Note that
increasing the number of samples taken during FSAA rendering may decrease
performance.

The following tables describe the possible values for __GL_FSAA_MODE and the
effects that they have on various NVIDIA GPUs.



    __GL_FSAA_MODE     GeForce, GeForce2, Quadro, and Quadro2 Pro
    ---------------    ------------------------------------------------------
    0                  FSAA disabled
    1                  FSAA disabled
    2                  FSAA disabled
    3                  1.5 x 1.5 Supersampling
    4                  2 x 2 Supersampling
    5                  FSAA disabled
    6                  FSAA disabled
    7                  FSAA disabled




    __GL_FSAA_MODE     GeForce4 MX, GeForce4 4xx Go, Quadro4 380,550,580
                       XGL, and Quadro4 NVS
    ---------------    ------------------------------------------------------
    0                  FSAA disabled
    1                  2x Bilinear Multisampling
    2                  2x Quincunx Multisampling
    3                  FSAA disabled
    4                  2 x 2 Supersampling
    5                  FSAA disabled
    6                  FSAA disabled
    7                  FSAA disabled




    __GL_FSAA_MODE     GeForce3, Quadro DCC, GeForce4 Ti, GeForce4 4200 Go,
                       and Quadro4 700,750,780,900,980 XGL
    ---------------    ------------------------------------------------------
    0                  FSAA disabled
    1                  2x Bilinear Multisampling
    2                  2x Quincunx Multisampling
    3                  FSAA disabled
    4                  4x Bilinear Multisampling
    5                  4x Gaussian Multisampling
    6                  2x Bilinear Multisampling by 4x Supersampling
    7                  FSAA disabled




    __GL_FSAA_MODE     GeForce FX, GeForce 6xxx, GeForce 7xxx, Quadro FX
    ---------------    ------------------------------------------------------
    0                  FSAA disabled
    1                  2x Bilinear Multisampling
    2                  2x Quincunx Multisampling
    3                  FSAA disabled
    4                  4x Bilinear Multisampling
    5                  4x Gaussian Multisampling
    6                  2x Bilinear Multisampling by 4x Supersampling
    7                  4x Bilinear Multisampling by 4x Supersampling
    8                  4x Bilinear Multisampling by 2x Supersampling
                       (available on GeForce FX and later GPUs; not
                       available on Quadro GPUs)


ANISOTROPIC TEXTURE FILTERING

Automatic anisotropic texture filtering can be enabled by setting the
environment variable __GL_LOG_MAX_ANISO. The possible values are:

    __GL_LOG_MAX_ANISO                    Filtering Type
    ----------------------------------    ----------------------------------
    0                                     No anisotropic filtering
    1                                     2x anisotropic filtering
    2                                     4x anisotropic filtering
    3                                     8x anisotropic filtering
    4                                     16x anisotropic filtering

4x and greater are only available on GeForce3 or newer GPUs; 16x is only
available on GeForce 6800 or newer GPUs.

VBLANK SYNCING

Setting the environment variable __GL_SYNC_TO_VBLANK to a non-zero value will
force glXSwapBuffers to sync to your monitor's vertical refresh (perform a
swap only during the vertical blanking period).

When using __GL_SYNC_TO_VBLANK with TwinView, OpenGL can only sync to one of
the display devices; this may cause tearing corruption on the display device
to which OpenGL is not syncing. You can use the environment variable
__GL_SYNC_DISPLAY_DEVICE to specify to which display device OpenGL should
sync. You should set this environment variable to the name of a display
device; for example "CRT-1". Please look for the line "Connected display
device(s):" in your X log file for a list of the display devices present and
their names. You may also find it useful to review Appendix G (Configuring
Twinview) and the section on Ensuring Identical Mode Timings in Appendix J.

DISABLING CPU-SPECIFIC FEATURES

Setting the environment variable __GL_FORCE_GENERIC_CPU to a non-zero value
will inhibit the use of CPU-specific features such as MMX, SSE, or 3DNOW!. Use
of this option may result in performance loss.

CONTROLLING FORK(2) HANDLING BEHAVIOR

In order to clean up and reinitialize system resources the NVIDIA OpenGL
implementation needs to be aware of fork(2) system calls. The mechanism used
by the NVIDIA OpenGL implementation to detect fork(2) system calls does not
work well on systems using the LinuxThreads implementation of pthreads. For
thread safety the NVIDIA OpenGL implementation disables its fork(2) detection
on LinuxThreads-based systems. Setting the environment variable
__GL_ALWAYS_HANDLE_FORK to a non-zero value will enable fork(2) detection on
all systems. Setting the environment variable __GL_ALWAYS_HANDLE_FORK will
reduce thread safety on LinuxThreads-based systems. It is strongly recommended
to only set the __GL_ALWAYS_HANDLE_FORK environment variable when running
single threaded applications that are known to use fork.

______________________________________________________________________________

Appendix F. Configuring AGP
______________________________________________________________________________

There are several choices for configuring the NVIDIA kernel module's use of
AGP on Linux. You can choose to either use NVIDIA's builtin AGP driver
(NvAGP), or the AGP driver that comes with the Linux kernel (AGPGART). This is
controlled through the "NvAGP" option in your X config file:

    Option "NvAGP" "0"  ... disables AGP support
    Option "NvAGP" "1"  ... use NvAGP, if possible
    Option "NvAGP" "2"  ... use AGPGART, if possible
    Option "NvAGP" "3"  ... try AGPGART; if that fails, try NvAGP

The default is 3 (the default was 1 until after 1.0-1251).

You should use the AGP driver that works best with your AGP chipset. If you
are experiencing problems with stability, you may want to start by disabling
AGP and seeing if that solves the problems. Then you can experiment with the
AGP driver configuration.

You can query the current AGP status at any time via the '/proc' filesystem
interface (see Appendix M).

To use the Linux 2.4 AGPGART driver, you will need to compile it with your
kernel and either statically link it in, or build it as a module and load it.
To use the Linux 2.6 AGPGART driver, both the AGPGART frontend module,
'apggart.ko', and the backend module for your AGP chipset ('nvidia-agp.ko',
'intel-agp.ko', 'via-agp.ko', ...) need to be statically linked into the
kernel, or built as modules and loaded.

NVIDIA's builtin AGP support is unavailable if an AGPGART backend driver is
loaded into the kernel. On Linux 2.4, it is recommended that you compile
AGPGART as a module and make sure that it is not loaded when trying to use
NVIDIA's AGP driver. On Linux 2.6, the 'agpgart.ko' frontend module will
always be loaded, as it is used by the NVIDIA kernel module to determine if an
AGPGART backend module is loaded. When the NVIDIA AGP driver is to be used on
a Linux 2.6 system, it is recommended that you make sure the AGPGART backend
drivers are built as modules and that they are not loaded.

Please also note that changing AGP drivers generally requires a reboot before
the changes actually take effect.

If you are using a recent Linux 2.6 kernel that has the Linux AGPGART driver
statically linked in (some distribution kernels do), you can pass the

    agp=off

parameter to the kernel (via LILO or GRUB, for example) to disable AGPGART
support. As of Linux 2.6.11, most AGPGART backend drivers should respect this
parameter.

The following AGP chipsets are supported by NVIDIA's AGP driver; for all other
chipsets it is recommended that you use the AGPGART module.

    Supported AGP Chipsets
    ----------------------------------------------------------------------
    Intel 440LX
    Intel 440BX
    Intel 440GX
    Intel 815 ("Solano")
    Intel 820 ("Camino")
    Intel 830M
    Intel 840 ("Carmel")
    Intel 845 ("Brookdale")
    Intel 845G
    Intel 850 ("Tehama")
    Intel 855 ("Odem")
    Intel 860 ("Colusa")
    Intel 865G ("Springdale")
    Intel 875P ("Canterwood")
    Intel E7205 ("Granite Bay")
    Intel E7505 ("Placer")
    AMD 751 ("Irongate")
    AMD 761 ("IGD4")
    AMD 762 ("IGD4 MP")
    AMD 8151 ("Lokar")
    VIA 8371
    VIA 82C694X
    VIA KT133
    VIA KT266
    VIA KT400
    VIA P4M266
    VIA P4M266A
    VIA P4X400
    VIA K8T800
    VIA K8N800
    VIA PT880
    VIA KT880
    RCC CNB20LE
    RCC 6585HE
    Micron SAMDDR ("Samurai")
    Micron SCIDDR ("Scimitar")
    NVIDIA nForce
    NVIDIA nForce2
    NVIDIA nForce3
    ALi 1621
    ALi 1631
    ALi 1647
    ALi 1651
    ALi 1671
    SiS 630
    SiS 633
    SiS 635
    SiS 645
    SiS 646
    SiS 648
    SiS 648FX
    SiS 650
    SiS 651
    SiS 655
    SiS 655FX
    SiS 661
    SiS 730
    SiS 733
    SiS 735
    SiS 745
    SiS 755
    ATI RS200M


If you are experiencing AGP stability problems, you should be aware of the
following:

Additional AGP Information

Support for the processor's Page Size Extension on Athlon Processors

    Some Linux kernels have a conflicting cache attribute bug that is exposed
    by advanced speculative caching in newer AMD Athlon family processors (AMD
    Athlon XP, AMD Athlon 4, AMD Athlon MP, and Models 6 and above AMD Duron).
    This kernel bug usually shows up under heavy use of accelerated 3D
    graphics with an AGP graphics card.

    Linux distributions based on kernel 2.4.19 and later *should* incorporate
    the bug fix, but older kernels require help from the user in ensuring that
    a small portion of advanced speculative caching is disabled (normally done
    through a kernel patch) and a boot option is specified in order to apply
    the whole fix.

    NVIDIA's driver automatically disables the small portion of advanced
    speculative caching for the affected AMD processors without the need to
    patch the kernel; it can be used even on kernels which do already
    incorporate the kernel bug fix. Additionally, for older kernels the user
    performs the boot option portion of the fix by explicitly disabling 4MB
    pages. This can be done from the boot command line by specifying:
    
        mem=nopentium
    
    Or by adding the following line to /etc/lilo.conf:
    
        append = "mem=nopentium"
    
    
AGP Rate

    You may want to decrease the AGP rate setting if you are seeing lockups
    with the value you are currently using. You can do so by extracting the
    '.run' file:
    
        # sh NVIDIA-Linux-x86-96.43.23-pkg1.run --extract-only
        # cd NVIDIA-Linux-x86-96.43.23-pkg1/usr/src/nv/
    
    Then edit os-registry.c, and make the following changes:
    
        - static int NVreg_ReqAGPRate = 15;
        + static int NVreg_ReqAGPRate = 4;   /* force AGP Rate to 4x */
    
    or
    
        + static int NVreg_ReqAGPRate = 2;   /* force AGP Rate to 2x */
    
    or
    
        + static int NVreg_ReqAGPRate = 1;   /* force AGP Rate to 1x */
    
    and enable the "ReqAGPRate" parameter:
    
        - { NULL, "ReqAGPRate",     &NVreg_ReqAGPRate,      0 },
        + { NULL, "ReqAGPRate",     &NVreg_ReqAGPRate,      1 },
    
    Then recompile and load the new kernel module. To do this, run
    'nvidia-installer' with the -n command line option:
    
        # cd ../../..; ./nvidia-installer -n
    
    
AGP drive strength BIOS setting (Via-based motherboards)

    Many Via-based motherboards allow adjusting the AGP drive strength in the
    system BIOS. The setting of this option largely affects system stability,
    the range between 0xEA and 0xEE seems to work best for NVIDIA hardware.
    Setting either nibble to 0xF generally results in severe stability
    problems.

    If you decide to experiment with this, you need to be aware of the fact
    that you are doing so at your own risk and that you may render your system
    unbootable with improper settings until you reset the setting to a working
    value (w/ a PCI graphics card or by resetting the BIOS to its default
    values).

System BIOS version

    Make sure you have the latest system BIOS provided by the motherboard
    manufacturer.

    On ALi1541 and ALi1647 chipsets, NVIDIA drivers disable AGP to work around
    timing and signal integrity problems. You can force AGP to be enabled on
    these chipsets by setting NVreg_EnableALiAGP to 1. Note that this may
    cause the system to become unstable.

    Early system BIOS revisions for the ASUS A7V8X-X KT400 motherboard
    misconfigure the chipset when an AGP 2.x graphics card is installed; if X
    hangs on your ASUS KT400 system with either Linux AGPGART or NvAGP enabled
    and the installed graphics card is not an AGP 8x device, make sure that
    you have the latest system BIOS installed.


______________________________________________________________________________

Appendix G. Configuring TwinView
______________________________________________________________________________

The TwinView feature is supported on NVIDIA GPUs that support dual-display
functionality, such as the GeForce2 MX, GeForce2 Go, Quadro2 MXR, Quadro2 Go,
GeForce4, Quadro4, GeForce FX, Quadro FX, Quadro NVS, GeForce 6 Series and
GeForce 7 Series GPUs. Please consult with your video card vendor to confirm
that TwinView is supported on your graphics card.

TwinView is a mode of operation where two display devices (digital flat
panels, CRTs, and TVs) can display the contents of a single X screen in any
arbitrary configuration. This method of multiple monitor use has several
distinct advantages over other techniques (such as Xinerama):


   o A single X screen is used. The NVIDIA driver conceals all information
     about multiple display devices from the X server; as far as X is
     concerned, there is only one screen.

   o Both display devices share one frame buffer. Thus, all the functionality
     present on a single display (e.g., accelerated OpenGL) is available with
     TwinView.

   o No additional overhead is needed to emulate having a single desktop.


If you are interested in using each display device as a separate X screen,
please see Appendix P.

X CONFIG TWINVIEW OPTIONS

To enable TwinView, you must specify the following option in the Device
section of your X Config file:

    Option "TwinView"

You may also use any of the following options, though they are not required:

    Option "MetaModes"                "<list of MetaModes>"

    Option "SecondMonitorHorizSync"   "<hsync range(s)>"
    Option "SecondMonitorVertRefresh" "<vrefresh range(s)>"

    Option "HorizSync"                "<hsync range(s)>"
    Option "VertRefresh"              "<vrefresh range(s)>"

    Option "TwinViewOrientation"      "<relationship of head 1 to head 0>"
    Option "ConnectedMonitor"         "<list of connected display devices>"

Please see detailed descriptions of each option below.

Alternatively, you can enable TwinView by running

    nvidia-xconfig --twinview

and restarting your X server. Or, you can configure TwinView dynamically in
the "Display Configuration" page in nvidia-settings.

DETAILED DESCRIPTION OF OPTIONS


TwinView

    This option is required to enable TwinView; without it, all other TwinView
    related options are ignored.

SecondMonitorHorizSync
SecondMonitorVertRefresh

    You specify the constraints of the second monitor through these options.
    The values given should follow the same convention as the "HorizSync" and
    "VertRefresh" entries in the Monitor section. As the XF86Config man page
    explains it: the ranges may be a comma separated list of distinct values
    and/or ranges of values, where a range is given by two distinct values
    separated by a dash. The HorizSync is given in kHz, and the VertRefresh is
    given in Hz.

    These options are normally not needed: by default, the NVIDIA X driver
    retrieves the valid frequency ranges from the display device's EDID (see
    Appendix D for a description of the "UseEdidFreqs" option). The
    SecondMonitor options will override any frequency ranges retrieved from
    the EDID.

HorizSync
VertRefresh

    Which display device is "first" and which is "second" is often unclear.
    For this reason, you may use these options instead of the SecondMonitor
    versions. With these options, you can specify a semicolon-separated list
    of frequency ranges, each optionally prepended with a display device name.
    For example:
    
        Option "HorizSync"   "CRT-0: 50-110;  DFP-0: 40-70"
        Option "VertRefresh" "CRT-0: 60-120;  DFP-0: 60"
    
    Please see Appendix R on Display Device Names for more information.

    These options are normally not needed: by default, the NVIDIA X driver
    retrieves the valid frequency ranges from the display device's EDID (see
    Appendix D for a description of the "UseEdidFreqs" option). The
    "HorizSync" and "VertRefresh" options override any frequency ranges
    retrieved from the EDID or any frequency ranges specified with the
    "SecondMonitorHorizSync" and "SecondMonitorVertRefresh" options.

MetaModes

    MetaModes are "containers" that store information about what mode should
    be used on each display device at any given time. Even if only one display
    device is actively in use, the NVIDIA X driver always uses a MetaMode to
    encapsulate the mode information per display device, so that it can
    support dynamically enabling TwinView.

    Multiple MetaModes list the combinations of modes and the sequence in
    which they should be used. When the NVIDIA driver tells X what modes are
    available, it is really the minimal bounding box of the MetaMode that is
    communicated to X, while the "per display device" mode is kept internal to
    the NVIDIA driver. In MetaMode syntax, modes within a MetaMode are comma
    separated, and multiple MetaModes are separated by semicolons. For
    example:
    
        "<mode name 0>, <mode name 1>; <mode name 2>, <mode name 3>"
    
    Where <mode name 0> is the name of the mode to be used on display device 0
    concurrently with <mode name 1> used on display device 1. A mode switch
    will then cause <mode name 2> to be used on display device 0 and <mode
    name 3> to be used on display device 1. Here is an example MetaMode:
    
        Option "MetaModes" "1280x1024,1280x1024; 1024x768,1024x768"
    
    If you want a display device to not be active for a certain MetaMode, you
    can use the mode name "NULL", or simply omit the mode name entirely:
    
        "1600x1200, NULL; NULL, 1024x768"
    
    or
    
        "1600x1200; , 1024x768"
    
    Optionally, mode names can be followed by offset information to control
    the positioning of the display devices within the virtual screen space;
    e.g.,
    
        "1600x1200 +0+0, 1024x768 +1600+0; ..."
    
    Offset descriptions follow the conventions used in the X "-geometry"
    command line option; i.e., both positive and negative offsets are valid,
    though negative offsets are only allowed when a virtual screen size is
    explicitly given in the X config file.

    When no offsets are given for a MetaMode, the offsets will be computed
    following the value of the TwinViewOrientation option (see below). Note
    that if offsets are given for any one of the modes in a single MetaMode,
    then offsets will be expected for all modes within that single MetaMode;
    in such a case offsets will be assumed to be +0+0 when not given.

    When not explicitly given, the virtual screen size will be computed as the
    the bounding box of all MetaMode bounding boxes. MetaModes with a bounding
    box larger than an explicitly given virtual screen size will be discarded.

    A MetaMode string can be further modified with a "Panning Domain"
    specification; e.g.,
    
        "1024x768 @1600x1200, 800x600 @1600x1200"
    
    A panning domain is the area in which a display device's viewport will be
    panned to follow the mouse. Panning actually happens on two levels with
    TwinView: first, an individual display device's viewport will be panned
    within its panning domain, as long as the viewport is contained by the
    bounding box of the MetaMode. Once the mouse leaves the bounding box of
    the MetaMode, the entire MetaMode (i.e., all display devices) will be
    panned to follow the mouse within the virtual screen. Note that individual
    display devices' panning domains default to being clamped to the position
    of the display devices' viewports, thus the default behavior is just that
    viewports remain "locked" together and only perform the second type of
    panning.

    The most beneficial use of panning domains is probably to eliminate dead
    areas -- regions of the virtual screen that are inaccessible due to
    display devices with different resolutions. For example:
    
        "1600x1200, 1024x768"
    
    produces an inaccessible region below the 1024x768 display. Specifying a
    panning domain for the second display device:
    
        "1600x1200, 1024x768 @1024x1200"
    
    provides access to that dead area by allowing you to pan the 1024x768
    viewport up and down in the 1024x1200 panning domain.

    Offsets can be used in conjunction with panning domains to position the
    panning domains in the virtual screen space (note that the offset
    describes the panning domain, and only affects the viewport in that the
    viewport must be contained within the panning domain). For example, the
    following describes two modes, each with a panning domain width of 1900
    pixels, and the second display is positioned below the first:
    
        "1600x1200 @1900x1200 +0+0, 1024x768 @1900x768 +0+1200"
    
    Because it is often unclear which mode within a MetaMode will be used on
    each display device, mode descriptions within a MetaMode can be prepended
    with a display device name. For example:
    
        "CRT-0: 1600x1200,  DFP-0: 1024x768"
    
    If no MetaMode string is specified, then the X driver uses the modes
    listed in the relevant "Display" subsection, attempting to place matching
    modes on each display device.

TwinViewOrientation

    This option controls the positioning of the second display device relative
    to the first within the virtual X screen, when offsets are not explicitly
    given in the MetaModes. The possible values are:
    
        "RightOf"  (the default)
        "LeftOf"
        "Above"
        "Below"
        "Clone"
    
    When "Clone" is specified, both display devices will be assigned an offset
    of 0,0.

    Because it is often unclear which display device is "first" and which is
    "second", TwinViewOrientation can be confusing. You can further clarify
    the TwinViewOrientation with display device names to indicate which
    display device is positioned relative to which display device. For
    example:
    
        "CRT-0 LeftOf DFP-0"
    
    
ConnectedMonitor

    With this option you can override what the NVIDIA kernel module detects is
    connected to your video card. This may be useful, for example, if any of
    your display devices do not support detection using Display Data Channel
    (DDC) protocols. Valid values are a comma-separated list of display device
    names; for example:
    
        "CRT-0, CRT-1"
        "CRT"
        "CRT-1, DFP-0"
    
    WARNING: this option overrides what display devices are detected by the
    NVIDIA kernel module, and is very seldom needed. You really only need this
    if a display device is not detected, either because it does not provide
    DDC information, or because it is on the other side of a KVM
    (Keyboard-Video-Mouse) switch. In most other cases, it is best not to
    specify this option.


Just as in all X config entries, spaces are ignored and all entries are case
insensitive.

DYNAMIC TWINVIEW

Using the NV-CONTROL X extension, the display devices in use by an X screen,
the mode pool for each display device, and the MetaModes for each X screen can
be dynamically manipulated. The "Display Configuration" page in
nvidia-settings uses this functionality to modify the MetaMode list and then
uses XRandR to switch between MetaModes. This gives the ability to dynamically
configure TwinView.

The details of how this works are documented in the nv-control-dpy.c sample
NV-CONTROL client in the nvidia-settings source tarball.

Because the NVIDIA X driver can now transition into and out of TwinView
dynamically, MetaModes are always used internally by the NVIDIA X driver,
regardless of how many display devices are currently in use by the X screen
and regardless of whether the TwinView X configuration option was specified.

One implication of this implementation is that each MetaMode must be uniquely
identifiable to the XRandR X extension. Unfortunately, two MetaModes with the
same bounding box will look the same to XRandR. For example, two MetaModes
with different orientations:

    "CRT: 1600x1200 +0+0, DFP: 1600x1200 +1600+0"
    "CRT: 1600x1200 +1600+0, DFP: 1600x1200 +0+0"

will look identical to the XRandR or XF86VidMode X extensions, because they
have the same total size (3200x1200), and nvidia-settings would not be able to
use XRandR to switch between these MetaModes. To work around this limitation,
the NVIDIA X driver "lies" about the refresh rate of each MetaMode, using the
refresh rate of the MetaMode as a unique identifier.

The XRandR extension is currently being redesigned by the X.Org community, so
the refresh rate workaround may be removed at some point in the future. This
workaround can also be disabled by setting the "DynamicTwinView" X
configuration option to FALSE, which will disable NV-CONTROL support for
manipulating MetaModes, but will cause the XRandR and XF86VidMode visible
refresh rate to be accurate.


FREQUENTLY ASKED TWINVIEW QUESTIONS

Q. Nothing gets displayed on my second monitor; what is wrong?

A. Monitors that do not support monitor detection using Display Data Channel
   (DDC) protocols (this includes most older monitors) are not detectable by
   your NVIDIA card. You need to explicitly tell the NVIDIA X driver what you
   have connected using the "ConnectedMonitor" option; e.g.,
   
       Option "ConnectedMonitor" "CRT, CRT"
   
   

Q. Will window managers be able to appropriately place windows (e.g., avoiding
   placing windows across both display devices, or in inaccessible regions of
   the virtual desktop)?

A. Yes. The NVIDIA X driver provides a Xinerama extension that X clients (such
   as window managers) can use to discover the current TwinView configuration.
   Note that the Xinerama protocol provides no way to notify clients when a
   configuration change occurs, so if you modeswitch to a different MetaMode,
   your window manager will still think you have the previous configuration.
   Using the Xinerama extension, in conjunction with the XF86VidMode extension
   to get modeswitch events, window managers should be able to determine the
   TwinView configuration at any given time.

   Unfortunately, the data provided by XineramaQueryScreens() appears to
   confuse some window managers; to work around such broken window mangers,
   you can disable communication of the TwinView screen layout with the
   "NoTwinViewXineramaInfo" X config Option (please see Appendix D for
   details).

   The order that display devices are reported in via the TwinView Xinerama
   information can be configured with the TwinViewXineramaInfoOrder X
   configuration option.

   Be aware that the NVIDIA driver cannot provide the Xinerama extension if
   the X server's own Xinerama extension is being used. Explicitly specifying
   Xinerama in the X config file or on the X server commandline will prohibit
   NVIDIA's Xinerama extension from installing, so make sure that the X
   server's log file does not contain:
   
       (++) Xinerama: enabled
   
   if you want the NVIDIA driver to be able to provide the Xinerama extension
   while in TwinView.

   Another solution is to use panning domains to eliminate inaccessible
   regions of the virtual screen (see the MetaMode description above).

   A third solution is to use two separate X screens, rather than use
   TwinView. Please see Appendix P.


Q. Why can I not get a resolution of 1600x1200 on the second display device
   when using a GeForce2 MX?

A. Because the second display device on the GeForce2 MX was designed to be a
   digital flat panel, the Pixel Clock for the second display device is only
   150 MHz. This effectively limits the resolution on the second display
   device to somewhere around 1280x1024 (for a description of how Pixel Clock
   frequencies limit the programmable modes, see the XFree86 Video Timings
   HOWTO). This constraint is not present on GeForce4 or GeForce FX chips --
   the maximum pixel clock is the same on both heads.


Q. Do video overlays work across both display devices?

A. Hardware video overlays only work on the first display device. The current
   solution is that blitted video is used instead on TwinView.


Q. How are virtual screen dimensions determined in TwinView?

A. After all requested modes have been validated, and the offsets for each
   MetaMode's viewports have been computed, the NVIDIA driver computes the
   bounding box of the panning domains for each MetaMode. The maximum bounding
   box width and height is then found.

   Note that one side effect of this is that the virtual width and virtual
   height may come from different MetaModes. Given the following MetaMode
   string:
   
       "1600x1200,NULL; 1024x768+0+0, 1024x768+0+768"
   
   the resulting virtual screen size will be 1600 x 1536.


Q. Can I play full screen games across both display devices?

A. Yes. While the details of configuration will vary from game to game, the
   basic idea is that a MetaMode presents X with a mode whose resolution is
   the bounding box of the viewports for that MetaMode. For example, the
   following:
   
       Option "MetaModes" "1024x768,1024x768; 800x600,800x600"
       Option "TwinViewOrientation" "RightOf"
   
   produce two modes: one whose resolution is 2048x768, and another whose
   resolution is 1600x600. Games such as Quake 3 Arena use the VidMode
   extension to discover the resolutions of the modes currently available. To
   configure Quake 3 Arena to use the above MetaMode string, add the following
   to your q3config.cfg file:
   
       seta r_customaspect "1"
       seta r_customheight "600"
       seta r_customwidth  "1600"
       seta r_fullscreen   "1"
       seta r_mode         "-1"
   
   Note that, given the above configuration, there is no mode with a
   resolution of 800x600 (remember that the MetaMode "800x600, 800x600" has a
   resolution of 1600x600"), so if you change Quake 3 Arena to use a
   resolution of 800x600, it will display in the lower left corner of your
   screen, with the rest of the screen grayed out. To have single head modes
   available as well, an appropriate MetaMode string might be something like:
   
       "800x600,800x600; 1024x768,NULL; 800x600,NULL; 640x480,NULL"
   
   More precise configuration information for specific games is beyond the
   scope of this document, but the above examples coupled with numerous online
   sources should be enough to point you in the right direction.


______________________________________________________________________________

Appendix H. Configuring TV-Out
______________________________________________________________________________

NVIDIA GPU-based video cards with a TV-Out connector can use a television as
another display device, just like a CRT or digital flat panel. The TV can be
used by itself, or in conjunction with another display device in a TwinView or
multiple X screen configuration. If a TV is the only display device connected
to your video card, it will be used as the primary display when you boot your
system (i.e. the console will come up on the TV just as if it were a CRT).

The NVIDIA X driver populates the mode pool for the TV with all the mode sizes
that the driver supports with the given TV standard and the TV encoder on the
graphics board. These modes are given names that correspond to their
resolution; e.g., "800x600".

Because these TV modes only depend on the TV encoder and the TV standard, TV
modes do not go through normal mode validation. The X configuration options
HorizSync and VertRefresh are not used for TV mode validation.

Additionally, the NVIDIA driver contains a hardcoded list of mode sizes that
it can drive for each combination of TV encoder and TV standard. Therefore,
custom modelines in your X configuration file are ignored for TVs.

To use your TV with X, there are several relevant X configuration options:

   o The Modes in the screen section of your X configuration file; you can use
     these to request any of the modes in the mode pool which the X driver
     created for this combination of TV standard and TV encoder. Examples
     include "640x480" and "800x600". If in doubt, use "nvidia-auto-select".

   o The "TVStandard" option should be added to your screen section; valid
     values are:
     
         TVStandard       Description
         -------------    --------------------------------------------------
         "PAL-B"          used in Belgium, Denmark, Finland, Germany,
                          Guinea, Hong Kong, India, Indonesia, Italy,
                          Malaysia, The Netherlands, Norway, Portugal,
                          Singapore, Spain, Sweden, and Switzerland
         "PAL-D"          used in China and North Korea
         "PAL-G"          used in Denmark, Finland, Germany, Italy,
                          Malaysia, The Netherlands, Norway, Portugal,
                          Spain, Sweden, and Switzerland
         "PAL-H"          used in Belgium
         "PAL-I"          used in Hong Kong and The United Kingdom
         "PAL-K1"         used in Guinea
         "PAL-M"          used in Brazil
         "PAL-N"          used in France, Paraguay, and Uruguay
         "PAL-NC"         used in Argentina
         "NTSC-J"         used in Japan
         "NTSC-M"         used in Canada, Chile, Colombia, Costa Rica,
                          Ecuador, Haiti, Honduras, Mexico, Panama, Puerto
                          Rico, South Korea, Taiwan, United States of
                          America, and Venezuela
         "HD480i"         480 line interlaced
         "HD480p"         480 line progressive
         "HD720p"         720 line progressive
         "HD1080i"        1080 line interlaced
         "HD1080p"        1080 line progressive
         "HD576i"         576 line interlace
         "HD576p"         576 line progressive
     
     The line in your X config file should be something like:
     
         Option "TVStandard" "NTSC-M"
     
     If you do not specify a TVStandard, or you specify an invalid value, the
     default "NTSC-M" will be used. Note: if your country is not in the above
     list, select the country closest to your location.

   o The "UseDisplayDevice" option can be used if there are multiple display
     devices connected, and you want the connected TV to be used instead of
     the connected CRTs and/or DFPs. E.g.,
     
         Option "UseDisplayDevice" "TV"
     
     It is recommended to use the "UseDisplayDevice" option, rather than the
     "ConnectedMonitor" option.

   o The "TVOutFormat" option can be used to force the output format. Without
     this option the driver autodetects the output format. Unfortunately, it
     does not always do this correctly. The output format can be forced with
     the "TVOutFormat" option; valid values are:
     
         TVOutFormat            Description            Supported TV
                                                       standards
         -------------------    -------------------    -------------------
         "AUTOSELECT"           The driver             PAL, NTSC, HD
                                autodetects the    
                                output format      
                                (default value).   
         "COMPOSITE"            Force Composite        PAL, NTSC
                                output format      
         "SVIDEO"               Force S-Video          PAL, NTSC
                                output format      
         "COMPONENT"            Force Component        HD
                                output format, also
                                called YPrPp       
         "SCART"                Force Scart output     PAL, NTSC
                                format, also called
                                Peritel            
     
     The line in your X config file should be something like:
     
         Option "TVOutFormat" "SVIDEO"
     
     
   o The "TVOverScan" option can be used to enable Overscan, when the TV
     encoder supports it. Valid values are decimal values in the range 1.0
     (which means overscan as much as possible: make the image as large as
     possible) and 0.0 (which means disable overscanning: make the image as
     small as possible). Overscanning is disabled (0.0) by default.

The NVIDIA X driver may not restore the console correctly with XFree86
versions older than 4.3 when the console is a TV. This is due to binary
incompatibilities between XFree86 int10 modules. If you use a TV as your
console it is recommended that you upgrade to XFree86 4.3 or later.

______________________________________________________________________________

Appendix I. Configuring a Laptop
______________________________________________________________________________

INSTALLATION AND CONFIGURATION

Installation and configuration of the NVIDIA Accelerated Linux Driver Set on a
laptop is the same as for any desktop environment, with a few minor
exceptions, listed below.

Starting with the 1.0-2802 release, information about the internal flat panel
for use in initializing the display is by default generated on the fly from
data stored in the video BIOS. This can be disabled by setting the "SoftEDIDs"
kernel option to 0. If "SoftEDIDs" is turned off, then hardcoded data will be
chosen from a table, based on the value of the "Mobile" kernel option.

The "Mobile" kernel option can be set to any of the following values:

    Value              Meaning
    ---------------    ------------------------------------------------------
    0xFFFFFFFF         let the kernel module autodetect the correct value
    1                  Dell laptops
    2                  non-Compal Toshiba laptops
    3                  all other laptops
    4                  Compal Toshiba laptops
    5                  Gateway laptops

Again, the "Mobile" kernel option is only needed if SoftEDIDs is disabled;
when it is used, it is usually safest to let the kernel module autodetect the
correct value (this is the default behavior).

Should you need to alter either of these options, you may do so in any of the
following ways:

   o editing os-registry.c in the usr/src/nv/ directory of the '.run' file.

   o setting the value on the modprobe command line (e.g.: `modprobe nvidia
     NVreg_SoftEDIDs=0 NVreg_Mobile=3`)

   o adding an "options" line to your module configuration file, usually
     '/etc/modules.conf' (e.g.: "options nvidia NVreg_Mobile=5")


ADDITIONAL FUNCTIONALITY

In this section we discuss additional functionality associated with laptop
configuration.

TWINVIEW

All mobile NVIDIA chips support TwinView. TwinView on a laptop can be
configured in the same way as on a desktop machine (please refer to Appendix G
); note that in a TwinView configuration using the laptop's internal flat
panel and an external CRT, the CRT is the primary display device (specify its
HorizSync and VertRefresh in the Monitor section of your X config file) and
the flat panel is the secondary display device (specify its HorizSync and
VertRefresh through the SecondMonitorHorizSync and SecondMonitorVertRefresh
options).

The "UseEdidFreqs" X config option is enabled by default, so normally you
should not need to specify the "SecondMonitorHorizSync" and
"SecondMonitorVertRefresh" options. Please see the description of the
UseEdidFreqs option in Appendix D for details).

HOTKEY SWITCHING OF DISPLAY DEVICES

Besides TwinView, mobile NVIDIA chips also have the capacity to react to an
LCD/CRT hotkey event, toggling between each of the connected display devices
and each possible combination of the connected display devices (note that only
2 display devices may be active at a time). TwinView as configured in your X
config file and hotkey functionality are mutually exclusive -- if you enable
TwinView in your X config file, then the NVIDIA X driver will ignore LCD/CRT
hotkey events.

Another important aspect of hotkey functionality is that you can dynamically
connect and remove display devices to/from your laptop and use the hotkey to
activate and deactivate them without restarting X.

When X is started, or when a change is detected in the list of connected
display devices, a new hotkey sequence list is constructed -- this lists which
display devices will be used with each hotkey event. When a hotkey event
occurs, the next hotkey state in the sequence is chosen. Each mode requested
in the X config file is validated against each display device's constraints,
and the resulting modes are made available for that display device. If
multiple display devices are to be active at once, then the modes from each
display device are paired together; if an exact match (same resolution) cannot
be found, then the closest fit is found, and the display device with the
smaller resolution is panned within the resolution of the other display
device.

When switching away from X to a virtual terminal, the VGA console will always
be restored to the display device on which it was present when X was started.
Similarly, when switching back into X, the same display device configuration
will be used as when you switched away, regardless of what LCD/CRT hotkey
activity occurred while the virtual terminal was active.

NON-STANDARD MODES ON LCD DISPLAYS

Some users have had difficulty programming a 1400x1050 mode (the native
resolution of some laptop LCDs). In version 4.0.3, XFree86 added several
1400x1050 modes to its database of default modes, but if you are using an
older version of XFree86, the following ModeLine may be useful:

    # -- 1400x1050 --
    # 1400x1050 @ 60Hz, 65.8 kHz hsync
    Modeline "1400x1050"  129  1400 1464 1656 1960
        1050 1051 1054 1100 +HSync +VSync


KNOWN LAPTOP ISSUES

There are a few known issues associated with laptops:

   o LCD/CRT hotkey switching is not currently functioning on any Toshiba
     laptop, with the exception of the Toshiba Satellite 3000 series.

   o TwinView on Satellite 2800 series Toshiba laptops is not currently
     functioning.

   o The video overlay only works on the first display device on which you
     started X. For example, if you start X on the internal LCD, run a video
     application that uses the video overlay (uses the "Video Overlay" adapter
     advertised through the XV extension), and then hotkey switch to add a
     second display device, the video will not appear on the second display
     device. To work around this, you can either configure the video
     application to use the "Video Blitter" adapter advertised through the XV
     extension (this is always available), or hotkey switch to the display
     device on which you want to use the video overlay *before* starting X.


______________________________________________________________________________

Appendix J. Programming Modes
______________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set supports all standard VGA and VESA
modes, as well as most user-written custom mode lines; double-scan modes are
supported on all hardware. Interlaced modes are supported on all GeForce
FX/Quadro FX and newer GPUs, and certain older GPUs; the X log file will
contain a message "Interlaced video modes are supported on this GPU" if
interlaced modes are supported.

To request one or more standard modes for use in X, you can simply add a
"Modes" line such as:

    Modes "1600x1200" "1024x768" "640x480"

in the appropriate Display subsection of your X config file (please see the
XF86Config(5x) or xorg.conf(5x) man pages for details). Or, the
nvidia-xconfig(1) utility can be used to request additional modes; for
example:

    nvidia-xconfig --mode 1600x1200

See the nvidia-xconfig(1) man page for details.

DEPTH, BITS PER PIXEL, AND PITCH

While not directly a concern when programming modes, the bits used per pixel
is an issue when considering the maximum programmable resolution; for this
reason, it is worthwhile to address the confusion surrounding the terms
"depth" and "bits per pixel". Depth is how many bits of data are stored per
pixel. Supported depths are 8, 15, 16, and 24. Most video hardware, however,
stores pixel data in sizes of 8, 16, or 32 bits; this is the amount of memory
allocated per pixel. When you specify your depth, X selects the bits per pixel
(bpp) size in which to store the data. Below is a table of what bpp is used
for each possible depth:

    Depth                                 BPP
    ----------------------------------    ----------------------------------
    8                                     8
    15                                    16
    16                                    16
    24                                    32

Lastly, the "pitch" is how many bytes in the linear frame buffer there are
between one pixel's data, and the data of the pixel immediately below. You can
think of this as the horizontal resolution multiplied by the bytes per pixel
(bits per pixel divided by 8). In practice, the pitch may be more than this
product due to alignment constraints.

MAXIMUM RESOLUTIONS

The NVIDIA Accelerated Linux Driver Set and NVIDIA GPU-based video boards
support resolutions up to 4096x4096, though the maximum resolution your system
can support is also limited by the amount of video memory (see USEFUL FORMULAS
for details) and the maximum supported resolution of your display device
(monitor/flat panel/television). Also note that while use of a video overlay
does not limit the maximum resolution or refresh rate, video memory bandwidth
used by a programmed mode does affect the overlay quality.

USEFUL FORMULAS

The maximum resolution is a function both of the amount of video memory and
the bits per pixel you elect to use:

HR * VR * (bpp/8) = Video Memory Used

In other words, the amount of video memory used is equal to the horizontal
resolution (HR) multiplied by the vertical resolution (VR) multiplied by the
bytes per pixel (bits per pixel divided by eight). Technically, the video
memory used is actually the pitch times the vertical resolution, and the pitch
may be slightly greater than (HR * (bpp/8)) to accommodate the hardware
requirement that the pitch be a multiple of some value.

Please note that this is just memory usage for the frame buffer; video memory
is also used by other things, such as OpenGL and pixmap caching.

Another important relationship is that between the resolution, the pixel clock
(aka dot clock) and the vertical refresh rate:

RR = PCLK / (HFL * VFL)

In other words, the refresh rate (RR) is equal to the pixel clock (PCLK)
divided by the total number of pixels: the horizontal frame length (HFL)
multiplied by the vertical frame length (VFL) (note that these are the frame
lengths, and not just the visible resolutions). As described in the XFree86
Video Timings HOWTO, the above formula can be rewritten as:

PCLK = RR * HFL * VFL

Given a maximum pixel clock, you can adjust the RR, HFL and VFL as desired, as
long as the product of the three is consistent. The pixel clock is reported in
the log file. Your X log should contain a line like this:

    (--) NVIDIA(0): ViewSonic VPD150 (DFP-1): 165 MHz maximum pixel clock

which indicates the maximum pixel clock for that display device.

HOW MODES ARE VALIDATED

In traditional XFree86/X.Org mode validation, the X server takes as a starting
point the X server's internal list of VESA standard modes, plus any modes
specified with special ModeLines in the X configuration file's Monitor
section. These modes are validated against criteria such as the valid
HorizSync/VertRefresh frequency ranges for the user's monitor (as specified in
the Monitor section of the X configuration file), as well as the maximum pixel
clock of the GPU.

Once the X server has determined the set of valid modes, it takes the list of
user requested modes (i.e., the set of modes named in the "Modes" line in the
Display subsection of the Screen section of X configuration file), and finds
the "best" validated mode with the requested name.

The NVIDIA X driver uses a variation on the above approach to perform mode
validation. During X server initialization, the NVIDIA X driver builds a pool
of valid modes for each display device. It gathers all possible modes from
several sources:

   o The display device's EDID

   o The X server's built-in list

   o Any user-specified ModeLines in the X configuration file

   o The VESA standard modes

For every possible mode, the mode is run through mode validation. The core of
mode validation is still performed similarly to traditional XFree86/X.Org mode
validation: the mode timings are checked against things such as the valid
HorizSync and VertRefresh ranges and the maximum pixelclock. Note that each
individual stage of mode validation can be independently controlled through
the "ModeValidation" X configuration option.

Invalid modes are discarded; valid modes are inserted into the mode pool. See
MODE VALIDATION REPORTING for how to get more details on mode validation
results for each considered mode.

Valid modes are given a unique name that is guaranteed to be unique across the
whole mode pool for this display device. This mode name is constructed
approximately like this:

    <width>x<height>_<refreshrate>

(e.g., "1600x1200_85")

The name may also be prepended with another number to ensure the mode is
unique; e.g., "1600x1200_85_0".

As validated modes are inserted into the mode pool, duplicate modes are
removed, and the mode pool is sorted, such that the "best" modes are at the
beginning of the mode pool. The sorting is based roughly on:

   o Resolution

   o Source (EDID-provided modes are prioritized higher than VESA-provided
     modes, which are prioritized higher than modes that were in the X
     server's built-in list)

   o Refresh rate

Once modes from all mode sources are validated and the mode pool is
constructed, all modes with the same resolution are compared; the best mode
with that resolution is added to the mode pool a second time, using just the
resolution as its unique modename (e.g., "1600x1200"). In this way, when you
request a mode using the traditional names (e.g., "1600x1200"), you still get
what you got before (the 'best' 1600x1200 mode); the added benefit is that all
modes in the mode pool can be addressed by a unique name.

When verbose logging is enabled (see the FAQ section on increasing the amount
of data printed in the X log file), the mode pool for each display device is
printed to the X log file.

After the mode pool is built for all display devices, the requested modes (as
specified in the X configuration file), are looked up from the mode pool. Each
requested mode that can be matched against a mode in the mode pool is then
advertised to the X server and is available to the user through the X server's
mode switching hotkeys (ctrl-alt-plus/minus) and the XRandR and XF86VidMode X
extensions.

If only one display device is in use by the X screen when the X server starts,
all modes in the mode pool are implicitly made available to the X server. See
the "IncludeImplicitMetaModes" X configuration option in Appendix D for
details.

THE NVIDIA-AUTO-SELECT MODE

You can request a special mode by name in the X config file, named
"nvidia-auto-select". When the X driver builds the mode pool for a display
device, it selects one of the modes as the "nvidia-auto-select" mode; a new
entry is made in the mode pool, and "nvidia-auto-select" is used as the unique
name for the mode.

The "nvidia-auto-select" mode is intended to be a reasonable mode for the
display device in question. For example, the "nvidia-auto-select" mode is
normally the native resolution for flatpanels, as reported by the flatpanel's
EDID, or one of the detailed timings from the EDID. The "nvidia-auto-select"
mode is guaranteed to always be present, and to always be defined as something
considered valid by the X driver for this display device.

Note that the "nvidia-auto-select" mode is not necessarily the largest
possible resolution, nor is it necessarily the mode with the highest refresh
rate. Rather, the "nvidia-auto-select" mode is selected such that it is a
reasonable default. The selection process is roughly:


   o If the EDID for the display device reported a preferred mode timing, and
     that mode timing is considered a valid mode, then that mode is used as
     the "nvidia-auto-select" mode. You can check if the EDID reported a
     preferred timing by starting X with logverbosity greater than or equal to
     5 (see the FAQ section on increasing the amount of data printed in the X
     log file), and looking at the EDID printout; if the EDID contains a line:
     
         Prefer first detailed timing : Yes
     
     Then the first mode listed under the "Detailed Timings" in the EDID will
     be used.

   o If the EDID did not provide a preferred timing, the best detailed timing
     from the EDID is used as the "nvidia-auto-select" mode.

   o If the EDID did not provide any detailed timings (or there was no EDID at
     all), the best valid mode not larger than 1024x768 is used as the
     "nvidia-auto-select" mode. The 1024x768 limit is imposed here to restrict
     use of modes that may have been validated, but may be too large to be
     considered a reasonable default, such as 2048x1536.

   o If all else fails, the X driver will use a built-in 800 x 600 60Hz mode
     as the "nvidia-auto-select" mode.


If no modes are requested in the X configuration file, or none of the
requested modes can be found in the mode pool, then the X driver falls back to
the "nvidia-auto-select" mode, so that X can always start. Appropriate warning
messages will be printed to the X log file in these fallback scenarios.

You can add the "nvidia-auto-select" mode to your X configuration file by
running the command

    nvidia-xconfig --mode nvidia-auto-select

and restarting your X server.

The X driver can generally do a much better job of selecting the
"nvidia-auto-select" mode if the display device's EDID is available. This is
one reason why the "IgnoreEDID" X configuration option has been deprecated,
and that it is recommended to only use the "UseEDID" X configuration option
sparingly. Note that, rather than globally disable all uses of the EDID with
the "UseEDID" option, you can individually disable each particular use of the
EDID using the "UseEDIDFreqs", "UseEDIDDpi", and/or the "NoEDIDModes" argument
in the "ModeValidation" X configuration option.

MODE VALIDATION REPORTING

When log verbosity is set to 6 or higher (see FAQ
section on increasing the amount of data printed in the X log file), the X log
will record every mode that is considered for each display device's mode pool,
and report whether the mode passed or failed. For modes that were considered
invalid, the log will report why the mode was considered invalid.

ENSURING IDENTICAL MODE TIMINGS

Some functionality, such as Active Stereo with TwinView, requires control over
exactly which mode timings are used. For explicit control over which mode
timings are used on each display device, you can specify the ModeLine you want
to use (using one of the ModeLine generators available), and using a unique
name. For example, if you wanted to use 1024x768 at 120 Hz on each monitor in
TwinView with active stereo, you might add something like this to the monitor
section of your X configuration file:

    # 1024x768 @ 120.00 Hz (GTF) hsync: 98.76 kHz; pclk: 139.05 MHz
    Modeline "1024x768_120"  139.05  1024 1104 1216 1408  768 769 772 823
-HSync +Vsync

Then, in the Screen section of your X config file, specify a MetaMode like
this:

    Option "MetaModes" "1024x768_120, 1024x768_120"


ADDITIONAL INFORMATION

An XFree86 ModeLine generator, conforming to the GTF Standard is available at
http://gtf.sourceforge.net/. Additional generators can be found by searching
for "modeline" on freshmeat.net.

______________________________________________________________________________

Appendix K. Flipping and UBB
______________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set supports Unified Back Buffer (UBB) and
OpenGL Flipping. These features can provide performance gains in certain
situations.

   o Unified Back Buffer (UBB): UBB is available only on the Quadro family of
     GPUs (Quadro4 NVS excluded) and is enabled by default when there is
     sufficient video memory available. This can be disabled with the UBB X
     config option described in Appendix D. When UBB is enabled, all windows
     share the same back, stencil and depth buffers. When there are many
     windows, the back, stencil and depth usage will never exceed the size of
     that used by a full screen window. However, even for a single small
     window, the back, stencil, and depth video memory usage is that of a full
     screen window. In that case video memory may be used less efficiently
     than in the non-UBB case.

   o Flipping: When OpenGL flipping is enabled, OpenGL can perform buffer
     swaps by changing which buffer the DAC scans out rather than copying the
     back buffer contents to the front buffer; this is generally a much higher
     performance mechanism and allows tearless swapping during the vertical
     retrace (when __GL_SYNC_TO_VBLANK is set). The conditions under which
     OpenGL can flip are slightly complicated, but in general: on GeForce or
     newer hardware, OpenGL can flip when a single full screen unobscured
     OpenGL application is running, and __GL_SYNC_TO_VBLANK is enabled.
     Additionally, OpenGL can flip on Quadro hardware even when an OpenGL
     window is partially obscured or not full screen or __GL_SYNC_TO_VBLANK is
     not enabled.


______________________________________________________________________________

Appendix L. Known Issues
______________________________________________________________________________

The following problems still exist in this release and are in the process of
being resolved.

Known Issues

OpenGL and dlopen()

    There are some issues with older versions of the glibc dynamic loader
    (e.g., the version that shipped with Red Hat Linux 7.2) and applications
    such as Quake3 and Radiant, that use dlopen(). Please see Chapter 4 for
    more details.

Multicard, Multimonitor

    In some cases, the secondary card is not initialized correctly by the
    NVIDIA kernel module. You can work around this by enabling the XFree86
    Int10 module to soft-boot all secondary cards. See Appendix D for details.

Interaction with pthreads

    Single-threaded applications that use dlopen() to load NVIDIA's libGL
    library, and then use dlopen() to load any other library that is linked
    against libpthread will crash in libGL. This does not happen in NVIDIA's
    new ELF TLS OpenGL libraries (please see Appendix C for a description of
    the ELF TLS OpenGL libraries). Possible workarounds for this problem are:
    
      1. Load the library that is linked with libpthread before loading libGL.
    
      2. Link the application with libpthread.
    
    
The X86-64 platform (AMD64/EM64T) and 2.6 kernels

    Many 2.4 and 2.6 x86_64 kernels have an accounting problem in their
    implementation of the change_page_attr kernel interface. Early 2.6 kernels
    include a check that triggers a BUG() when this situation is encountered
    (triggering a BUG() results in the current application being killed by the
    kernel; this application would be your OpenGL application or potentially
    the X server). The accounting issue has been resolved in the 2.6.11
    kernel.

    We have added checks to recognize that the NVIDIA kernel module is being
    compiled for the x86-64 platform on a kernel between 2.6.0 and 2.6.11. In
    this case, we will disable usage of the change_page_attr kernel interface.
    This will avoid the accounting issue but leaves the system in danger of
    cache aliasing (see entry below on Cache Aliasing for more information
    about cache aliasing). Note that this change_page_attr accounting issue
    and BUG() can be triggered by other kernel subsystems that rely on this
    interface.

    If you are using a 2.6 x86_64 kernel, it is recommended that you upgrade
    to a 2.6.11 or later kernel.

    Also take note of common dma issues on 64-bit platforms in Appendix AA.

Cache Aliasing

    Cache aliasing occurs when multiple mappings to a physical page of memory
    have conflicting caching states, such as cached and uncached. Due to these
    conflicting states, data in that physical page may become corrupted when
    the processor's cache is flushed. If that page is being used for DMA by a
    driver such as NVIDIA's graphics driver, this can lead to hardware
    stability problems and system lockups.

    NVIDIA has encountered bugs with some Linux kernel versions that lead to
    cache aliasing. Although some systems will run perfectly fine when cache
    aliasing occurs, other systems will experience severe stability problems,
    including random lockups. Users experiencing stability problems due to
    cache aliasing will benefit from updating to a kernel that does not cause
    cache aliasing to occur.

    NVIDIA has added driver logic to detect cache aliasing and to print a
    warning with a message similar to the following:
    
    NVRM: bad caching on address 0x1cdf000: actual 0x46 != expected 0x73
    
    If you see this message in your log files and are experiencing stability
    problems, you should update your kernel to the latest version.

    If the message persists after updating your kernel, please send a bug
    report to NVIDIA.

64-Bit BARs (Base Address Registers)

    Starting with native PCI Express GPUs, NVIDIA's GPUs will advertise a
    64-bit BAR capability (a Base Address Register stores the location of a
    PCI I/O region, such as registers or a frame buffer). This means that the
    GPU's PCI I/O regions (registers and frame buffer) can be placed above the
    32-bit address space (the first 4 gigabytes of memory).

    The decision of where the BAR is placed is made by the system BIOS at boot
    time. If the BIOS supports 64-bit BARs, then the NVIDIA PCI I/O regions
    may be placed above the 32-bit address space. If the BIOS does not support
    this feature, then our PCI I/O regions will be placed within the 32-bit
    address space as they have always been.

    Unfortunately, current Linux kernels (as of 2.6.11.x) do not understand or
    support 64-bit BARs. If the BIOS does place any NVIDIA PCI I/O regions
    above the 32-bit address space, the kernel will reject the BAR and the
    NVIDIA driver will not work.

    There is no known workaround at this point.

Kernel virtual address space exhaustion on the X86 platform

    On X86 systems and AMD64/EM64T systems using X86 kernels, only 4GB of
    virtual address space are available, which the Linux kernel typically
    partitions such that user processes are allocated 3GB, the kernel itself
    1GB. Part of the kernel's share is used to create a direct mapping of
    system memory (RAM). Depending on how much system memory is installed, the
    kernel virtual address space remaining for other uses varies in size and
    may be as small as 128MB, if 1GB of system memory (or more) are installed.
    By default, the kernel reserves a minimum of 128MB.

    The kernel virtual address space still available after the creation of the
    direct system memory mapping is used by both the kernel and by drivers to
    map I/O resources, and for some memory allocations. Depending on the
    number of consumers and their respective requirements, the Linux kernel's
    virtual address space may be exhausted. Newer Linux kernels print an error
    message of the form below when this happens:
    
    allocation failed: out of vmalloc space - use vmalloc=<size> to increase
    size.
    
    
    The NVIDIA kernel module requires portions of the kernel's virtual address
    space for each GPU and for certain memory allocations. If no more than
    128MB are available to the kernel and device drivers at boot time, the
    NVIDIA kernel module may be unable to initialize all GPUs, or fail memory
    allocations. This is not usually a problem with only 1 or 2 GPUs, however
    depending on the number of other drivers and their usage patterns, it can
    be; it is likely to be a problem with 3 or more GPUs.

    Possible solutions for this problem include:
    
       o If available, the 'vmalloc' kernel parameter can be used to increase
         the size of the kernel virtual address space reserved by the Linux
         kernel (the default is 128MB). It is recommended to raise this value
         in increments to find the best balance between the size of the kernel
         virtual address space made available and the size of the direct
         system memory mapping. You can achieve this by passing
         'vmalloc=192M', 'vmalloc=256MB', ..., to the kernel and checking if
         the above error message continues to be printed.
    
         Note that some versions of the GRUB boot loader have problems
         calculating the memory layout and loading the initrd if the 'vmalloc'
         kernel parameter is used. The 'uppermem' GRUB command can be used to
         force GRUB to load the initrd into a lower region of system memory to
         work around this problem. This will not adversely affect system
         performance once the kernel has been loaded. The suggested syntax is:
         
         title     Kernel Title
         uppermem  524288
         kernel    (hdX,Y)/boot/vmlinuz...
         
         
         Please also note that the 'vmalloc' kernel parameter only exists on
         Linux 2.6.9 and later kernels. On older kernels, the amount of system
         memory used by the kernel can be reduced with the 'mem' kernel
         parameter, which also reduces the size of the direct mapping and thus
         increases the size of the kernel virtual address space available. For
         example, 'mem=512M' instructs the kernel to ignore all but the first
         512MB of system memory. Although it is undesirable to reduce the
         amount of usable system memory, this approach can be used to check if
         initialization problems are caused by kernel virtual address space
         exhaustion.
    
       o In some cases, disabling frame buffer drivers such as vesafb can
         help, as such drivers may attempt to map all or a large part of the
         installed graphics cards' video memory into the kernel's virtual
         address space, which rapidly consumes this resource. You can disable
         the vesafb frame buffer driver by passing these parameters to the
         kernel: 'video=vesa:off vga=normal'.
    
       o Some Linux kernels can be configured with alternate address space
         layouts (e.g. 2.8GB:1.2GB, 2GB:2GB, etc.). This option can be used to
         avoid exhaustion of the kernel virtual address space without reducing
         the size of the direct system memory mapping. Some Linux distributors
         also provide kernels that use seperate 4GB address spaces for user
         processes and the kernel. Such Linux kernels provide sufficient
         kernel virtual address space on typical systems.
    
       o If your system is equipped with an X86-64 (AMD64/EM64T) processor, it
         is recommended that you switch to a 64-bit Linux kernel/distribution.
         Due to the significantly larger address space provided by the X86-64
         processors' addressing capabilities, X86-64 kernels will not run out
         of kernel virtual address space in the foreseeable future.
    
    
Valgrind

    The NVIDIA OpenGL implementation makes use of self modifying code. To
    force Valgrind to retranslate this code after a modification you must run
    using the Valgrind command line option:
    
    --smc-check=all
    
    Without this option Valgrind may execute incorrect code causing incorrect
    behavior and reports of the form:
    
    ==30313== Invalid write of size 4
    
    
MMConfig-based PCI Configuration Space Accesses

    2.6 kernels have added support for Memory-Mapped PCI Configuration Space
    accesses. Unfortunately, there are many problems with this mechanism, and
    the latest kernel updates are more careful about enabling this support.

    The NVIDIA driver may be unable to reliably read/write the PCI
    Configuration Space of NVIDIA devices when the kernel is using the
    MMCONFIG method to access PCI Configuration Space, specifically when using
    multiple GPUs and multiple CPUs on 32-bit kernels.

    This access method can be identified by the presence of the string "PCI:
    Using MMCONFIG" in the 'dmesg' output on your system. This access method
    can be disabled via the "pci=nommconf" kernel parameter.

Laptops

    If you are using a laptop please see the "Known Laptop Issues" in Appendix
    I.

FSAA

    When FSAA is enabled (the __GL_FSAA_MODE environment variable is set to a
    value that enables FSAA and a multisample visual is chosen), the rendering
    may be corrupted when resizing the window.

libGL DSO finalizer and pthreads

    When a multithreaded OpenGL application exits, it is possible for libGL's
    DSO finalizer (also known as the destructor, or "_fini") to be called
    while other threads are executing OpenGL code. The finalizer needs to free
    resources allocated by libGL. This can cause problems for threads that are
    still using these resources. Setting the environment variable
    "__GL_NO_DSO_FINALIZER" to "1" will work around this problem by forcing
    libGL's finalizer to leave its resources in place. These resources will
    still be reclaimed by the operating system when the process exits. Note
    that the finalizer is also executed as part of dlclose(3), so if you have
    an application that dlopens(3) and dlcloses(3) libGL repeatedly,
    "__GL_NO_DSO_FINALIZER" will cause libGL to leak resources until the
    process exits. Using this option can improve stability in some
    multithreaded applications, including Java3D applications.

XVideo and the Composite X extension

    XVideo will not work correctly when Composite is enabled unless using
    X.Org 7.1 or later. See Appendix S.

This section describes problems that will not be fixed. Usually, the source of
the problem is beyond the control of NVIDIA. Following is the list of
problems:

Problems that Will Not Be Fixed

Gigabyte GA-6BX Motherboard

    This motherboard uses a LinFinity regulator on the 3.3 V rail that is only
    rated to 5 A -- less than the AGP specification, which requires 6 A. When
    diagnostics or applications are running, the temperature of the regulator
    rises, causing the voltage to the NVIDIA chip to drop as low as 2.2 V.
    Under these circumstances, the regulator cannot supply the current on the
    3.3 V rail that the NVIDIA chip requires.

    This problem does not occur when the graphics board has a switching
    regulator or when an external power supply is connected to the 3.3 V rail.

VIA KX133 and 694X Chip sets with AGP 2x

    On Athlon motherboards with the VIA KX133 or 694X chip set, such as the
    ASUS K7V motherboard, NVIDIA drivers default to AGP 2x mode to work around
    insufficient drive strength on one of the signals.

Irongate Chip sets with AGP 1x

    AGP 1x transfers are used on Athlon motherboards with the Irongate chipset
    to work around a problem with signal integrity.

ALi chipsets, ALi1541 and ALi1647

    On ALi1541 and ALi1647 chipsets, NVIDIA drivers disable AGP to work around
    timing issues and signal integrity issues. See Chapter 5 for more
    information on ALi chipsets.

NV-CONTROL versions 1.8 and 1.9

    Version 1.8 of the NV-CONTROL X Extension introduced target types for
    setting and querying attributes as well as receiving event notification on
    targets. Targets are objects like X Screens, GPUs and G-Sync devices.
    Previously, all attributes were described relative to an X Screen. These
    new bits of information (target type and target id) were packed in a
    non-compatible way in the protocol stream such that addressing X Screen 1
    or higher would generate an X protocol error when mixing NV-CONTROL client
    and server versions.

    This packing problem has been fixed in the NV-CONTROL 1.10 protocol,
    making it possible for the older (1.7 and prior) clients to communicate
    with NV-CONTROL 1.10 servers. Furthermore, the NV-CONTROL 1.10 client
    library has been updated to accommodate the target protocol packing bug
    when communicating with a 1.8 or 1.9 NV-CONTROL server. This means that
    the NV-CONTROL 1.10 client library should be able to communicate with any
    version of the NV-CONTROL server.

    It is recommended that NV-CONTROL client applications relink with version
    1.10 or later of the NV-CONTROL client library (libXNVCtrl.a, in the
    nvidia-settings-1.0.tar.gz tarball). The version of the client library can
    be determined by checking the NV_CONTROL_MAJOR and NV_CONTROL_MINOR
    definitions in the accompanying nv_control.h.

    The only web released NVIDIA Linux driver that is affected by this problem
    (i.e., the only driver to use either version 1.8 or 1.9 of the NV-CONTROL
    X extension) is 1.0-8756.

I/O APIC (SMP)

    If you are experiencing stability problems with a Linux SMP machine and
    seeing I/O APIC warning messages from the Linux kernel, system reliability
    may be greatly improved by setting the "noapic" kernel parameter.

Local APIC (UP)

    On some systems, setting the "Local APIC Support on Uniprocessors" kernel
    configuration option can have adverse effects on system stability and
    performance. If you are experiencing lockups with a Linux UP machine and
    have this option set, try disabling local APIC support.

nForce2 Chipsets and AGPGART

    Some of the earlier versions of agpgart to support the nForce2 chipset are
    known to contain bugs that result in system hangs. The suggested
    workaround is to use NVAGP or update to a newer kernel. Known problematic
    versions include all known Red Hat Enterprise Linux 3 kernels (through
    Update 7).

    If a broken agpgart is used on an nForce2 chipset, the NVIDIA driver will
    attempt to work around these agpgart bugs as best it can, by recovering
    from AGP errors and eventually disabling AGP.

    To configure NVAGP, please see Appendix F.


______________________________________________________________________________

Appendix M. Proc Interface
______________________________________________________________________________

You can use the /proc filesystem interface to obtain run-time information
about the driver, any installed NVIDIA graphics cards, and the AGP status.

This information is contained in several files in /proc/driver/nvidia

/proc/driver/nvidia/version

    Lists the installed driver revision and the version of the GNU C compiler
    used to build the Linux kernel module.

/proc/driver/nvidia/warnings

    The NVIDIA graphics driver tries to detect potential problems with the
    host system's kernel and warns about them using the kernel's printk()
    mechanism, typically logged by the system to '/var/log/messages'.

    Important NVIDIA warning messages are also logged to dedicated text files
    in this /proc directory.

/proc/driver/nvidia/cards/0...3

    Provide information about each of the installed NVIDIA graphics adapters
    (model name, IRQ, BIOS version, Bus Type). Please note that the BIOS
    version is only available while X is running.

/proc/driver/nvidia/agp/card

    Information about the installed AGP card's AGP capabilities.

/proc/driver/nvidia/agp/host-bridge

    Information about the host bridge (model and AGP capabilities).

/proc/driver/nvidia/agp/status

    The current AGP status. If AGP support has been enabled on your system,
    the AGP driver being used, the AGP rate, and information about the status
    of AGP Fast Writes and Side Band Addressing is shown.

    The AGP driver is either NVIDIA (NVIDIA's built-in AGP driver) or AGPGART
    (the Linux kernel's agpgart.o driver). If you see "inactive" next to
    AGPGART, then this means that the AGP chipset was programmed by AGPGART,
    but is not currently in use.

    SBA and Fast Writes indicate whether either one of these features is
    currently in use. Please note that several factors determine whether
    support for either will be enabled. Even if both the AGP card and the host
    bridge support them, the driver may decide not to use these features in
    favor of system stability. This is particularly true of AGP Fast Writes.


______________________________________________________________________________

Appendix N. XvMC Support
______________________________________________________________________________

This release includes support for the XVideo Motion Compensation (XvMC)
version 1.0 API on GeForce4, GeForce FX and newer products. There is a static
library "libXvMCNVIDIA.a" and a dynamic one "libXvMCNVIDIA_dynamic.so" which
is suitable for dlopening. GeForce4 MX, GeForce FX and newer products support
both XvMC's "IDCT" and "motion-compensation" levels of acceleration. GeForce4
Ti products only support the motion-compensation level. AI44 and IA44
subpictures are supported. 4:2:0 Surfaces up to 2032x2032 are supported.

libXvMCNVIDIA observes the XVMC_DEBUG environment variable and will provide
some debug output to stderr when set to an appropriate integer value. '0'
disables debug output. '1' enables debug output for failure conditions. '2' or
higher enables output of warning messages.

______________________________________________________________________________

Appendix O. GLX Support
______________________________________________________________________________

This release supports GLX 1.4. Additionally, the following GLX extensions are
supported on appropriate GPUs:

   o GLX_EXT_visual_info

   o GLX_EXT_visual_rating

   o GLX_SGIX_fbconfig

   o GLX_SGIX_pbuffer

   o GLX_ARB_get_proc_address

   o GLX_SGI_video_sync

   o GLX_SGI_swap_control

   o GLX_ARB_multisample

   o GLX_NV_float_buffer

   o GLX_ARB_fbconfig_float

   o GLX_NV_swap_group

   o GLX_NV_video_out

   o GLX_EXT_texture_from_pixmap

For a description of these extensions, please see the OpenGL extension
registry at http://www.opengl.org/registry/

Some of the above extensions exist as part of core GLX 1.4 functionality,
however, they are also exported as extensions for backwards compatibility.

______________________________________________________________________________

Appendix P. Configuring Multiple X Screens on One Card
______________________________________________________________________________

Graphics chips that support TwinView (Appendix G) can also be configured to
treat each connected display device as a separate X screen.

While there are several disadvantages to this approach as compared to TwinView
(e.g.: windows cannot be dragged between X screens, hardware accelerated
OpenGL cannot span the two X screens), it does offer several advantages over
TwinView:

   o If each display device is a separate X screen, then properties that may
     vary between X screens may vary between displays (e.g.: depth, root
     window size, etc).

   o Hardware that can only be used on one display at a time (e.g.: video
     overlays, hardware accelerated RGB overlays), and which consequently
     cannot be used at all when in TwinView, can be exposed on the first X
     screen when each display is a separate X screen.

   o TwinView is a fairly new feature. X has historically used one screen per
     display device.


To configure two separate X screens to share one graphics chip, here is what
you will need to do:

First, create two separate Device sections, each listing the BusID of the
graphics card to be shared and listing the driver as "nvidia", and assign each
a separate screen:

    Section "Device"
        Identifier  "nvidia0"
        Driver      "nvidia"
        # Edit the BusID with the location of your graphics card
        BusID       "PCI:2:0:0"
        Screen      0
    EndSection

    Section "Device"
        Identifier  "nvidia1"
        Driver      "nvidia"
        # Edit the BusID with the location of your graphics card
        BusId       "PCI:2:0:0"
        Screen      1
    EndSection

Then, create two Screen sections, each using one of the Device sections:

    Section "Screen"
        Identifier  "Screen0"
        Device      "nvidia0"
        Monitor     "Monitor0"
        DefaultDepth 24
        Subsection "Display"
            Depth       24
            Modes       "1600x1200" "1024x768" "800x600" "640x480" 
        EndSubsection
    EndSection

    Section "Screen"
        Identifier  "Screen1"
        Device      "nvidia1"
        Monitor     "Monitor1"
        DefaultDepth 24
        Subsection "Display"
            Depth       24
            Modes       "1600x1200" "1024x768" "800x600" "640x480" 
        EndSubsection
    EndSection

(Note: You'll also need to create a second Monitor section) Finally, update
the ServerLayout section to use and position both Screen sections:

    Section "ServerLayout"
        ...
        Screen         0 "Screen0" 
        Screen         1 "Screen1" leftOf "Screen0"
        ...
    EndSection

For further details, please refer to the XF86Config(5x) or xorg.conf(5x)
manpages.

______________________________________________________________________________

Appendix Q. Power Management Support
______________________________________________________________________________

The NVIDIA driver includes support for both APM- and ACPI- based power
management. This driver will support APM-based suspend and resume, as well as
ACPI standby (S3) and suspend (S4).

To use APM, your system's BIOS will need to support APM, rather than ACPI.
Many, but not all, of the GeForce2- and GeForce4-based laptops include APM
support. You can check for APM support via the procfs interface (check for the
existence of /proc/apm) or via the kernel's boot output:

    % dmesg | grep -i apm

a message similar to this indicates APM support:

    apm: BIOS version 1.2 Flags 0x03 (Driver version 1.16)

or a message like this indicates no APM support:

    No APM support in Kernel

Please note: If you are using Linux kernel 2.6 and your kernel was configured
with support for both ACPI and APM, the NVIDIA kernel module will be built
with ACPI Power Management support. If you wish to use APM, you will need to
rebuild the Linux kernel without ACPI support and reinstall the NVIDIA Linux
graphics driver.

Sometimes chipsets lose their AGP configuration during suspend, and may cause
corruption on the bus upon resume. The AGP driver is required to save and
restore relevant register state on such systems; NVIDIA's NvAGP is notified of
power management events and ensures its configuration is kept intact across
suspend/resume cycles.

Linux 2.4 AGPGART does not support power management, Linux 2.6 AGPGART does,
but only for a few select chipsets. If you use either of these two AGP drivers
and find your system fails to resume reliably, you may have more success with
NVIDIA's NvAGP driver.

Disabling AGP support (please see Appendix F for more details on disabling
AGP) will also work around this problem.

More recent systems are more likely to support ACPI. ACPI is supported by the
NVIDIA graphics driver in 2.6 and newer kernels. The driver supports ACPI
standby (S3) and includes beta support for ACPI suspend (S4).

If you enable ACPI S4 support via suspend2 patches, you will need to tweak the
Linux kernel such that it dynamically determines the amount of pages needed by
the drivers that will be suspended in the system. This is done by issuing the
following command from root:

    % echo 0 > /proc/suspend2/extra_pages_allowance

The system does NOT need rebooting, and as a matter of fact, the setting is
volatile over reboots. You will need to include the tweak in your startup
scripts. However, failure to perform the tweak will result in a hang going to
sleep. For further information regarding suspend2 patches, please see
http://www.suspend2.net/.

______________________________________________________________________________

Appendix R. Display Device Names
______________________________________________________________________________

A "Display Device" refers to some piece of hardware capable of displaying an
image. Display devices are separated into three categories: analog CRTs,
digital flat panels (DFPs), and televisions. Note that analog flat panels are
considered the same as analog CRTs by the driver.

A "Display Device Name" is a string description that uniquely identifies a
display device; it follows the format "<type>-<number>", for example: "CRT-0",
"CRT-1", "DFP-0", or "TV-0". Note that the number indicates how the display
device connector is wired on the graphics board, and has nothing to do with
how many of that kind of display device are present. This means, for example,
that you may have a "CRT-1", even if you do not have a "CRT-0". To determine
which display devices are currently connected, you may check your X log file
for a line similar to the following:

    (II) NVIDIA(0): Connected display device(s): CRT-0, DFP-0

Display device names can be used in the MetaMode, HorizSync, and VertRefresh X
config options to indicate which display device a setting should be applied
to. For example:

    Option "MetaModes"   "CRT-0: 1600x1200,  DFP-0: 1024x768"
    Option "HorizSync"   "CRT-0: 50-110;     DFP-0: 40-70"
    Option "VertRefresh" "CRT-0: 60-120;     DFP-0: 60"

Specifying the display device name in these options is not required; if
display device names are not specified, then the driver attempts to infer
which display device a setting applies to. In the case of MetaModes, for
example, the first mode listed is applied to the "first" display device, and
the second mode listed is applied to the "second" display device.
Unfortunately, it is often unclear which display device is the "first" or
"second". That is why specifying the display device name is preferable.

When specifying display device names, you may also omit the number part of the
name, though this is only useful if you only have one of that type of display
device. For example, if you have one CRT and one DFP connected, you may
reference them in the MetaMode string as follows:

    Option "MetaModes"   "CRT: 1600x1200,  DFP: 1024x768"


______________________________________________________________________________

Appendix S. The X Composite Extension
______________________________________________________________________________

X.Org X servers, beginning with X11R6.8.0, contain experimental support for a
new X protocol extension called Composite. This extension allows windows to be
drawn into pixmaps instead of directly onto the screen. In conjunction with
the Damage and Render extensions, this allows a program called a composite
manager to blend windows together to draw the screen.

Performance will be degraded significantly if the "RenderAccel" option is
disabled in xorg.conf. See Appendix D for more details.

When the NVIDIA X driver is used with an X.Org X server X11R6.9.0 or newer and
the Composite extension is enabled, NVIDIA's OpenGL implementation interacts
properly with the Damage and Composite X extensions. This means that OpenGL
rendering is drawn into offscreen pixmaps and the X server is notified of the
Damage event when OpenGL renders to the pixmap. This allows OpenGL
applications to behave properly in a composited X desktop.

If the Composite extension is enabled on an X server older than X11R6.9.0,
then GLX will be disabled. You can force GLX on while Composite is enabled on
pre-X11R6.9.0 X servers with the "AllowGLXWithComposite" X configuration
option. However, GLX will not render correctly in this environment. It is
recommended that you upgrade your X server to X11R6.9.0 or newer.

You can enable the Composite X extension by running 'nvidia-xconfig
--composite'. Composite can be disabled with 'nvidia-xconfig --no-composite'.
See the nvidia-xconfig(1) man page for details.

If you are using Composite with GLX, it is recommended that you also enable
the "DamageEvents" X option for enhanced performance. If you are using an
OpenGL-based composite manager, you may also need the "DisableGLXRootClipping"
option to obtain proper output.

The Composite extension also causes problems with other driver components:

   o In X servers prior to X.Org 7.1, Xv cannot draw into pixmaps that have
     been redirected offscreen and will draw directly onto the screen instead.
     For some programs you can work around this issue by using an alternative
     video driver. For example, "mplayer -vo x11" will work correctly, as will
     "xine -V xshm". If you must use Xv with an older server, you can also
     disable the compositing manager and re-enable it when you are finished.

     On X.Org 7.1 and higher, the driver will properly redirect video into
     offscreen pixmaps. Note that the Xv adaptors will ignore the
     sync-to-vblank option when drawing into a redirected window.

   o Workstation overlays, stereo visuals, and the unified back buffer (UBB)
     are incompatible with Composite. These features will be automatically
     disabled when Composite is detected.


This driver supports OpenGL rendering to 32-bit ARGB windows when the
"AddARGBGLXVisuals" X config file option is enabled. If you are an application
developer, you can use these new visuals in conjunction with a composite
manager to create translucent OpenGL applications:

    int attrib[] = {
        GLX_RENDER_TYPE, GLX_RGBA_BIT,
        GLX_DRAWABLE_TYPE, GLX_WINDOW_BIT,
        GLX_RED_SIZE, 1,
        GLX_GREEN_SIZE, 1,
        GLX_BLUE_SIZE, 1,
        GLX_ALPHA_SIZE, 1,
        GLX_DOUBLEBUFFER, True,
        GLX_DEPTH_SIZE, 1,
        None };
    GLXFBConfig *fbconfigs, fbconfig;
    int numfbconfigs, render_event_base, render_error_base;
    XVisualInfo *visinfo;
    XRenderPictFormat *pictFormat;

    /* Make sure we have the RENDER extension */
    if(!XRenderQueryExtension(dpy, &render_event_base, &render_error_base)) {
        fprintf(stderr, "No RENDER extension found\n");
        exit(EXIT_FAILURE);
    }

    /* Get the list of FBConfigs that match our criteria */
    fbconfigs = glXChooseFBConfig(dpy, scrnum, attrib, &numfbconfigs);
    if (!fbconfigs) {
        /* None matched */
        exit(EXIT_FAILURE);
    }

    /* Find an FBConfig with a visual that has a RENDER picture format that
     * has alpha */
    for (i = 0; i < numfbconfigs; i++) {
        visinfo = glXGetVisualFromFBConfig(dpy, fbconfigs[i]);
        if (!visinfo) continue;
        pictFormat = XRenderFindVisualFormat(dpy, visinfo->visual);
        if (!pictFormat) continue;

        if(pictFormat->direct.alphaMask > 0) {
            fbconfig = fbconfigs[i];
            break;
        }

        XFree(visinfo);
    }

    if (i == numfbconfigs) {
        /* None of the FBConfigs have alpha.  Use a normal (opaque)
         * FBConfig instead */
        fbconfig = fbconfigs[0];
        visinfo = glXGetVisualFromFBConfig(dpy, fbconfig);
        pictFormat = XRenderFindVisualFormat(dpy, visinfo->visual);
    }

    XFree(fbconfigs);


When rendering to a 32-bit window, keep in mind that the X RENDER extension,
used by most composite managers, expects "premultiplied alpha" colors. This
means that if your color has components (r,g,b) and alpha value a, then you
must render (a*r, a*g, a*b, a) into the target window.

More information about Composite can be found at
http://freedesktop.org/Software/CompositeExt

______________________________________________________________________________

Appendix T. The nvidia-settings Utility
______________________________________________________________________________

A graphical configuration utility, 'nvidia-settings', is included with the
NVIDIA Linux graphics driver. After installing the driver and starting X, you
can run this configuration utility by running:

    % nvidia-settings

in a terminal window.

Detailed information about the configuration options available are documented
in the help window in the utility.

For more information, see the nvidia-settings man page.

The source code to nvidia-settings is released as GPL and is available here:
ftp://download.nvidia.com/XFree86/nvidia-settings/

If you have trouble running the nvidia-settings binary shipped with the NVIDIA
Linux Graphics Driver, please refer to the nvidia-settings entry in Chapter 5.

______________________________________________________________________________

Appendix U. The XRandR Extension
______________________________________________________________________________

X.Org version X11R6.8.1 contains support for the rotation component of the
XRandR extension. This allows screens to be rotated at 90 degree increments.

The driver supports rotation with the extension when 'Option "RandRRotation"'
is enabled in the X config file.

Workstation RGB or CI overlay visuals will function at lower performance and
the video overlay will not be available when RandRRotation is enabled.

You can query the available rotations using the 'xrandr' command line
interface to the RandR extension by running:

    xrandr -q

You can set the rotation orientation of the screen by running any of:

    xrandr -o left
    xrandr -o right
    xrandr -o inverted
    xrandr -o normal

Rotation may also be set through the nvidia-settings configuration utility in
the "Rotation Settings" panel.

TwinView and rotation can be used together, but rotation affects the entire
desktop. This means that the same rotation setting will apply to both display
devices in a TwinView pair. Note also that the "TwinViewOrientation" option
applies before rotation does. For example, if you have two screens
side-by-side and you want to rotate them, you should set "TwinViewOrientation"
to "Above" or "Below".

______________________________________________________________________________

Appendix V. Support for GLX in Xinerama
______________________________________________________________________________

This driver supports GLX when Xinerama is enabled on similar GPUs. The
Xinerama extension takes multiple physical X screens (possibly spanning
multiple GPUs), and binds them into one logical X screen. This allows windows
to be dragged between GPUs and to span across multiple GPUs. The NVIDIA driver
supports hardware accelerated OpenGL rendering across all NVIDIA GPUs when
Xinerama is enabled.

To configure Xinerama: configure multiple X screens (please refer to the
XF86Config(5x) or xorg.conf(5x) manpages for details). The Xinerama extension
can be enabled by adding the line

    Option "Xinerama" "True"

to the "ServerFlags" section of your X config file.

Requirements:

   o It is recommended to use identical GPUs. Some combinations of
     non-identical, but similar, GPUs are supported. If a GPU is incompatible
     with the rest of a Xinerama desktop then no OpenGL rendering will appear
     on the screens driven by that GPU. Rendering will still appear normally
     on screens connected to other supported GPUs. In this situation the X log
     file will include a message of the form:



(WW) NVIDIA(2): The GPU driving screen 2 is incompatible with the rest of
(WW) NVIDIA(2):      the GPUs composing the desktop.  OpenGL rendering will
(WW) NVIDIA(2):      be disabled on screen 2.



   o The NVIDIA X driver must be used for all X screens in the server.

   o Only the intersection of capabilities across all GPUs will be advertised.

   o X configuration options that affect GLX operation (e.g.: stereo,
     overlays) should be set consistently across all X screens in the X
     server.


Known Issues:

   o Versions of XFree86 prior to 4.5 and versions of X.Org prior to 6.8.0
     lack the required interfaces to properly implement overlays with the
     Xinerama extension. On earlier server versions mixing overlays and
     Xinerama will result in rendering corruption. If you are using the
     Xinerama extension with overlays, it is recommended that you upgrade to
     XFree86 4.5, X.Org 6.8.0, or newer.


______________________________________________________________________________

Appendix W. SLI and MultiGPU FrameRendering
______________________________________________________________________________

This driver contains support for NVIDIA SLI FrameRendering and NVIDIA MultiGPU
FrameRendering. Both of these technologies allow an OpenGL application to take
advantage of multiple GPUs to improve visual performance.

The distinction between SLI and MultiGPU is straightforward. SLI is used to
leverage the processing power of GPUs across two or more graphics cards, while
MultiGPU is used to leverage the processing power of two GPUs colocated on the
same graphics card. If you want to link together separate graphics cards, you
should use the "SLI" X config option. Likewise, if you want to link together
GPUs on the same graphics card, you should use the "MultiGPU" X config option.
If you have two cards, each with two GPUs, and you wish to link them all
together, you should use the "SLI" option.

In Linux, with two GPUs SLI and MultiGPU can both operate in one of three
modes: Alternate Frame Rendering (AFR), Split Frame Rendering (SFR), and
Antialiasing (AA). When AFR mode is active, one GPU draws the next frame while
the other one works on the frame after that. In SFR mode, each frame is split
horizontally into two pieces, with one GPU rendering each piece. The split
line is adjusted to balance the load between the two GPUs. AA mode splits
antialiasing work between the two GPUs. Both GPUs work on the same scene and
the result is blended together to produce the final frame. This mode is useful
for applications that spend most of their time processing with the CPU and
cannot benefit from AFR.

With four GPUs, the same options are applicable. AFR mode cycles through all
four GPUs, each GPU rendering a frame in turn. SFR mode splits the frame
horizontally into four pieces. AA mode splits the work between the four GPUs,
allowing antialiasing up to 64x. With four GPUs SLI can also operate in an
additional mode, Alternate Frame Rendering of Antialiasing. (AFR of AA). With
AFR of AA, pairs of GPUs render alternate frames, each GPU in a pair doing
half of the antialiasing work. Note that these scenarios apply whether you
have four separate cards or you have two cards, each with two GPUs.

MultiGPU is enabled by setting the "MultiGPU" option in the X configuration
file; see Appendix D for more details about the MultiGPU option.

The nvidia-xconfig utility can be used to set the MultiGPU option, rather than
modifying the X configuration file by hand. For example:

    % nvidia-xconfig --multigpu=on


SLI is enabled by setting the "SLI" option in the X configuration file; see
Appendix D for more details about the SLI option.

The nvidia-xconfig utility can be used to set the SLI option, rather than
modifying the X configuration file by hand. For example:

    % nvidia-xconfig --sli=on


SLI requires identical PCI-Express graphics cards, a supported motherboard
chipset, and in most cases a "video bridge" connecting the graphics cards.
Note that no mobile GPUs are supported, and SLI on Quadro always requires a
video bridge.

For the latest in supported SLI and MultiGPU configurations, including SLI-
and Multi-GPU capable GPUs and SLI-capable motherboards, please see
http://www.slizone.com.

Only one display can be used when SLI or MultiGPU is enabled. If X is
configured to use multiple screens and screen 0 has SLI or MultiGPU enabled,
the other screens will be disabled. TwinView is also not supported with SLI or
MultiGPU. Please note that if SLI or MultiGPU is enabled, the GPUs used by
that configuration are unavailable for single GPU rendering.


FREQUENTLY ASKED SLI AND MULTIGPU QUESTIONS

Q. Why is glxgears slower when SLI or MultiGPU is enabled?

A. When SLI or MultiGPU is enabled, the NVIDIA driver must coordinate the
   operations of all GPUs when each new frame is swapped (made visible). For
   most applications, this GPU synchronization overhead is negligible.
   However, because glxgears renders so many frames per second, the GPU
   synchronization overhead consumes a significant portion of the total time,
   and the framerate is reduced.


Q. Why is Doom 3 slower when SLI or MultiGPU is enabled?

A. The NVIDIA Accelerated Linux Driver Set does not automatically detect the
   optimal SLI or MultiGPU settings for games such as Doom 3 and Quake 4. To
   work around this issue, the environment variable __GL_DOOM3 can be set to
   tell OpenGL that Doom 3's optimal settings should be used. In Bash, this
   can be done in the same command that launches Doom 3 so the environment
   variable does not remain set for other OpenGL applications started in the
   same session:
   
       % __GL_DOOM3=1 doom3
   
   Doom 3's startup script can also be modified to set this environment
   variable:
   
       #!/bin/sh
       # Needed to make symlinks/shortcuts work.
       # the binaries must run with correct working directory
       cd "/usr/local/games/doom3/"
       export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:.
       export __GL_DOOM3=1
       exec ./doom.x86 "$@"
   
   This environment variable is temporary and will be removed in the future.


Q. Why does SLI or MultiGPU fail to initialize?

A. There are several reasons why SLI or MultiGPU may fail to initialize. Most
   of these should be clear from the warning message in the X log file; e.g.:
   
      o "Unsupported bus type"
   
      o "The video link was not detected"
   
      o "GPUs do not match"
   
      o "Unsupported GPU video BIOS"
   
      o "Insufficient PCI-E link width"
   
   The warning message "'Unsupported PCI topology'" is likely due to problems
   with your Linux kernel. The NVIDIA driver must have access to the PCI
   Bridge (often called the Root Bridge) that each NVIDIA GPU is connected to
   in order to configure SLI or MultiGPU correctly. There are many kernels
   that do not properly recognize this bridge and, as a result, do not allow
   the NVIDIA driver to access this bridge. Please see the below "How can I
   determine if my kernel correctly detects my PCI Bridge?" FAQ for details.

   Below are some specific troubleshooting steps to help deal with SLI and
   MultiGPU initialization failures.
   
      o Make sure that ACPI is enabled in your kernel. NVIDIA's experience
        has been that ACPI is needed for the kernel to correctly recognize
        the Root Bridge. Note that in some cases, the kernel's version of
        ACPI may still have problems and require an update to a newer kernel.
   
      o Run 'lspci' to check that multiple NVIDIA GPUs can be identified by
        the operating system; e.g:
        
            % /sbin/lspci | grep -i nvidia
        
        If 'lspci' does not report all the GPUs that are in your system, then
        this is a problem with your Linux kernel, and it is recommended that
        you use a different kernel.
   
      o Make sure you have the most recent SBIOS available for your
        motherboard.
   
      o The PCI-Express slots on the motherboard must provide a minimum link
        width. Please make sure that the PCI Express slot(s) on your
        motherboard meet the following requirements and that you have
        connected the graphics board to the correct PCI Express slot(s):
        
           o A dual-GPU board needs a minimum of 8 lanes (i.e. x8 or x16)
        
           o A pair of single-GPU boards requires one of the following
             supported link width combinations:
             
                o x16 + x16
             
                o x16 + x8
             
                o x16 + x4
             
                o x8 + x8
             
             
        
   

Q. How can I determine if my kernel correctly detects my PCI Bridge?

A. As discussed above, the NVIDIA driver must have access to the PCI Bridge
   that each NVIDIA GPU is connected to in order to configure SLI or MultiGPU
   correctly. The following steps will identify whether the kernel correctly
   recognizes the PCI Bridge:
   
      o Identify both NVIDIA GPUs:
        
            % /sbin/lspci | grep -i vga
        
            0a:00.0 VGA compatible controller: nVidia Corporation [...]
            81:00.0 VGA compatible controller: nVidia Corporation [...]
        
        
      o Verify that each GPU is connected to a bus connected to the Root
        Bridge (note that the GPUs in the above example are on buses 0a and
        81):
        
            % /sbin/lspci -t
        
        good:
        
            -+-[0000:80]-+-00.0
             |           +-01.0
             |           \-0e.0-[0000:81]----00.0
            ...
             \-[0000:00]-+-00.0
                         +-01.0
                         +-01.1
                         +-0e.0-[0000:0a]----00.0
        
        bad:
        
            -+-[0000:81]---00.0
            ...
             \-[0000:00]-+-00.0
                         +-01.0
                         +-01.1
                         +-0e.0-[0000:0a]----00.0
        
        Note that in the first example, bus 81 is connected to Root Bridge
        80, but that in the second example there is no Root Bridge 80 and bus
        81 is incorrectly connected at the base of the device tree. In the
        bad case, the only solution is to upgrade your kernel to one that
        properly detects your PCI bus layout.
   
   

______________________________________________________________________________

Appendix X. Frame Lock and Genlock
______________________________________________________________________________

Visual computing applications that involve multiple displays, or even multiple
windows within a display, can require special signal processing and
application controls in order to function properly. For example, in order to
produce quality video recording of animated graphics, the graphics display
must be synchronized with the video camera. As another example, applications
presented on multiple displays must be synchronized in order to complete the
illusion of a larger, virtual canvas.

This synchronization is enabled through the frame lock and genlock
capabilities of the NVIDIA driver. This section describes the setup and use of
frame lock and genlock.

DEFINITION OF TERMS

GENLOCK: Genlock refers to the process of synchronizing the pixel scanning of
one or more displays to an external synchronization source. NVIDIA Genlock
requires the external signal to be either TTL or composite, such as used for
NTSC, PAL, or HDTV. It should be noted that the NVIDIA Genlock implementation
is guaranteed only to be frame-synchronized, and not necessarily
pixel-synchronized.

FRAME LOCK: Frame Lock involves the use of hardware to synchronize the frames
on each display in a connected system. When graphics and video are displayed
across multiple monitors, frame locked systems help maintain image continuity
to create a virtual canvas. Frame lock is especially critical for stereo
viewing, where the left and right fields must be in sync across all displays.

In short, to enable genlock means to sync to an external signal. To enable
frame lock means to sync 2 or more display devices to a signal generated
internally by the hardware, and to use both means to sync 2 or more display
devices to an external signal.

SWAP SYNC: Swap sync refers to the synchronization of buffer swaps of multiple
application windows. By means of swap sync, applications running on multiple
systems can synchronize the application buffer swaps between all the systems.
In order to work across multiple systems, swap sync requires that the systems
are frame locked.

G-SYNC DEVICE: A G-Sync Device refers to devices capable of Frame
lock/Genlock. This can be a graphics card (Quadro FX 3000G) or a stand alone
device (Quadro FX G-Sync). See "Supported Hardware" below.

SUPPORTED HARDWARE

Frame lock and genlock are supported for the following hardware:

    Board
    ----------------------------------------------------------------------
    Quadro FX 3000G
    Quadro FX G-Sync, used in conjunction with a Quadro FX 4400, Quadro FX
    4500, or Quadro FX 5500


HARDWARE SETUP

Before you begin, you should check that your hardware has been properly
installed. If you are using the Quadro FX 3000G, the genlock/frame lock signal
processing hardware is located on the dual-slot card itself, and after
installing the card, no additional setup is necessary.

If you are using the Quadro FX G-Sync board in conjunction with a graphics
card, the following additional setup steps are required. These steps must be
performed when the system is off.

  1. On the Quadro FX G-Sync board, locate the fourteen-pin connector labeled
     "primary". If the associated ribbon cable is not already joined to this
     connector, do so now. If you plan to use frame lock or genlock in
     conjunction with SLI FrameRendering or MultiGPU FrameRendering (see
     Appendix W) or other multi-GPU configurations, you should connect the
     fourteen-pin connector labeled "secondary" to the second GPU. A section
     at the end of this appendix describes restrictions on such setups.

  2. Install the Quadro FX G-Sync board in any available slot. Note that the
     slot itself is only used for support, so even a known "bad" slot is
     acceptable. The slot must be close enough to the graphics card that the
     ribbon cable can reach.

  3. Connect the other end of the ribbon cable to the fourteen-pin connector
     on the graphics card.

You may now boot the system and begin the software setup of genlock and/or
frame lock. These instructions assume that you have already successfully
installed the NVIDIA Accelerated Linux Driver Set. If you have not done so,
please see Chapter 2.

CONFIGURATION WITH NVIDIA-SETTINGS

Frame lock and genlock are configured through the nvidia-settings utility.
Please see the 'nvidia-settings(1)' man page, and the nvidia-settings online
help (click the "Help" button in the lower right corner of the interface for
per-page help information).

From the nvidia-settings frame lock panel, you may control the addition of
G-Sync (and display) devices to the frame lock/genlock group, monitor the
status of that group, and enable/disable frame lock and genlock.

After the system has booted and X Windows has been started, run
nvidia-settings as

    % nvidia-settings

You may wish to start this utility before continuing, as we refer to it
frequently in the subsequent discussion.

The setup of genlock and frame lock are described separately. We then describe
the use of genlock and frame lock together.

GENLOCK SETUP

After the system has been booted, connect the external signal to the house
sync connector (the BNC connector) on either the graphics card or the G-Sync
card. There is a status LED next to the connector. A solid red LED indicates
that the hardware cannot detect the timing signal. A green LED indicates that
the hardware is detecting a timing signal. An occasional red flash is okay.
The G-Sync device (graphics card or G-Sync card) will need to be configured
correctly for the signal to be detected.

In the frame lock panel of the nvidia-settings interface, add the X Server
that contains the display and G-Sync devices that you would like to sync to
this external source by clicking the "Add Devices..." button. An X Server is
typically specified in the format "system:m", e.g.:

    mycomputer.domain.com:0

or

    localhost:0

After adding an X Server, rows will appear in the "G-Sync Devices" section on
the frame lock panel that displays relevant status information about the
G-Sync devices, GPUs attached to those G-Sync devices and the display devices
driven by those GPUs. In particular, the G-Sync rows will display the server
name and G-Sync device number along with "Receiving" LED, "Rate", "House" LED,
"Port0"/"Port1" Images, and "Delay" information. The GPU rows will display the
GPU product name information along with the GPU ID for the server. The Display
Device rows will show the display device name and device type along with
server/client checkboxes, refresh rate, "Timing" LED and "Stereo" LED.

Once the G-Sync and display devices have been added to the frame lock/genlock
group, a Server display device will need to be selected. This is done by
selecting the "Server" checkbox of the desired display device.

If you are using a G-Sync card, you must also click the "Use House Sync if
Present" checkbox. To enable synchronization of this G-Sync device to the
external source, click the "Enable Frame Lock" button. The display device(s)
may take a moment to stabilize. If it does not stabilize, you may have
selected a synchronization signal that the system cannot support. You should
disable synchronization by clicking the "Disable Frame Lock" button and check
the external sync signal.

Modifications to genlock settings (e.g., "Use House Sync if Present", "Add
Devices...") must be done while synchronization is disabled.

FRAME LOCK SETUP

Frame Lock is supported across an arbitrary number of Quadro FX 3000 or Quadro
FX G-Sync systems, although mixing the two in the same frame lock group is not
supported. Additionally, each system to be included in the frame lock group
must be configured with identical mode timings. Please see Appendix J for
information on mode timings.

Connect the systems through their RJ45 ports using standard CAT5 patch cables.
These ports are located on the frame lock board itself (either the Quadro FX
3000 or the Quadro FX G-Sync board). DO NOT CONNECT A FRAME LOCK PORT TO AN
ETHERNET CARD OR HUB. DOING SO MAY PERMANENTLY DAMAGE THE HARDWARE The
connections should be made in a daisy-chain fashion: each card has two RJ45
ports, call them 1 and 2. Connect port 1 of system A to port 2 of system B,
connect port 1 of system B to port 2 of system C, etc. Note that you will
always have two empty ports in your frame lock group.

The ports self-configure as inputs or outputs once frame lock is enabled. Each
port has a yellow and a green LED that reflect this state. A flashing yellow
LED indicates an output and a flashing green LED indicates an input. A solid
green LED indicates that the port has not yet configured.

In the frame lock panel of the nvidia-settings interface, add the X server
that contains the display devices that you would like to include in the frame
lock group by clicking the "Add Devices..." button (see the description for
adding display devices in the previous section on GENLOCK SETUP. Like the
genlock status indicators, the "Port0" and "Port1" columns in the table on the
frame lock panel contain indicators whose states mirror the states of the
physical LEDs on the RJ45 ports. Thus, you may monitor the status of these
ports from the software interface.

Any X Server can be added to the frame lock group, provided that

  1. The system supporting the X Server is configured to support frame lock
     and is connected via RJ45 cable to the other systems in the frame lock
     group.

  2. The system driving nvidia-settings can locate and has display privileges
     on the X server that is to be included for frame lock.

A system can gain display privileges on a remote system by executing

    % xhost +

on the remote system. Please see the xhost(1) man page for details. Typically,
frame lock is controlled through one of the systems that will be included in
the frame lock group. While this is not a requirement, note that
nvidia-settings will only display the frame lock panel when running on an X
server that supports frame lock.

To enable synchronization on these display devices, click the "Enable Frame
Lock" button. The screens may take a moment to stabilize. If they do not
stabilize, you may have selected mode timings that one or more of the systems
cannot support. In this case you should disable synchronization by clicking
the "Disable Frame Lock" button and refer to Appendix J for information on
mode timings.

Modifications to frame lock settings (e.g. "Add/Remove Devices...") must be
done while synchronization is disabled.

FRAME LOCK + GENLOCK

The use of frame lock and genlock together is a simple extension of the above
instructions for using them separately. You should first follow the
instructions for Frame Lock Setup, and then to one of the systems that will be
included in the frame lock group, attach an external sync source. In order to
sync the frame lock group to this single external source, you must select a
display device driven by the GPU connected to the G-Sync card (through the
primary connector) that is connected to the external source to be the signal
server for the group. This is done by selecting the checkbox labeled "Server"
of the tree on the frame lock panel in nvidia-settings. If you are using a
G-Sync based frame lock group, you must also select the "Use House Sync if
Present" checkbox. Enable synchronization by clicking the "Enable Frame Lock"
button. As with other frame lock/genlock controls, you must select the signal
server while synchronization is disabled.

LEVERAGING FRAME LOCK/GENLOCK IN OPENGL

With the GLX_NV_swap_group extension, OpenGL applications can be implemented
to join a group of applications within a system for local swap sync, and bind
the group to a barrier for swap sync across a frame lock group. A universal
frame counter is also provided to promote synchronization across applications.

FRAME LOCK RESTRICTIONS:

The following restrictions must be met for enabling frame lock:

  1. All display devices set as client in a frame lock group must have the
     same mode timings as the server (master) display device. If a House Sync
     signal is used (instead of internal timings), all client display devices
     must be set to have the same refresh rate as the incoming house sync
     signal.

  2. All X Screens (driving the selected client/server display devices) must
     have the same stereo setting. Please see Appendix D for instructions on
     how to set the stereo X option.

  3. The frame lock server (master) display device must be on a GPU on the
     primary connector to a G-Sync device.

  4. If connecting a single GPU to a G-Sync device, the primary connector must
     be used.

  5. In configurations with more than one display device per GPU, we recommend
     enabling frame lock on all display devices on those GPUs.


SUPPORTED FRAME LOCK CONFIGURATIONS:

The following configurations are currently supported:

  1. Basic Frame Lock: Single GPU, Single X Screen, Single Display Device with
     or without OpenGL applications that make use of Quad-Buffered Stereo
     and/or the GLX_NV_swap_group extension.

  2. Frame Lock + TwinView: Single GPU, Single X Screen, Multiple Display
     Devices with or without OpenGL applications that make use of
     Quad-Buffered Stereo and/or the GLX_NV_swap_group extension.

  3. Frame Lock + Xinerama: 1 or more GPU(s), Multiple X Screens, Multiple
     Display Devices with or without OpenGL applications that make use of
     Quad-Buffered Stereo and/or the GLX_NV_swap_group extension.

  4. Frame Lock + TwinView + Xinerama: 1 or more GPU(s), Multiple X Screens,
     Multiple Display Devices with or without OpenGL applications that make
     use of Quad-Buffered Stereo and/or the GLX_NV_swap_group extension.

  5. Frame Lock + SLI SFR, AFR, or AA: 2 GPUs, Single X Screen, Single Display
     Device with either OpenGL applications that make use of Quad-Buffered
     Stereo or the GLX_NV_swap_group extension. Note that for Frame Lock + SLI
     Frame Rendering applications that make use of both Quad-Buffered Stereo
     and the GLX_NV_swap_group extension are not supported. Note that only
     2-GPU SLI configurations are currently supported.

  6. Frame Lock + MultiGPU SFR, AFR, or AA: 2 GPUs, Single X Screen, Single
     Display Device with either OpenGL applications that make use of
     Quad-Buffered Stereo or the GLX_NV_swap_group extension. Note that for
     Frame Lock + MultiGPU Frame Rendering applications that make use of both
     Quad-Buffered Stereo and the GLX_NV_swap_group extension are not
     supported.


______________________________________________________________________________

Appendix Y. Dots Per Inch
______________________________________________________________________________

DPI (Dots Per Inch), also known as PPI (Pixels Per Inch), is a property of an
X screen that describes the physical size of pixels. Some X applications, such
as xterm, can use the DPI of an X screen to determine how large (in pixels) to
draw an object in order for that object to be displayed at the desired
physical size on the display device.

The DPI of an X screen is computed by dividing the size of the X screen in
pixels by the size of the X screen in inches:

    DPI = SizeInPixels / SizeInInches

Since the X screen stores its physical size in millimeters rather than inches
(1 inch = 25.4 millimeters):

    DPI = (SizeInPixels * 25.4) / SizeInMillimeters

The NVIDIA X driver reports the size of the X screen in pixels and in
millimeters. On X.Org 6.9 or newer, when the XRandR extension resizes the X
screen in pixels, the NVIDIA X driver computes a new size in millimeters for
the X screen, to maintain a constant DPI (see the "Physical Size" column of
the `xrandr -q` output as an example). This is done because a changing DPI can
cause interaction problems for some applications. To disable this behavior,
and instead keep the same millimeter size for the X screen (and therefore have
a changing DPI), set the ConstantDPI option to FALSE (see Appendix D for
details).

You can query the DPI of your X screen by running:


    % xdpyinfo | grep -B1 dot


which should generate output like this:


    dimensions:    1280x1024 pixels (382x302 millimeters)
    resolution:    85x86 dots per inch



The NVIDIA X driver performs several steps during X screen initialization to
determine the DPI of each X screen:


   o If the display device provides an EDID, and the EDID contains information
     about the physical size of the display device, that is used to compute
     the DPI, along with the size in pixels of the first mode to be used on
     the display device. If multiple display devices are used by this X
     screen, then the NVIDIA X screen will choose which display device to use.
     You can override this with the "UseEdidDpi" X configuration option: you
     can specify a particular display device to use; e.g.:
     
         Option "UseEdidDpi" "DFP-1"
     
     or disable EDID-computed DPI by setting this option to false:
     
         Option "UseEdidDpi" "FALSE"
     
     EDID-based DPI computation is enabled by default when an EDID is
     available.

   o If the "-dpi" commandline option to the X server is specified, that is
     used to set the DPI (see `X -h` for details). This will override the
     "UseEdidDpi" option.

   o If the "DPI" X configuration option is specified (see Appendix D for
     details), that will be used to set the DPI. This will override the
     "UseEdidDpi" option.

   o If none of the above are available, then the "DisplaySize" X config file
     Monitor section information will be used to determine the DPI, if
     provided; see the xorg.conf or XF86Config man pages for details.

   o If none of the above are available, the DPI defaults to 75x75.


You can find how the NVIDIA X driver determined the DPI by looking in your X
log file. There will be a line that looks something like the following:

    (--) NVIDIA(0): DPI set to (101, 101); computed from "UseEdidDpi" X config
option


Note that the physical size of the X screen, as reported through `xdpyinfo` is
computed based on the DPI and the size of the X screen in pixels.

The DPI of an X screen can be confusing when TwinView is enabled: with
TwinView, multiple display devices (possibly with different DPIs) display
portions of the same X screen, yet DPI can only be advertised from the X
server to the X application with X screen granularity. Solutions for this
include:


   o Use separate X screens, rather than TwinView; see Appendix P for details.

   o Experiment with different DPI settings to find a DPI that is suitable for
     both display devices.


______________________________________________________________________________

Appendix Z. i2c Bus Support
______________________________________________________________________________

The NVIDIA Linux kernel module now includes i2c (also called I-squared-c,
Inter-IC Communications, or IIC) functionality that allows the NVIDIA Linux
kernel module to export i2c ports found on board NVIDIA cards to the Linux
kernel. This allows i2c devices on-board the NVIDIA graphics card, as well as
devices connected to the VGA and/or DVI ports, to be accessed from kernel
modules or userspace programs in a manner consistent with other i2c ports
exported by the Linux kernel through the i2c framework.

You must have i2c support compiled into the kernel, or as a module, and X must
be running. The i2c framework is available for both 2.4 and 2.6 series
kernels. Linux kernel documentation covers the kernel and userspace /dev APIs,
which you must use to access NVIDIA i2c ports.

NVIDIA has noted that in some distibutions, i2c support is enabled. However,
the Linux kernel module i2c-core.o (2.4) or i2c-core.ko (2.6), which provides
the export infrastructure, was not shipped. In this case, you will need to
build the i2c support module. For directions on how to build and install your
kernel's i2c support, please refer to your distribution's documentation for
configuring, building, and installing the kernel and associated modules.

For further information regarding the Linux kernel i2c framework, please refer
to the documentation for your kernel, located at .../Documentation/i2c/ within
the kernel source tree.

The following functionality is currently supported:


  I2C_FUNC_I2C
  I2C_FUNC_SMBUS_QUICK
  I2C_FUNC_SMBUS_BYTE
  I2C_FUNC_SMBUS_BYTE_DATA
  I2C_FUNC_SMBUS_WORD_DATA



______________________________________________________________________________

Appendix AA. Allocating DMA Buffers on 64-bit Platforms
______________________________________________________________________________

NVIDIA GPUs have limits on how much physical memory they can address. This
directly impacts DMA buffers, as a DMA buffer allocated in physical memory
that is unaddressable by the NVIDIA GPU cannot be used (or may be truncated,
resulting in bad memory accesses).

All pre-PCI Express GPUs and non-Native PCI Express GPUs (often known as
bridged GPUs) are limited to 32 bits of physical address space, which
corresponds to 4 GB of memory. On a system with greater than 4 GB of memory,
allocating usable DMA buffers can be a problem. Native PCI Express GPUs are
capable of addressing greater than 32 bits of physical address space and do
not experience the same problems.

Newer kernels provide a simple way to allocate memory that is guaranteed to
reside within the 32 bit physical address space. Kernel 2.6.15 provides this
functionality with the __GFP_DMA32 interface. Kernels earlier than this
version provide a software I/O TLB on Intel's EM64T and IOMMU support on AMD's
AMD64 platform.

Unfortunately, some problems exist with both interfaces. Early implementations
of the Linux SWIOTLB set aside a very small amount of memory for its memory
pool (only 4 MB). Also, when this memory pool is exhausted, some SWIOTLB
implementations forcibly panic the kernel. This is also true for some
implementations of the IOMMU interface.

Kernel panics and related stability problems on Intel's EM64T platform can be
avoided by increasing the size of the SWIOTLB pool with the 'swiotlb' kernel
parameter. This kernel parameter expects the desired size as a number of 4 KB
pages. NVIDIA suggests raising the size of the SWIOTLB pool to 64 MB; this is
accomplished by passing 'swiotlb=16384' to the kernel. note that newer Linux
2.6 kernels already default to a 64 MB SWIOTLB pool, see below for more
information.

Starting with Linux 2.6.9, the default size of the SWIOTLB is 64 MB and
overflow handling is improved. Both of these changes are expected to greatly
improve stability on Intel's EM64T platform. If you consider upgrading your
Linux kernel to benefit from these improvements, NVIDIA recommends that you
upgrade to Linux 2.6.11 or a more recent Linux kernel. Please see the previous
section for additional information.

On AMD's AMD64 platform, the size of the IOMMU can be configured in the system
BIOS or, if no IOMMU BIOS option is available, using the 'iommu=memaper'
kernel parameter. This kernel parameter expects an order and instructs the
Linux kernel to create an IOMMU of size 32 MB^order overlapping physical
memory. If the system's default IOMMU is smaller than 64 MB, the Linux kernel
automatically replaces it with a 64 MB IOMMU.

To reduce the risk of stability problems as a result of IOMMU or SWIOTLB
exhaustion on the X86-64 platform, the NVIDIA Linux driver internally limits
its use of these interfaces. By default, the driver will not use more than 60
MB of IOMMU/SWIOTLB space, leaving 4 MB for the rest of the system (assuming a
64 MB IOMMU/SWIOTLB).

This limit can be adjusted with the 'NVreg_RemapLimit' NVIDIA kernel module
option. Specifically, if the IOMMU/SWIOTLB is larger than 64 MB, the limit can
be adjusted to take advantage of the additional space. The 'NVreg_RemapLimit'
option expects the size argument in bytes.

NVIDIA recommends leaving 4 MB available for the rest of the system when
changing the limit. For example, if the internal limit is to be relaxed to
account for a 128 MB IOMMU/SWIOTLB, the recommended remap limit is 124 MB.
This remap limit can be specified by passing 'NVreg_RemapLimit=0x7c00000' to
the NVIDIA kernel module.

Please also see the 'The X86-64 platform (AMD64/EM64T) and 2.6 kernels'
section in .

