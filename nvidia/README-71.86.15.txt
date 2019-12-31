NVIDIA Accelerated Linux Driver Set README & Installation Guide

Last Updated: $Date: 2009/07/02 $
Most Recent Driver: 71.86.15


The NVIDIA Accelerated Linux Driver Set brings both accelerated 2D
functionality and high performance OpenGL support to Linux x86_64 with the
use of NVIDIA graphics processing units (GPUs).

These drivers provide optimized hardware acceleration of OpenGL
applications via a direct-rendering X Server and support nearly all
NVIDIA graphics chips (please see APPENDIX A for a complete list of
supported chips).  TwinView, TV-Out and flat panel displays are also
supported.

This README describes how to install, configure, and use the NVIDIA
Accelerated Linux Driver Set.  This file is posted on NVIDIA's web site
(www.nvidia.com), and is installed in /usr/share/doc/NVIDIA_GLX-1.0/.


__________________________________________________________________________

CONTENTS:

        (sec-01) CHOOSING THE NVIDIA PACKAGES APPROPRIATE FOR YOUR SYSTEM
        (sec-02) INSTALLING THE NVIDIA DRIVER
        (sec-03) EDITING YOUR X CONFIG FILE
        (sec-04) FREQUENTLY ASKED QUESTIONS
        (sec-05) CONTACTING US
        (sec-06) FURTHER RESOURCES

        (app-a)  APPENDIX A: SUPPORTED NVIDIA GRAPHICS CHIPS
        (app-b)  APPENDIX B: MINIMUM SOFTWARE REQUIREMENTS
        (app-c)  APPENDIX C: INSTALLED COMPONENTS
        (app-d)  APPENDIX D: X CONFIG OPTIONS
        (app-e)  APPENDIX E: OPENGL ENVIRONMENT VARIABLE SETTINGS
        (app-f)  APPENDIX F: CONFIGURING AGP
        (app-g)  APPENDIX G: ALI SPECIFIC ISSUES
        (app-h)  APPENDIX H: TNT SPECIFIC ISSUES
        (app-i)  APPENDIX I: CONFIGURING TWINVIEW
        (app-j)  APPENDIX J: CONFIGURING TV-OUT
        (app-k)  APPENDIX K: CONFIGURING A LAPTOP
        (app-l)  APPENDIX L: PROGRAMMING MODES
        (app-m)  APPENDIX M: FLIPPING AND UBB
        (app-n)  APPENDIX N: KNOWN ISSUES
        (app-o)  APPENDIX O: PROC INTERFACE
        (app-p)  APPENDIX P: XVMC SUPPORT
        (app-q)  APPENDIX Q: GLX SUPPORT
        (app-r)  APPENDIX R: CONFIGURING MULTIPLE X SCREENS ON ONE CARD
        (app-s)  APPENDIX S: POWER MANAGEMENT SUPPORT
        (app-t)  APPENDIX T: DISPLAY DEVICE NAMES
        (app-u)  APPENDIX U: THE COMPOSITE X EXTENSION
        (app-v)  APPENDIX V: NVIDIA-SETTINGS
        (app-w)  APPENDIX W: THE XRANDR X EXTENSION

Please note that, in order to keep the instructions more concise, most
caveats and frequently encountered problems are not detailed in the
installation instructions, but rather in the FREQUENTLY ASKED QUESTIONS
section.  Therefore, it is recommended that you read this entire README
before proceeding to perform any of the steps described.


__________________________________________________________________________

(sec-01) CHOOSING THE NVIDIA PACKAGES APPROPRIATE FOR YOUR SYSTEM
__________________________________________________________________________

NVIDIA has a unified driver architecture model; this means that one driver
set can be used with all supported NVIDIA graphics chips.  Please see
Appendix A for a list of the NVIDIA graphics chips supported by the
current drivers.

Driver release 1.0-4349 introduced a new packaging
and installation mechanism, which greatly simplifies the
installation process.  There is only a single file to download:
NVIDIA-Linux-x86_64-71.86.15-pkg1.run.  This contains
everything previously contained by the old NVIDIA_kernel and NVIDIA_GLX
packages.

Driver release 71.86.15 introduces a package suffix ("-pkg#") to
the .run file.  This is used to distinguish between packages containing
the same driver, but with different precompiled kernel interfaces.
If there is any confusion, just download the .run file with the largest
pkg number.

__________________________________________________________________________

(sec-02) INSTALLING THE NVIDIA DRIVER
__________________________________________________________________________

BEFORE YOU BEGIN DRIVER INSTALLATION

Before beginning the driver installation, you should exit the X server.
In addition you should set your default run level so you will boot to a
vga console and not boot directly into X (please consult the documentation
that came with your Linux distribution if you are unsure how to do this;
this is normally done by modifying your /etc/inittab file).  This will
make it easier to recover if there is a problem during the installation.
After installing the driver you must edit your X config file before
the newly installed driver will be used.  See the section below entitled
EDITING YOUR X CONFIG FILE.


INTRODUCTION TO THE NEW NVIDIA DRIVER INSTALLER

After you have downloaded NVIDIA-Linux-x86_64-71.86.15-pkg1.run,
begin installation by exiting X, cd'ing into the directory containing
the downloaded file, and run:

    sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run

The .run file is a self-extracting archive.  When the .run file is
executed, it extracts the contents of the archive, and runs the contained
`nvidia-installer` utility, which will walk you through installation of
the NVIDIA driver.

The .run file accepts many commandline options.  Here are a few of the
more common options:

    --info
        Print embedded info about the .run file and exit.

    --check
        Check integrity of the archive and exit.

    --extract-only
        Extract the contents of ./NVIDIA-Linux-x86_64-71.86.15.run,
        but do not run 'nvidia-installer'.

    --help
        Print usage information for the common commandline options
        and exit.

    --advanced-options
        Print usage information for the common commandline options as
        well as the advanced options, and then exit.

Installation will also install the utility `nvidia-installer`, which may
be later used to uninstall drivers, auto-download updated drivers, etc.


KERNEL INTERFACES

The NVIDIA kernel module has a kernel interface layer which must be
compiled specifically for the configuration and version of the kernel
you are running.  NVIDIA distributes the source code to this kernel
interface layer, as well as a precompiled version for many of the kernels
distributed by some popular distributions.
  
When the installer is run, it will determine if it has a precompiled
kernel interface for the kernel you are running.  If it does not have
one, it will check if there is one on the NVIDIA ftp site (assuming you
have an internet connection), and download it.

If a precompiled kernel interface is found that matches your kernel,
then that will be linked[1] against the binary portion of the NVIDIA
kernel module.  The result of this operation will be a kernel module
appropriate for your kernel.

If no matching precompiled kernel interface is found, then the installer
will compile the kernel interface for you.  However, first it will
check that you have the correct kernel headers intalled on your system.
If the installer must compile the kernel interface, then you must install
the kernel-sources package for your kernel.

[1] NOTE: installation requires that you have a linker installed.
The linker, usually '/usr/bin/ld', is part of the binutils package;
please be sure you have this package installed prior to installing the
NVIDIA driver.


FEATURES OF NVIDIA-INSTALLER

o Uninstall: Driver installation will backup any conflicting files
  and record what new files are installed on the system.  You may run:

    nvidia-installer --uninstall
  
  to uninstall the current driver; this will remove any files that
  were installed on the system, and restore any backed up files.
  Installing new drivers implicitly uninstalls any previous drivers.

o Auto-Updating: If you run:

    nvidia-installer --latest
  
  the utility will connect to NVIDIA's FTP site, and report the latest
  driver version and the url to the latest driver file.
  
  If you run:

    nvidia-installer --update
  
  the utility will connect to NVIDIA's FTP site, download the most recent
  driver file, and install it.

o Multiple user interfaces: The installer will use an ncurses-based
  user interface if it can find the correct ncurses library, otherwise,
  it will fall back to a simple commandline user interface.  To disable
  use of the ncurses user interface, use the option '--ui=none'.


NVIDIA-INSTALLER FAQ

Q: How do I extract the contents of the .run file without actually
   installing the driver?

A: Run:

    sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run --extract-only

   This will create the directory NVIDIA-Linux-x86_64-71.86.15-pkg1
   which contains the uncompressed contents of the .run file.


Q: How can I see the source code to the kernel interface layer?

A: The source files to the kernel interface layer are in the usr/src/nv
   directory of the extracted .run file.  To get to these sources, run:

    sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run --extract-only
    cd NVIDIA-Linux-x86_64-71.86.15-pkg1/usr/src/nv/


Q: I just upgraded my kernel, and now the NVIDIA kernel module will not
   load.  What is wrong?

A: The kernel interface layer of the NVIDIA kernel module must be
   compiled specifically for the configuration and version of your kernel.
   If you upgrade your kernel, then the simplest solution is to reinstall
   the driver.

   ADVANCED: You can install the NVIDIA kernel module for a non 
   running kernel (for example: in the situation where you just built
   and installed a new kernel, but have not rebooted yet) with a command
   line such as this:

    sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run --kernel-name='KERNEL_NAME'

   Where 'KERNEL_NAME' is what `uname -r` would report if the target
   kernel were running.


Q: Why does NVIDIA not provide rpms anymore?

A: Not every Linux distribution uses rpm, and NVIDIA wanted a single
   solution that would work across all Linux distributions.  As indicated
   in the NVIDIA Software License, Linux distributions are welcome to
   repackage and redistribute the NVIDIA Linux driver in whatever package
   format they wish.


Q: nvidia-installer does not work on my computer.  How can I install the
   driver contained within the .run file?

A: To install the NVIDIA driver contained within the .run file without
   using nvidia-installer, you can use the included Makefile:

       sh ./NVIDIA-Linux-x86_64-71.86.15-pkg1.run --extract-only
       cd NVIDIA-Linux-x86_64-71.86.15-pkg1
       make install

   This method of installation is not recommended, and is only provided
   as a last resort, should nvidia-installer not work correctly on
   your system.


Q: Can the nvidia-installer use a proxy server?

A: Yes, because the ftp support in nvidia-installer is based on snarf,
   it will honor the FTP_PROXY, SNARF_PROXY, and PROXY environment
   variables.


Q: What is the significance of the "pkg#" suffix on the .run file?

A: The "pkg#" suffix is used to distinguish between .run files containing
   the same driver, but different sets of precompiled kernel interfaces.
   If a distribution releases a new kernel after an NVIDIA driver is
   released, the current NVIDIA driver can be repackaged to include
   a precompiled kernel interface for that newer kernel (in addition
   to all the precompiled kernel interfaces that were included in the
   previous package of the driver).

   .run files with the same version number, but different pkg numbers,
   only differ in what precompiled kernel interfaces are included.
   Additionally, .run files with higher pkg numbers will contain
   everything the .run files with lower .pkg numbers contain.


Q: I have already installed NVIDIA-Linux-x86_64-71.86.15-pkg1.run,
   but I see that NVIDIA-Linux-x86_64-71.86.15-pkg2.run was just
   posted on the NVIDIA Linux driver download page.  Should I download
   and install NVIDIA-Linux-x86_64-71.86.15-pkg2.run?

A: This is not necessary.  The driver contained within all
   71.86.15 .run files will be identical.  There is no need
   to reinstall.


Q: Can I add my own precompiled kernel interfaces to a .run file?

A: Yes, the "--add-this-kernel" .run file option will unpack the .run
   file, build a precompiled kernel interface for the currently running
   kernel, and repackage the .run file, appending "-custom" to the file
   name.  This may be useful, for example. if you administer multiple
   Linux machines, each running the same kernel.


Q: Where can I find the source code for the nvidia-installer utility?

A: The nvidia-installer utility is released under the
   GPL.  The latest source code for it is available at:
   ftp://download.nvidia.com/XFree86/nvidia-installer/


NVIDIA-INSTALLER ACKNOWLEDGEMENTS

nvidia-installer was inspired by the loki_update tool:
(http://www.lokigames.com/development/loki_update.php3.)

The ftp and http support in nvidia-installer is based upon snarf 7.0:
(http://www.xach.com/snarf/).

The self-extracting archive (aka ".run file") is generated using
makeself.sh: (http://www.megastep.org/makeself/)


__________________________________________________________________________

(sec-03) EDITING YOUR X CONFIG FILE
__________________________________________________________________________

In April of 2004, the X.org Foundation released an X server based on
the XFree86 X server.  Many Linux distributions will use the X.org
X server in the future, rather than XFree86.  The differences between
the two X servers should have no impact on NVIDIA Linux users with
two exceptions:

    1) The X.org configuration file name, though it uses the same syntax
       as XFree86's XF86Config file, is called /etc/X11/xorg.conf;
       this README refers generically to these configuration files as
       "the X config file".

    2) The X.org log file, though its output is nearly identical
       to the XFree86.0.log file, is called /var/log/Xorg.0.log; this
       README refers generically to these files as "the X log file".


When XFree86 4.0 was released, it used a slightly different XF86Config
file syntax than the 3.x series did, and so to allow both 3.x and 4.x
versions of XFree86 to co-exist on the same system, it was decided that
XFree86 4.x was to use the configuration file "/etc/X11/XF86Config-4"
if it existed, and only if that file did not exist would the file
"/etc/X11/XF86Config" be used (actually, that is an over-simplification
of the search criteria; please see the XF86Config man page for a
complete description of the search path).  Please make sure you know
what configuration file your X server is using.  If you are in doubt,
look for a line beginning with "(==) Using config file:" in your X log
file ("/var/log/XFree86.0.log" or "/var/log/Xorg.0.log").

If you do not have a working X config file, there are several ways
to start: there is a sample config file that comes with XFree86,
and there is a sample config file included with the NVIDIA driver
package (it gets installed in /usr/share/doc/NVIDIA_GLX-1.0/).
You could also use a program like 'xf86config'; some distributions
provide their own tool for generating an X config file.  For more
on X config file syntax, please refer to the man page (`man XF86Config`,
or `man xorg.conf`).

If you already have an X config file working with a different driver
(such as the 'nv' or 'vesa' driver), then all you need to do is find
the relevant Device section and replace the line:

        Driver "nv"
    (or Driver "vesa")

with 

        Driver "nvidia"  

In the Module section, make sure you have:

        Load   "glx"

You should also remove the following lines:
      
        Load  "dri"
        Load  "GLcore"

if they exist.  There are also numerous options that can be added to the
X config file to fine-tune the NVIDIA X driver.  Please see Appendix D
for a complete list of these options.

Once you have configured your X config file, you are ready to restart X
and begin using the accelerated OpenGL libraries.  After you restart X,
you should be able to run any OpenGL application and it will automatically
use the new NVIDIA libraries.  If you encounter any problems, please
see the FREQUENTLY ASKED QUESTIONS section below.


__________________________________________________________________________

(sec-04) FREQUENTLY ASKED QUESTIONS
__________________________________________________________________________


Q: Where should I start when diagnosing display problems?

A: One of the most useful tools for diagnosing problems is the X
   log file in /var/log (the file is named: "/var/log/XFree86.<#>.log" or
   "/var/log/Xorg.<#>.log" where "<#>" is the server number -- usually 0).
   Lines that begin with "(II)" are information, "(WW)" are warnings, and
   "(EE)" are errors.  You should make sure that the correct config file
   (ie the config file you are editing) is being used; look for the line
   that begins with: "(==) Using config file:".  Also check that the
   NVIDIA driver is being used, rather than the 'nv' or 'vesa' driver;
   you can look for: "(II) LoadModule: "nvidia"", and lines from the
   driver should begin with: "(II) NVIDIA(0)".


Q: How can I increase the amount of data printed in the X log file?

A: By default, the NVIDIA X driver prints relatively few messages to
   stderr and the X log file.  If you need to troubleshoot, then it may
   be helpful to enable more verbose output by using the X command line
   options "-verbose" and "-logverbose" which can be used to set the
   verbosity level for the stderr and log file messages, respectively.
   The NVIDIA X driver will output more messages when the verbosity
   level is at or above 5 (X defaults to verbosity level 1 for stderr
   and level 3 for the log file).  So, to enable verbose messaging from
   the NVIDIA X driver to both the log file and stderr, you could start
   X by doing the following: 'startx -- -verbose 5 -logverbose 5'.


Q: My X server fails to start, and my X log file contains the error:

   "(EE) NVIDIA(0): Failed to load the NVIDIA kernel module!"

A: The X driver will abort with this error message if the NVIDIA kernel
   module fails to load.

   If you receive this error, you should check the output of `dmesg`
   for kernel error messages and/or attempt to load the kernel module
   explicitly with `modprobe nvidia`.  If unresolved symbols are
   reported, then the kernel module was most likely built against a
   Linux kernel source tree (or kernel headers) for a kernel revision
   or configuration that doesn't match the running kernel.

   You can specify the location of the kernel source tree (or headers)
   when you install the NVIDIA driver using the --kernel-source-path
   command line option (see `sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run
   --advanced-options` for details).

   Old versions of the module-init-tools include `modprobe` binaries
   that report an error when instructed to load a module that's already
   loaded into the kernel.  Please upgrade your module-init-tools if
   you receive an error message to this effect.

   The X server reads "/proc/sys/kernel/modprobe" to determine the
   path to the `modprobe` utility and falls back to `/sbin/modprobe`
   if the file doesn't exist.  Please make sure that this path is
   valid and refers to a `modprobe` binary compatible with the Linux
   kernel running on your system.

   The "LoadKernelModule" X driver option can be used to change the
   default behavior and disable kernel module auto-loading.


Q: How and when are the the NVIDIA device files created on Linux?

A: Depending on the target system's configuration, the NVIDIA device
   files used to be created in one of three different ways:

   - at installation time, using mknod
   - at module load time, via devfs (Linux device file system)
   - at module load time, via hotplug/udev

   With currrent NVIDIA driver releases, device files are created or
   modified by the X driver when the X server is started.

   By default, the NVIDIA driver will attempt to create device files
   with the following attributes:

     UID:  0     - 'root'
     GID:  0     - 'root'
     Mode: 0666  - 'rw-rw-rw-'

   Existing device files are changed if their attributes don't match
   these defaults. If you wish for the NVIDIA driver to create the
   device files with different attributes, you can specify them with
   the "NVreg_DeviceFileUID" (user), "NVreg_DeviceFileGID" (group)
   and "NVreg_DeviceFileMode" NVIDIA Linux kernel module parameters.

   For example, the NVIDIA driver can be instructed to create device
   files with UID=0 (root), GID=44 (video) and Mode=0660 by passing
   the following module parameters to the NVIDIA Linux kernel module:

     NVreg_DeviceFileUID=0 NVreg_DeviceFileGID=44 NVreg_DeviceFileMode=0660

   The "NVreg_ModifyDeviceFiles" NVIDIA kernel module parameter will
   disable dynamic device file management, if set to 0.


Q: My X server fails to start, and my X log file contains the error:

   "(EE) NVIDIA(0): Failed to initialize the NVIDIA kernel module!"

A: Nothing will work if the NVIDIA kernel module does not function
   properly.  If you see anything in the X log file like "(EE)
   NVIDIA(0): Failed to initialize the NVIDIA kernel module!" then
   there is most likely a problem with the NVIDIA kernel module.

   The NVIDIA kernel module may print error messages indicating
   a problem -- to view these messages please check the output of
   `dmesg`, /var/log/messages, or wherever syslog is directed to
    place kernel messages.  These messages are prepended with "NVRM".


Q: My X server fails to start, and my X log file contains the error:

    "(EE) NVIDIA(0): The NVIDIA kernel module does not appear to be receiving
     (EE) NVIDIA(0):      interrupts generated by the NVIDIA graphics device. 
     (EE) NVIDIA(0):      Please see the FREQUENTLY ASKED QUESTIONS section in the
     (EE) NVIDIA(0):      README for additional information."

A: This can be caused by a variety of problems, such as PCI IRQ routing
   errors, I/O APIC problems or conflicts with other devices sharing
   the IRQ (or their drivers).

   If possible, configure your system such that your graphics card does
   not share its IRQ with other devices (try moving the graphics card
   to another slot (if applicable), unload/disable the driver(s) for the
   device(s) sharing the card's IRQ, or remove/disable the device(s)).

   Depending on the nature of the problem, one of (or a combination of)
   these kernel parameters might also help:

     pci=noacpi     (don't use ACPI for PCI IRQ routing)
     pci=biosirq    (use PCI BIOS calls to retrieve the IRQ routing table)
     noapic         (don't use I/O APICs present in the system)
     acpi=off       (disable ACPI)


Q: My X server fails to start, and my X log file contains the error:

    "(EE) NVIDIA(0): The interrupt for the NVIDIA graphics device appears to be
     (EE) NVIDIA(0):      edge-triggered.  Please see the FREQUENTLY ASKED
     (EE) NVIDIA(0):      QUESTIONS section in the README for additional
     (EE) NVIDIA(0):      information."

A: This occurs when ACPI is not used to program interrupt routing in the
   APIC. This often occurs on 2.4 Linux kernels, which do not fully
   support ACPI, or 2.6 kernels when ACPI is disabled or fails to
   initialize. In these cases, the Linux kernel falls back to tables
   provided by the system BIOS. In some cases the system BIOS assumes
   ACPI will be used for routing interrupts and configures these tables
   to incorrectly label all interrupts as edge-triggered. The current
   interrupt configuration can be found in /proc/interrupts.

   Available workarounds include: updating to a newer system BIOS,
   trying a 2.6 kernel with ACPI enabled, or passing the 'noapic' option
   to the kernel to force interrupt routing through the traditional
   Programmable Interrupt Controller (PIC). Newer kernels also provide
   an interrupt polling mechanism to attempt to work around this
   problem. This mechanism can be enabled by passing the 'irqpoll'
   parameter to the kernel.

   Currently, the NVIDIA driver will attempt to detect edge triggered
   interrupts and X will purposely fail to start (to avoid stability
   issues).  This behavior can be overridden by setting the
   "NVreg_RMEdgeIntrCheck" NVIDIA Linux kernel module parameter. This
   parameter defaults to "1", which enables the edge triggered interrupt
   detection. Set this parameter to "0" to disable this detection.


Q: X starts for me, but OpenGL applications terminate immediately.

A: If X starts, but OpenGL causes problems, you most likely have a
   problem with other libraries in the way, or there are stale symlinks.
   See Appendix C for details.  Sometimes, all it takes is to rerun
   'ldconfig'.

   You should also check that the correct extensions are present;
   'xdpyinfo' should show the "GLX" and "NV-GLX" extensions present.
   If these two extensions are not present, then there is most likely
   a problem with the glx module getting loaded or it is unable to
   implicitly load GLcore.  Check your X config file and make sure that
   you are loading glx (see "Editing Your X config File" above). If your X
   config file is correct, then check the X log file for warnings/errors
   pertaining to GLX.  Also check that all of the necessary symlinks
   are in place (refer to Appendix C).


Q: Installing the NVIDIA kernel module gives an error message like:
        #error Modules should never use kernel-headers system headers
        #error but headers from an appropriate kernel-source

A: You need to install the source for the Linux kernel.  In most
   situations you can fix this problem by installing the kernel-source
   package for your distribution


Q: OpenGL applications crash and print out the following warning:
    
        WARNING: Your system is running with a buggy dynamic loader.
        This may cause crashes in certain applications.  If you
        experience crashes you can try setting the environment
        variable __GL_SINGLE_THREADED to 1.  For more information
        please consult the FREQUENTLY ASKED QUESTIONS section in
        the file /usr/share/doc/NVIDIA_GLX-1.0/README.

A: The dynamic loader on your system has a bug which will cause
   applications linked with pthreads, and that dlopen() libGL multiple 
   times, to crash.  This bug is present in older versions of the dynamic 
   loader.  Distributions that shipped with this loader include but
   are not limited to Red Hat Linux 6.2 and Mandrake Linux 7.1.  Version
   2.2 and later of the dynamic loader are known to work properly.  If
   the crashing application is single threaded then setting the environment 
   variable __GL_SINGLE_THREADED to 1 will prevent the crash.
   In the bash shell you would enter:
        export __GL_SINGLE_THREADED=1
   and in csh and derivatives use:
        setenv __GL_SINGLE_THREADED 1
   Previous releases of the NVIDIA Accelerated Linux Driver Set attempted
   to work around this problem, however the workaround caused problems with
   other applications and was removed after version 1.0-1541.


Q: When I run Quake3, it crashes when changing video modes; what is wrong?

A: You are probably experiencing the problem described above.  Please
   check the text output for the "WARNING" message describe in the
   previous hint.  Setting __GL_SINGLE_THREADED to 1 as described
   above, before running Quake3  will fix the problem.


Q: My system runs, but seems unstable.  What is wrong?

A: Your stability problems may be AGP-related.  See Appendix F for
   details.


Q: I cannot build the NVIDIA kernel module, or I can build the NVIDIA
   kernel module, but modprobe/insmod fails to load the module into
   my kernel.  What is wrong?

A: These problems are generally caused by the build using the wrong kernel
   header files (ie header files for a different kernel version than
   the one you are running).  The convention used to be that kernel
   header files should be stored in "/usr/include/linux/", but that
   is deprecated in favor of "/lib/modules/`uname -r`/build/include".
   The nvidia-installer should be able to determine the location on your
   system; however, if you encounter a problem you can force the build
   to use certain header files by using the --kernel-include-dir option.
   Obviously, for this to work, you need the appropriate kernel header
   files installed on your system.  Consult the documentation that came
   with your distribution; some distributions do not install the kernel
   header files by default, or they install headers that do not coincide
   properly with the kernel you are running.


Q: Why do OpenGL applications run so slow?

A: The application is probably using a different library still on your
   system, rather than the NVIDIA supplied OpenGL library.  Please see
   APPENDIX C for details.


Q: There are problems running Quake2.

A: Quake2 requires some minor setup to get it going.  First, in the Quake2
   directory, the install creates a symlink called libGL.so that points
   at libMesaGL.so.  This symlink should be removed or renamed.  Then,
   to run Quake2 in OpenGL mode, you would type: 'quake2 +set vid_ref glx
   +set gl_driver libGL.so'.  Quake2 does not seem to support any kind of
   full-screen mode, but you can run your X server at whatever resolution
   Quake2 runs at to emulate full-screen mode.


Q: There are problems running Heretic II.

A: Heretic II also installs, by default, a symlink called libGL.so in
   the application directory.  You can remove or rename this symlink, since
   the system will then find the default libGL.so (which our
   drivers install in /usr/lib).  From within Heretic II you
   can then set your render mode to OpenGL in the video menu.
   There is also a patch available to Heretic II from lokigames at:
   http://www.lokigames.com/products/heretic2/updates.php3


Q: Where can I get gl.h or glx.h so I can compile OpenGL programs?

A: Most systems come with these header files preinstalled.  However,
   NVIDIA provides its own gl.h and glx.h files which get installed
   in /usr/share/doc/NVIDIA_GLX-1.0/include/GL/.  To use these
   files, either manually copy them into /usr/include/GL/,
   or instruct the installer to install these files in
   /usr/include/GL/ by passing the '--opengl-headers' option to the
   NVIDIA-Linux-x86_64-71.86.15-pkg1.run file during installation.


Q: Can I receive email notification of new NVIDIA Accelerated Linux
   Driver Set releases?

A: Yes.  Fill out the form at:
   http://www.nvidia.com/view.asp?FO=driver_update


Q: My system hangs when vt-switching if I have rivafb enabled.

A: Using both rivafb and the NVIDIA kernel module at the same time is
   currently broken.  In general, using two independent software drivers
   to drive the same piece of hardware is a bad idea.


Q: Compiling the NVIDIA kernel module gives this error:

        You appear to be compiling the NVIDIA kernel module with
        a compiler different from the one that was used to compile
        the running kernel. This may be perfectly fine, but there
        are cases where this can lead to unexpected behaviour and
        system crashes.

        If you know what you are doing and want to override this
        check, you can do so by setting IGNORE_CC_MISMATCH.

        In any other case, set the CC environment variable to the
        name of the compiler that was used to compile the kernel.

A: You should compile the NVIDIA kernel module with the same compiler
   version that was used to compile your kernel.  Some Linux kernel data
   structures are dependent on the version of gcc used to compile it;
   for example, in include/linux/spinlock.h:

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

   If the kernel is compiled with gcc 2.x, but gcc 3.x is used when the
   kernel interface is compiled (or vice versa), the size of rwlock_t
   will vary, and things like ioremap will fail.

   To check what version of gcc was used to compile your kernel, you
   can examine the output of:

        cat /proc/version

   To check what version of gcc is currently in your $PATH, you can
   examine the output of:

        gcc -v


Q: X fails with error "Failed to allocate LUT context DMA"

A: This is one of the possible consequences of compiling the NVIDIA
   kernel interface with a different gcc version than used to compile
   the Linux kernel (see above).


Q: What is NVIDIA's policy towards development series Linux kernels?

A: NVIDIA does not officially support development series kernels.
   However, all the kernel module source code that interfaces with the
   Linux kernel is available in the usr/src/nv/ directory of the .run file.
   NVIDIA encourages members of the Linux community to develop patches
   to these source files to support development series kernels.  A google
   search will most likely yield several community supported patches.


Q: I recently updated various libraries on my system using my Linux
   distributor's update utility, and the NVIDIA graphics driver no
   longer works.  What is wrong?

A: Conflicting libraries may have been installed by your
   distribution's update utility; please see APPENDIX C: INSTALLED
   COMPONENTS for details on how to diagnose this.


Q: `rpm --rebuild` gives an error "unknown option".

A: Recent versions of rpm no longer support the "--rebuild" option;
   if you have such a version of rpm, you should instead use the command
   `rpmbuild --rebuild`.  The `rpmbuild` executable is provided by the
   rpm-build package.


Q: I am using either nForce of nForce2 internal graphics, and I see
   warnings like this in my X log file:

    Not using mode "1600x1200" (exceeds valid memory bandwidth usage)

A: Integrated graphics have stricter memory bandwidth limitations
   that restrict the resolution and refresh rate of the modes you
   request.  To work around this, you can reduce the maximum refresh
   rate by lowering the upper value of the "VertRefresh" range in the
   Monitor section of your X config file.  Though not recommended,
   you can disable the memory bandwidth test with the "NoBandWidthTest"
   X config file option.


Q: I have rebuilt the NVIDIA kernel module, but when I try to insert
   it, I get a message telling me I have unresolved symbols.

A. Unresolved symbols are most often caused by a mismatch between your
   kernel sources and your running kernel.  They must match for the
   NVIDIA kernel module to build correctly.  Please make sure your kernel
   sources are installed and configured to match your running kernel.


Q: How do I tell if I have my kernel sources installed?

A: If you are running on a distro that uses RPM (Red Hat, Mandrake, SuSE,
   etc), then you can use RPM to tell you.  At a shell prompt, type:

    `rpm -qa | grep kernel`

   and look at the output.  You should see a package that corresponds
   to your kernel (often named something like kernel-2.4.18-3)
   and a kernel source package with the same version (often named
   something like kernel-source-2.4.18-3).  If none of the lines seem
   to correspond to a source package, then you will probably need to
   install it.  If the versions listed mismatch (ex: kernel-2.4.18-10 vs.
   kernel-source-2.4.18-3), then you will need to update the kernel-source
   package to match the installed kernel.  If you have multiple kernels
   installed, you need to install the kernel-source package that
   corresponds to your *running* kernel (or make sure your installed
   source package matches the running kernel).  You can do this by
   looking at the output of 'uname -r' and matching versions.


Q: Why am I unable to load the NVIDIA kernel module that I compiled
   for the Red Hat Linux 7.3 2.4.18-3bigmem kernel?

A: The kernel header files Red Hat Linux distributes for Red Hat Linux 7.3
   2.4.18-3bigmem kernel are misconfigured.  NVIDIA's precompiled kernel
   module for this kernel can be loaded, but if you wish to compile the
   NVIDIA kernel interface files yourself for this kernel, then you will
   need to perform the following:

    cd /lib/modules/`uname -r`/build/
    make mrproper
    cp configs/kernel-2.4.18-i686-bigmem.config .config
    make oldconfig dep

   Note: Red Hat Linux ships kernel header files that are simultaneously
   configured for ALL of their kernels for a particular distribution
   version.  A header file generated at boot time sets up a few parameters
   that select the correct configuration.  Rebuilding the kernel headers
   with the above commands will create header files suitable for the
   Red Hat Linux 7.3 2.4.18-3bigmem kernel configuration only, thus trashing
   the header files for the other configurations.


Q: X takes a long time to start (possibly several minutes).  What can
   I do?

A: Most of the startx delay problems we have found are caused by incorrect
   data in video BIOSes about what display devices are possibly connected
   or what i2c port should be used for detection.  You can work around
   these problems with the X config option "IgnoreDisplayDevices"
   (please see the description in (app-d) APPENDIX D: X CONFIG OPTIONS).


Q: Why does X use so much memory?

A: When measuring any application's memory usage, you must be
   careful to distinguish between physical system RAM used and virtual
   mappings of shared resources.  For example, most shared libraries exist
   only once in physical memory but are mapped into multiple processes.
   This memory should only be counted once when computing total memory
   usage.  In the same way, the video memory on a graphics card or
   register memory on any device can be mapped into multiple processes.
   These mappings do not consume normal system RAM.

   This has been a frequently discussed topic on XFree86 mailing
   lists; see, for example:

    http://marc.theaimsgroup.com/?l=xfree-xpert&m=96835767116567&w=2

   The `pmap` utility described in the above thread and available here:

    http://web.hexapodia.org/~adi/pmap.c

   is a useful tool in distinguishing between types of memory mappings.
   For example, while `top` may indicate that X is using several hundred
   MB of memory, the last line of output from pmap:

    mapped:   287020 KB writable/private: 9932 KB shared: 264656 KB

   reveals that X is really only using roughly 10MB of system RAM
   (the "writable/private" value).

   Note, also, that X must allocate resources on behalf of X clients (the
   window manager, your web browser, etc); X's memory usage will increase
   as more clients request resources such as pixmaps, and decrease as
   you close X applications.


Q: OpenGL applications leak significant amounts of memory on my system!

A: If your kernel is making use of the -rmap VM, the system may be leaking
   memory due to a memory management optimization introduced in -rmap14a.
   The -rmap VM has been adopted by several popular distributions, the
   memory leak is known to be present in some of the distribution kernels;
   it has been fixed in -rmap15e.

   If you suspect that your system is affected, please try upgrading your
   kernel or contact the distribution's vendor for assistance.


Q: Some OpenGL applications (like Quake3 Arena) crash when I start them
   on Red Hat Linux 9.0.

A: Some versions of the glibc package shipped by Red Hat that support
   TLS do not properly handle using dlopen() to access shared libraries
   which utilize some TLS models.  This problem is exhibited, for example,
   when Quake3 Area dlopen()'s NVIDIA's libGL library.  Please obtain
   at least glibc-2.3.2-11.9 which is available as an update from Red Hat.


Q: I have installed the driver, but my Enable 3D Acceleration checkbox
   is still greyed out!  What did I do wrong?

A: Most distribution-provided configuration applets are not aware of
   the NVIDIA accelerated driver, and consequently will not update
   themselves when you install the driver.  Your driver, if it has been
   installed properly, should function fine.


Q: Where can I find the tarballs?

A: Plain tarballs are no longer available.  The .run file is a
   tarball with a shell script prepended.  You can execute the .run
   file with the '--extract-only' option to unpack the tarball.


Q: Where can I find older driver versions?

A: Please visit ftp://download.nvidia.com/XFree86_40/.


Q: X does not restore the vga console when run on a TV.  I get this
   error message in my X log file:

    Unable to initialize the X int10 module; the console may not be
    restored correctly on your TV.

A: The NVIDIA X driver uses the X Int10 module to save
   and restore console state on TV out, and will not be able to restore
   the console correctly if it cannot use the Int10 module.  If you
   have built the X server yourself, please be sure you have built the
   Int10 module.  If you are using a build of the X server provided by a
   Linux distribution, and are missing the Int10 module, please contact
   your distributor,


Q: When changing settings in games like Quake 3 Arena, or Wolfenstein
   Enemy Territry, the game crashes and I see this error:

        ...loading libGL.so.1: QGL_Init: dlopen libGL.so.1 failed: 
        /usr/lib/tls/libGL.so.1: shared object cannot be dlopen()ed:
        static TLS memory too small

A: These games close and reopen the NVIDIA OpenGL driver (via
   dlopen()/dlclose()) when settings are changed.  On some versions of
   glibc (such as the one shipped with Red Hat Linux 9), there is a bug
   that leaks static TLS entries.  This glibc bug causes subsequent
   re-loadings of the OpenGL driver to fail.  This is fixed in more
   recent versions of glibc; see Red Hat bug #89692:

        https://bugzilla.redhat.com/bugzilla/show_bug.cgi?id=89692


Q: X crashes during `startx`, and my X log file contains this
   error message:

    (EE) NVIDIA(0): Failed to obtain a shared memory identifier.

A: The NVIDIA OpenGL driver and the NVIDIA X driver require shared memory
   to communicate; you must have CONFIG_SYSVIPC enabled in your kernel.


Q: When I try to install the driver, the installer claims that X is
   running, even though I have exited X.  What is wrong?

A: The installer detects the presence of an X server by checking for
   X's lock files: /tmp/.X[n]-lock, where [n] is the number of the X
   Display (the installer checks for X Displays 0-7).  If you have exited
   X, but one of these files have been left behind, then you will need
   to manually delete the lock file.  DO NOT remove this file is X is
   still running.


Q: Fonts are incorrectly sized after installing the NVIDIA driver.

A: Incorrectly sized fonts are generally caused by a monitor
   reporting an incorrect physical size, which causes various X
   applications to render fonts at the wrong size.  You can check what
   X thinks the physical size of your monitor is, by running:

    xdpyinfo | grep dimensions

   This will report the size in pixels, and in millimeters.  If the
   sizes in millimeters are drastically incorrect, then you can correct
   this by adding the DisplaySize field to the monitor section of your
   X config file (see the XF86Config or xorg.conf manpages for details).

   You can check what your monitor reports its physical size is by
   running X with verbose logging: `startx -- -logverbose`.  Then,
   search your X log file for a line that looks like:

    (II) NVIDIA(0): Max H-Image Size [cm]: horiz.: 36  vert.: 27

   (the numbers will be different)  The NVIDIA driver uses these
   values to compute the DPI.


Q: I want to use Valgrind with OpenGL applications, but my
   distribution uses ELF TLS, and Valgrind cannot yet deal with NVIDIA's
   ELF TLS OpenGL.
 
A: You can set the environment variable LD_ASSUME_KERNEL to something
   below "2.3.99" (for example: `export LD_ASSUME_KERNEL 2.3.98`).
 
   NVIDIA's OpenGL libraries contain an OS ABI ELF note that indicates
   the minimum kernel version that is required to use the library.
   The ELF TLS OpenGL libraries have an OS ABI of 2.3.99 (the first
   Linux kernel that contained the necessary LDT support for ELF TLS),
   while the non ELF TLS OpenGL libraries contain an OS ABI of 2.2.5.

   The run-time loader will not load libraries with an OS ABI greater
   than the current kernel version.  The LD_ASSUME_KERNEL environment
   variable can be used to override the kernel version that the run-time 
   loader uses in this test.

   By setting LD_ASSUME_KERNEL to any kernel version below 2.3.99,
   you can force the loader to not use the ELF TLS OpenGL libraries,
   and fall back to the regular OpenGL libraries.

   If, for some reason, you need to remove this OS ABI note from the
   NVIDIA OpenGL libraries, you can do so by passing the .run file the
   "--no-abi-note" option during installation.


__________________________________________________________________________

(sec-05) CONTACTING US
__________________________________________________________________________


There is an NVIDIA Linux Driver web forum.  You can access it by going
to www.nvnews.net and following the "Forum" and "Linux Discussion Area"
links.  This is the preferable tool for seeking help; users can post
questions, answer other users' questions, and search the archives of
previous postings.

If all else fails, you can contact NVIDIA for support at:
linux-bugs@nvidia.com.  But please, only send email to this address
after you have followed the FREQUENTLY ASKED QUESTIONS section in this
README and asked for help on the nvnews.net web forum.  When emailing
linux-bugs@nvidia.com, please include the nvidia-bug-report.log file
generated by the nvidia-bug-report.sh script (which is installed as part
of driver installation).


__________________________________________________________________________

(sec-06) FURTHER RESOURCES
__________________________________________________________________________

Linux OpenGL ABI
http://oss.sgi.com/projects/ogl-sample/ABI/

NVIDIA Linux HowTo
http://www.tldp.org/HOWTO/XFree86-Video-Timings-HOWTO/index.html

OpenGL
www.opengl.org

The XFree86 Project
www.xfree86.org

#nvidia (irc.freenode.net)


__________________________________________________________________________

(app-a) APPENDIX A: SUPPORTED NVIDIA GRAPHICS CHIPS
__________________________________________________________________________

  NVIDIA CHIP NAME                     DEVICE PCI ID

  RIVA TNT                             0x0020
  RIVA TNT2/TNT2 Pro                   0x0028
  RIVA TNT2 Ultra                      0x0029
  Vanta/Vanta LT                       0x002C
  RIVA TNT2 Model 64/Model 64 Pro      0x002D
  GeForce 6800 Ultra                   0x0040
  GeForce 6800                         0x0041
  GeForce 6800 GT                      0x0045
  Quadro FX 4000                       0x004E
  Aladdin TNT2                         0x00A0
  GeForce 6800                         0x00C1
  GeForce 6800 LE                      0x00C2
  GeForce Go 6800                      0x00C8
  GeForce Go 6800 Ultra                0x00C9
  Quadro FX Go1400                     0x00CC
  Quadro FX Go1400                     0x00CC
  Quadro FX 3450/4000 SDI              0x00CD
  Quadro FX 1400                       0x00CE
  GeForce 6800/GeForce 6800 Ultra      0x00F0
  GeForce 6600/GeForce 6600 GT         0x00F1
  GeForce 6600                         0x00F2
  GeForce 6200                         0x00F3
  GeForce 6200                         0x00F3
  Quadro FX 3400                       0x00F8
  GeForce 6800 Ultra                   0x00F9
  GeForce PCX 5750                     0x00FA
  GeForce PCX 5900                     0x00FB
  Quadro FX 330/GeForce PCX 5300       0x00FC
  Quadro NVS 280 PCI-E/Quadro FX 330   0x00FD
  Quadro FX 1300                       0x00FE
  GeForce PCX 4300                     0x00FF
  GeForce 256                          0x0100
  GeForce DDR                          0x0101
  Quadro                               0x0103
  GeForce2 MX/MX 400                   0x0110
  GeForce2 MX 100/200                  0x0111
  GeForce2 Go                          0x0112
  Quadro2 MXR/EX/Go                    0x0113
  GeForce 6600 GT                      0x0140
  GeForce 6600                         0x0141
  GeForce Go 6600                      0x0144
  GeForce 6610 XL                      0x0145
  GeForce Go 6600 TE/6200 TE           0x0146
  GeForce Go 6600                      0x0148
  Quadro FX 540                        0x014E
  GeForce 6200                         0x014F
  GeForce2 GTS/GeForce2 Pro            0x0150
  GeForce2 Ti                          0x0151
  GeForce2 Ultra                       0x0152
  Quadro2 Pro                          0x0153
  GeForce 6200 TurboCache(TM)          0x0161
  GeForce 6200SE TurboCache(TM)        0x0162
  GeForce Go 6200                      0x0164
  GeForce Go 6250                      0x0166
  GeForce Go 6200                      0x0167
  GeForce Go 6250                      0x0168
  GeForce4 MX 460                      0x0170
  GeForce4 MX 440                      0x0171
  GeForce4 MX 420                      0x0172
  GeForce4 MX 440-SE                   0x0173
  GeForce4 440 Go                      0x0174
  GeForce4 420 Go                      0x0175
  GeForce4 420 Go 32M                  0x0176
  GeForce4 460 Go                      0x0177
  Quadro4 550 XGL                      0x0178
  GeForce4 440 Go 64M                  0x0179
  Quadro NVS                           0x017A
  Quadro4 500 GoGL                     0x017C
  GeForce4 410 Go 16M                  0x017D
  GeForce4 MX 440 with AGP8X           0x0181
  GeForce4 MX 440SE with AGP8X         0x0182
  GeForce4 MX 420 with AGP8X           0x0183
  GeForce4 MX 4000                     0x0185
  Quadro4 580 XGL                      0x0188
  Quadro NVS with AGP8X                0x018A
  Quadro4 380 XGL                      0x018B
  Quadro NVS 50 PCI                    0x018C
  GeForce2 Integrated GPU              0x01A0
  GeForce4 MX Integrated GPU           0x01F0
  GeForce3                             0x0200
  GeForce3 Ti 200                      0x0201
  GeForce3 Ti 500                      0x0202
  Quadro DCC                           0x0203
  GeForce 6800                         0x0211
  GeForce 6800 LE                      0x0212
  GeForce 6800 GT                      0x0215
  GeForce4 Ti 4600                     0x0250
  GeForce4 Ti 4400                     0x0251
  GeForce4 Ti 4200                     0x0253
  Quadro4 900 XGL                      0x0258
  Quadro4 750 XGL                      0x0259
  Quadro4 700 XGL                      0x025B
  GeForce4 Ti 4800                     0x0280
  GeForce4 Ti 4200 with AGP8X          0x0281
  GeForce4 Ti 4800 SE                  0x0282
  GeForce4 4200 Go                     0x0286
  Quadro4 980 XGL                      0x0288
  Quadro4 780 XGL                      0x0289
  Quadro4 700 GoGL                     0x028C
  GeForce FX 5800 Ultra                0x0301
  GeForce FX 5800                      0x0302
  Quadro FX 2000                       0x0308
  Quadro FX 1000                       0x0309
  GeForce FX 5600 Ultra                0x0311
  GeForce FX 5600                      0x0312
  GeForce FX 5600XT                    0x0314
  GeForce FX Go5600                    0x031A
  GeForce FX Go5650                    0x031B
  Quadro FX Go700                      0x031C
  GeForce FX 5200                      0x0320
  GeForce FX 5200 Ultra                0x0321
  GeForce FX 5200                      0x0322
  GeForce FX 5200LE                    0x0323
  GeForce FX Go5200                    0x0324
  GeForce FX Go5250                    0x0325
  GeForce FX 5500                      0x0326
  GeForce FX 5100                      0x0327
  GeForce FX Go5200 32M/64M            0x0328
  Quadro NVS 280 PCI                   0x032A
  Quadro FX 500/FX 600                 0x032B
  GeForce FX Go53xx                    0x032C
  GeForce FX Go5100                    0x032D
  GeForce FX 5900 Ultra                0x0330
  GeForce FX 5900                      0x0331
  GeForce FX 5900XT                    0x0332
  GeForce FX 5950 Ultra                0x0333
  GeForce FX 5900ZT                    0x0334
  Quadro FX 3000                       0x0338
  Quadro FX 700                        0x033F
  GeForce FX 5700 Ultra                0x0341
  GeForce FX 5700                      0x0342
  GeForce FX 5700LE                    0x0343
  GeForce FX 5700VE                    0x0344
  GeForce FX Go5700                    0x0347
  GeForce FX Go5700                    0x0348
  Quadro FX Go1000                     0x034C
  Quadro FX 1100                       0x034E


__________________________________________________________________________

(app-b) APPENDIX B: MINIMUM SOFTWARE REQUIREMENTS
__________________________________________________________________________

  o linux kernel     2.4.0    # cat /proc/version
  o XFree86          4.0.1    # XFree86 -version, or
    Xorg             6.7      # Xorg -version
  o Kernel modutils  2.1.121  # insmod -V

    If you need to build the NVIDIA kernel module:

  o binutils         2.9.5    # size --version
  o GNU make         3.77     # make --version
  o gcc              2.91.66  # gcc --version
  o glibc            2.0      # /lib/libc.so.6

    If you build from source rpms:

  o spec-helper rpm           # rpm -qi spec-helper

All official stable kernel releases from 2.4.0 and up are supported;
"prerelease" versions such as "2.4.3-pre2" are not supported, nor are
development series kernels such as 2.3.x or 2.5.x.  The linux kernel
can be downloaded from www.kernel.org or one of its mirrors.

binutils and gcc can be retrieved from www.gnu.org or one of its mirrors.

If you are using XFree86, but do not have a file /var/log/XFree86.0.log,
then you probably have a 3.x version of XFree86 and must upgrade.

If you are setting up XFree86 4.x for the first time, it is often easier
to begin with one of the open source drivers that ships with XFree86
(either 'nv', 'vga' or 'vesa').  Once XFree86 is operating properly with
the open source driver, then it is easier to switch to the nvidia driver.

Note that newer NVIDIA GPUs may not work with older versions of the "nv"
driver shipped with XFree86.  For example, the "nv" driver that shipped
with XFree86 version 4.0.1 did not recognize the GeForce2 family and
the Quadro2 MXR GPUs.  However, this was fixed in XFree86 version 4.0.2
(XFree86 can be retrieved from www.xfree86.org).

These software packages may also be available through your linux
distributor.


__________________________________________________________________________

(app-c) APPENDIX C: INSTALLED COMPONENTS
__________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set consists of the following
components (the file in parenthesis is the full name of the component
after installation; "x.y.z" denotes the current version -- in these
cases appropriate symlinks are created during installation):

  o An X driver (/usr/X11R6/lib/modules/drivers/nvidia_drv.so);
    this driver is needed by the X server to use your NVIDIA hardware.
    The nvidia_drv.so driver is binary compatible with XFree86 4.0.1 and
    greater, as well as the Xorg X server.

  o A GLX extension module for X
    (/usr/X11R6/lib/modules/extensions/libglx.so.x.y.z); this module is
    used by the X server to provide server-side glx support.

  o An OpenGL library (/usr/lib/libGL.so.x.y.z); this library
    provides the API entry points for all OpenGL and GLX function calls.
    It is linked to at run-time by OpenGL applications.

  o An OpenGL core library (/usr/lib/libGLcore.so.x.y.z); this
    library is implicitly used by libGL and by libglx.  It contains the
    core accelerated 3D functionality.  You should not explicitly load
    it in your X config file -- that is taken care of by libglx.

  o Two XvMC (X-Video Motion Compensation) libraries: a static library
    and a shared library (/usr/X11R6/lib/libXvMCNVIDIA.a,
    /usr/X11R6/lib/libXvMCNVIDIA.so.x.y.z); please see (app-p) APPENDIX P:
    XVMC SUPPORT for details.

  o A kernel module (/lib/modules/`uname -r`/video/nvidia.o
    or /lib/modules/`uname -r`/kernel/drivers/video/nvidia.o); this
    kernel module provides low-level access to your NVIDIA hardware for
    all of the above components.  It is generally loaded into the kernel
    when the X server is started, and is used by the X driver and OpenGL.
    nvidia.o consists of two pieces: the binary-only core, and a kernel
    interface that must be compiled specifically for your kernel version.
    Note that the linux kernel does not have a consistent binary interface
    like the X server, so it is important that this kernel interface be
    matched with the version of the kernel that you are using.  This can
    either be accomplished by compiling yourself, or using precompiled
    binaries provided for the kernels shipped with some of the more
    common linux distributions.

  o OpenGL and GLX header files
    (/usr/share/doc/NVIDIA_GLX-1.0/include/GL/gl.h, and
    /usr/share/doc/NVIDIA_GLX-1.0/include/GL/glx.h); these files can also
    be installed in /usr/include/GL/ by passing the "--opengl-headers"
    option to the .run file during installation.

  o The nvidia-tls libraries (/usr/lib/libnvidia-tls.so.x.y.z and
    /usr/lib/tls/libnvidia-tls.so.x.y.z); these files provide thread
    local storage support for the NVIDIA OpenGL libraries (libGL,
    libGLcore, and libglx).  Each nvidia-tls library provides support
    for a particular thread local storage model (such as ELF TLS),
    and the one appropriate for your system will be loaded at run time.

  o The application nvidia-installer (/usr/bin/nvidia-installer) is
    NVIDIA's tool for installing and updating NVIDIA drivers.  Please see
    (sec-02) INSTALLING THE NVIDIA DRIVER for a more thorough description.


Problems will arise if applications use the wrong version of a library.
This can be the case if there are either old libGL libraries or stale
symlinks left lying around.  If you think there may be something awry
in your installation, check that the following files are in place
(these are all the files of the NVIDIA Accelerated Linux Driver Set,
plus their symlinks):

        /usr/X11R6/lib/modules/drivers/nvidia_drv.so

        /usr/X11R6/lib/modules/extensions/libglx.so.x.y.z
        /usr/X11R6/lib/modules/extensions/libglx.so -> libglx.so.x.y.z

        /usr/lib/libGL.so.x.y.z
        /usr/lib/libGL.so.x -> libGL.so.x.y.z
        /usr/lib/libGL.so -> libGL.so.x

        /usr/lib/libGLcore.so.x.y.z
        /usr/lib/libGLcore.so.x -> libGLcore.so.x.y.z

        /lib/modules/`uname -r`/video/nvidia.o, or
        /lib/modules/`uname -r`/kernel/drivers/video/nvidia.o

If there are other libraries whose "soname" conflicts with that of
the NVIDIA libraries, ldconfig may create the wrong symlinks.  It is
recommended that you manually remove or rename conflicting libraries
(be sure to rename clashing libraries to something that ldconfig will
not look at -- we have found that prepending "XXX" to a library name
generally does the trick), rerun 'ldconfig', and check that the correct
symlinks were made.  Some libraries that often create conflicts are
"/usr/X11R6/lib/libGL.so*" and "/usr/X11R6/lib/libGLcore.so*".

If the libraries checks out, then verify that the application is using
the correct libraries.  For example, to check that the application
/usr/X11R6/bin/gears is using the NVIDIA libraries, you would do:

$ ldd /usr/X11R6/bin/gears
        libglut.so.3 => /usr/lib/libglut.so.3 (0x40014000)
        libGLU.so.1 => /usr/lib/libGLU.so.1 (0x40046000)
        libGL.so.1 => /usr/lib/libGL.so.1 (0x40062000)
        libc.so.6 => /lib/libc.so.6 (0x4009f000)
        libSM.so.6 => /usr/X11R6/lib/libSM.so.6 (0x4018d000)
        libICE.so.6 => /usr/X11R6/lib/libICE.so.6 (0x40196000)
        libXmu.so.6 => /usr/X11R6/lib/libXmu.so.6 (0x401ac000)
        libXext.so.6 => /usr/X11R6/lib/libXext.so.6 (0x401c0000)
        libXi.so.6 => /usr/X11R6/lib/libXi.so.6 (0x401cd000)
        libX11.so.6 => /usr/X11R6/lib/libX11.so.6 (0x401d6000)
        libGLcore.so.1 => /usr/lib/libGLcore.so.1 (0x402ab000)
        libm.so.6 => /lib/libm.so.6 (0x4048d000)
        libdl.so.2 => /lib/libdl.so.2 (0x404a9000)
        /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)
        libXt.so.6 => /usr/X11R6/lib/libXt.so.6 (0x404ac000)

Note the files being used for libGL and libGLcore -- if they are something
other than the NVIDIA libraries, then you will need to either remove the
libraries that are getting in the way, or adjust your ld search path.
If any of this seems foreign to you, then you may want to read the man
pages for "ldconfig" and "ldd" for pointers.


__________________________________________________________________________

(app-d) APPENDIX D: X CONFIG OPTIONS
__________________________________________________________________________

The following driver options are supported by the NVIDIA X driver.
They may be specified either in the Screen or Device sections of the X
config file.

        Option "NvAGP" "integer"
                Configure AGP support. Integer argument can be one of:
                0 : disable agp 
                1 : use NVIDIA's internal AGP support, if possible 
                2 : use AGPGART, if possible 
                3 : use any agp support (try AGPGART, then NVIDIA's AGP) 
                Please note that NVIDIA's internal AGP support cannot
                work if AGPGART is either statically compiled into your
                kernel or is built as a module, but loaded into your
                kernel (some distributions load AGPGART into the kernel
                at boot up).  Default: 3 (the default was 1 until after
                1.0-1251).

        Option "NoLogo" "boolean"
                Disable drawing of the NVIDIA logo splash screen at
                X startup.  Default: the logo is drawn.

        Option "RenderAccel" "boolean"
                Enable or disable hardware acceleration of the RENDER
                extension.  THIS OPTION IS EXPERIMENTAL.  ENABLE IT AT YOUR
                OWN RISK.  There is no correctness test suite for the
                RENDER extension so NVIDIA can not verify that RENDER
                acceleration works correctly.   Default: hardware 
                acceleration of the RENDER extension is disabled.

        Option "NoRenderExtension" "boolean"
                Disable the RENDER extension.  Other than recompiling
                the X-server, XFree86 does not seem to have another way of
                disabling this.  Fortunatly, we can control this from the
                driver so we export this option.  This is useful in depth
                8 where RENDER would normally steal most of the default
                colormap. Default: RENDER is offered when possible.

        Option "UBB" "boolean"
                Enable or disable Unified Back Buffer on any Quadro
                based GPUs (Quadro4 NVS excluded); please see
                Appendix M for a description of UBB.  This option has
                no affect on non-Quadro chipsets.  Default: UBB is on
                for Quadro chipsets.

        Option "NoFlip" "boolean"
                Disable OpenGL flipping; please see Appendix M for
                a description.  Default: OpenGL will swap by flipping
                when possible.

        Option "DigitalVibrance" "integer"
                Enables Digital Vibrance Control.  The range of valid
                values are 0 through 255.  This feature is not available
                on products older than GeForce2.  Default: 0.

        Option "Dac8Bit" "boolean"
                Most Quadro parts by default use a 10 bit color look
                up table (LUT) by default; setting this option to TRUE forces
                these graphics chips to use an 8 bit (LUT).  Default:
                a 10 bit LUT is used, when available.

        Option "Overlay" "boolean"
                Enables RGB workstation overlay visuals.  This is only
                supported on Quadro4 and Quadro FX chips (Quadro4 NVS
                excluded) in depth 24.  This option causes the server to
                advertise the SERVER_OVERLAY_VISUALS root window property
                and GLX will report single and double buffered, Z-buffered
                16 bit overlay visuals.  The transparency key is pixel
                0x0000 (hex).  There is no gamma correction support in
                the overlay plane.  This feature requires XFree86 version
                4.1.0 or newer (or the Xorg X server).  NV17/18 based
                Quadros (ie. 500/550 XGL) have additional restrictions,
                namely, overlays are not supported in TwinView mode
                or with virtual desktops larger than 2046x2047 in any
                dimension (eg.  it will not work in 2048x1536 modes).
                Quadro 7xx/9xx and Quadro FX will offer overlay visuals
                in these modes (TwinView, or virtual desktops larger
                than 2046x2047), but the overlay will be emulated with
                a substantial performance penalty.  RGB workstation
                overlays are not supported when the Composite extension is
                enabled.  Default: off.

        Option "CIOverlay" "boolean"
                Enables Color Index workstation overlay visuals with
                identical restrictions to Option "Overlay" above.
                The server will offer visuals both with and without a
                transparency key.  These are depth 8 PseudoColor visuals.
                Enabling Color Index overlays on X servers older than
                XFree86 4.3 will force the RENDER extension to be disabled
                due to bugs in the RENDER extension in older X servers.
                Color Index workstation overlays are not supported when the
                Composite extension is enabled.  Default: off.

        Option "TransparentIndex" "integer"
                When color index overlays are enabled, use this option
                to choose which pixel is used for the transparent pixel
                in visuals featuring transparent pixels.  This value
                is clamped between 0 and 255 (Note: some applications
                such as Alias's Maya require this to be zero
                in order to work correctly).  Default: 0.

        Option "OverlayDefaultVisual" "boolean"
                When overlays are used, this option sets the default
                visual to an overlay visual thereby putting the root
                window in the overlay.  This option is not recommended
                for RGB overlays.  Default: off.

        Option "RandRRotation" "boolean"
                Enable rotation support for the XRandR extension.  This
                allows use of the XRandR X server extension for
                configuring the screen orientation through rotation.
                This feature is supported on GeForce2 or better hardware
                using depth 24.  This requires an XOrg 6.8.1 or newer
                X server.  This feature does not work with hardware overlays,
                emulated overlays will be used instead at a substantial
                performance penalty.  See APPENDIX W for details.
                Default: off.

        Option "SWCursor" "boolean"
                Enable or disable software rendering of the X cursor.
                Default: off.

        Option "HWCursor" "boolean"
                Enable or disable hardware rendering of the X cursor.
                Default: on.

        Option "CursorShadow" "boolean" Enable or disable use of a
                shadow with the hardware accelerated cursor; this is a
                black translucent replica of your cursor shape at a
                given offset from the real cursor.  This option is
                only available on GeForce2 or better hardware (ie
                everything but TNT/TNT2, GeForce 256, GeForce DDR and
                Quadro).  Default: no cursor shadow.

        Option "CursorShadowAlpha" "integer"
                The alpha value to use for the cursor shadow; only
                applicable if CursorShadow is enabled.  This value must
                be in the range [0, 255] -- 0 is completely transparent;
                255 is completely opaque.  Default: 64.

        Option "CursorShadowXOffset" "integer"
                The offset, in pixels, that the shadow image will be
                shifted to the right from the real cursor image; only
                applicable if CursorShadow is enabled.  This value must
                be in the range [0, 32].  Default: 4.

        Option "CursorShadowYOffset" "integer"
                The offset, in pixels, that the shadow image will be
                shifted down from the real cursor image; only applicable
                if CursorShadow is enabled.  This value must be in the
                range [0, 32].  Default: 2.

        Option "ConnectedMonitor" "string"
                Allows you to override what the NVIDIA kernel module
                detects is connected to your video card.  This may
                be useful, for example, if you use a KVM (keyboard,
                video, mouse) switch and you are switched away when
                X is started.  In such a situation, the NVIDIA kernel
                module cannot detect what display devices are connected,
                and the NVIDIA X driver assumes you have a single CRT.

                Valid values for this option are "CRT" (cathode ray
                tube), "DFP" (digital flat panel), or "TV" (television);
                if using TwinView, this option may be a comma-separated
                list of display devices; e.g.: "CRT, CRT" or "CRT, DFP".

                NOTE: anything attached to a 15 pin VGA connector is
                regarded by the driver as a CRT.  "DFP" should only be
                used to refer to flatpanels connected via a DVI port.

                Default: string is NULL.

        Option "UseEdidFreqs" "boolean"
                This option causes the X server to use the HorizSync
                and VertRefresh ranges given in a display device's EDID,
                if any.  EDID provided range information will override
                the HorizSync and VertRefresh ranges specified in the
                Monitor section.  If a display device does not provide an
                EDID, or the EDID does not specify an hsync or vrefresh
                range, then the X server will default to the HorizSync
                and VertRefresh ranges specified in the Monitor section.

        Option "IgnoreEDID" "boolean"
                Disable probing of EDID (Extended Display Identification
                Data) from your monitor.  Requested modes are compared
                against values gotten from your monitor EDIDs (if any)
                during mode validation.  Some monitors are known to lie
                about their own capabilities.  Ignoring the values that
                the monitor gives may help get a certain mode validated.
                On the other hand, this may be dangerous if you do not
                know what you are doing.  Default: Use EDIDs.

        Option "NoDDC" "boolean"
                Synonym for "IgnoreEDID"

        Option "FlatPanelProperties" "string"
                Requests particular properties of any connected flat
                panels as a comma-separated list of property=value pairs.
                Currently, the only two available properties are 'Scaling'
                and 'Dithering'.   The possible values for 'Scaling' are:
                'default' (the driver will use whatever scaling state
                is current), 'native' (the driver will use the flat
                panel's scaler, if it has one), 'scaled' (the driver
                will use the NVIDIA scaler, if possible), 'centered'
                (the driver will center the image, if possible),
                and 'aspect-scaled' (the driver will scale with the
                NVIDIA scaler, but keep the aspect ratio correct).
                The possible values for 'Dithering' are: 'default'
                (the driver will decide when to dither), 'enabled' (the
                driver will always dither when possible), and 'disabled'
                (the driver will never dither).  If any property is not
                specified, it's value shall be 'default'.  An example
                properties string might look like:

                "Scaling = centered, Dithering = enabled"

        Option "UseInt10Module" "boolean"
                Enable use of the X Int10 module to soft-boot all
                secondary cards, rather than POSTing the cards through
                the NVIDIA kernel module.  Default: off (POSTing is done
                through the NVIDIA kernel module).

        Option "TwinView" "boolean"
                Enable or disable TwinView.  Please see APPENDIX I for
                details. Default: TwinView is disabled.

        Option "TwinViewOrientation" "string"
                Controls the relationship between the two display devices
                when using TwinView.  Takes one of the following values:
                "RightOf" "LeftOf" "Above" "Below" "Clone".  Please see
                APPENDIX I for details. Default: string is NULL.

        Option "SecondMonitorHorizSync" "range(s)"
                This option is like the HorizSync entry in the Monitor
                section, but is for the second monitor when using
                TwinView.  Please see APPENDIX I for details. Default:
                none.

        Option "SecondMonitorVertRefresh" "range(s)"
                This option is like the VertRefresh entry in the Monitor
                section, but is for the second monitor when using
                TwinView.  Please see APPENDIX I for details. Default:
                none.

        Option "MetaModes" "string"
                This option describes the combination of modes to use
                on each monitor when using TwinView. Please see APPENDIX
                I for details. Default: string is NULL.

        Option "NoTwinViewXineramaInfo" "boolean"
                When in TwinView, the NVIDIA X driver normally provides
                a Xinerama extension that X clients (such as window
                managers) can use to to discover the current TwinView
                configuration.  Some window mangers can get confused by
                this information, so this option is provided to disable
                this behavior.  Default: TwinView Xinerama information
                is provided.

        Option "TVStandard" "string"
                Please see (app-j)  APPENDIX J: CONFIGURING TV-OUT.

        Option "TVOutFormat" "string"
                Please see (app-j)  APPENDIX J: CONFIGURING TV-OUT.

        Option "TVOverScan" "Decimal value in the range 0.0 to 1.0"
                Valid values are in the range 0.0 through 1.0; please see
                (app-j)  APPENDIX J: CONFIGURING TV-OUT.

        Option "Stereo" "integer"
                Enable offering of quad-buffered stereo visuals on Quadro.
                Integer indicates the type of stereo glasses being used:
 
                1 - DDC glasses.  The sync signal is sent to the glasses
                    via the DDC signal to the monitor.  These usually
                    involve a passthrough cable between the monitor and
                    video card.

                2 - "Blueline" glasses.  These usually involve
                    a passthrough cable between the monitor and video
                    card.  The glasses know which eye to display based
                    on the length of a blue line visible at the bottom
                    of the screen.  When in this mode, the root window
                    dimensions are one pixel shorter in the Y dimension
                    than requested.  This mode does not work with virtual
                    root window sizes larger than the visible root window
                    size (desktop panning).

                3 - Onboard stereo support.  This is usually only found
                    on professional cards.  The glasses connect via a
                    DIN connector on the back of the video card.

                4 - TwinView clone mode stereo (aka "passive" stereo).
                    On video cards that support TwinView, the left eye
                    is displayed on the first display, and the right
                    eye is displayed on the second display.  This is
                    normally used in conjuction with special projectors
                    to produce 2 polarized images which are then viewed
                    with polarized glasses.  To use this stereo mode,
                    you must also configure TwinView in clone mode with
                    the same resolution, panning offset, and panning
                    domains on each display.

                Stereo is only available on Quadro cards.  Stereo
                options 1, 2, and 3 (aka "active" stereo) may be used
                with TwinView if all modes within each metamode have
                identical timing values.  Please see (app-l)  APPENDIX
                L: PROGRAMMING MODES for suggestions on making sure the
                modes within your metamodes are identical.  The identical
                modeline requirement is not necessary for Stereo option 4
                ("passive" stereo).  Currently, stereo operation may
                be "quirky" on the original Quadro (NV10) chip and
                left-right flipping may be erratic.  We are trying
                to resolve this issue for a future release.  Default:
                Stereo is not enabled.

                Stereo options 1, 2, and 3 (aka "active" stereo) are not
                supported on Digital Flatpanels.

        Option "AllowDFPStereo" "boolean"
                By default, the NVIDIA X driver performs a check which
                disables active stereo (stereo options 1, 2, and 3)
                if the X screen is driving a DFP.  The "AllowDFPStereo"
                option bypasses this check.

        Option "NoBandWidthTest" "boolean"
                As part of mode validation, the X driver tests if a
                given mode fits within the hardware's memory bandwidth
                constraints.  This option disables this test.  Default:
                the memory bandwidth test is performed.

        Option "IgnoreDisplayDevices" "string"
                This option tells the NVIDIA kernel module to completely
                ignore the indicated classes of display devices when
                checking what display devices are connected.  You may
                specify a comma-separated list containing any of "CRT",
                "DFP", and "TV".

                For example:

                    Option "IgnoreDisplayDevices" "DFP, TV"

                will cause the NVIDIA driver to not attempt to detect
                if any flatpanels or TVs are connected.

                This option is not normally necessary; however, some video
                BIOSes contain incorrect information about what display
                devices may be connected, or what i2c port should be
                used for detection.  These errors can cause long delays
                in starting X.  If you are experiencing such delays, you
                may be able to avoid this by telling the NVIDIA driver to
                ignore display devices which you know are not connected.

                NOTE: anything attached to a 15 pin VGA connector is
                regarded by the driver as a CRT.  "DFP" should only be
                used to refer to flatpanels connected via a DVI port.

        Option "MultisampleCompatibility" "boolean"
                Enable or disable the use of separate front and back
                multisample buffers.  This will consume more memory
                but is necessary for correct output when rendering to
                both the front and back buffers of a multisample or
                FSAA drawable.  This option is necessary for correct
                operation of SoftImage XSI.  Default: a singlemultisample
                buffer is shared between the front and back buffers.

        Option "NoPowerConnectorCheck" "boolean"
                The NVIDIA X driver will abort X server initialization
                if it detects that a GPU that requires an external power
                connector does not have an external power connector
                plugged in.  This option can be used to bypass this test.
                Default: the power connector test is performed.

        Option "XvmcUsesTextures" "boolean"
                Forces XvMC to use the 3D engine for XvMCPutSurface
                requests rather than the video overlay.  Default: video
                overlay is used when available.

        Option "AllowGLXWithComposite" "boolean"
                Enables GLX even when the Composite X extension is loaded.
                ENABLE AT YOUR OWN RISK.  OpenGL applications will not
                display correctly in many circumstances with this setting
                enabled.  Default: GLX is disabled when Composite is
                loaded.

        Option "ExactModeTimingsDVI" "boolean"
                Forces the initialization of the X server with the exact 
                timings specified in the ModeLine. Default: For DVI 
                devices, the X server inilializes with the closest mode in 
                the EDID list.

        Option "LoadKernelModule" "boolean"
                By default, the NVIDIA Linux X driver module will attempt
                to load the NVIDIA Linux kernel module. Set this option
                to "off" to disable automatic loading of the NVIDIA kernel
                module by the NVIDIA X driver.

__________________________________________________________________________

(app-e) APPENDIX E: OPENGL ENVIRONMENT VARIABLE SETTINGS
__________________________________________________________________________

FULL SCENE ANTIALIASING

Antialiasing is a technique used to smooth the edges of objects in a
scene to reduce the jagged "stairstep" effect that sometimes appears.
Full-scene antialiasing is supported on GeForce or newer hardware.
By setting the appropriate environment variable, you can enable full-scene
antialiasing in any OpenGL application on these GPUs.

Several anti-aliasing methods are available and you can select between
them by setting the __GL_FSAA_MODE environment variable appropriately.
Note that increasing the number of samples taken during FSAA rendering
may decrease performance.

The following tables describe the possible values for __GL_FSAA_MODE
and their effect on various NVIDIA GPUs.

__GL_FSAA_MODE  GeForce, GeForce2, Quadro, and Quadro2 Pro
-----------------------------------------------------------------------
  0             FSAA disabled
  1             FSAA disabled
  2             FSAA disabled
  3             1.5 x 1.5 Supersampling
  4             2 x 2 Supersampling
  5             FSAA disabled
  6             FSAA disabled
  7             FSAA disabled



__GL_FSAA_MODE  GeForce4 MX, GeForce4 4xx Go, Quadro4 380,550,580 XGL,
                and Quadro4 NVS
-----------------------------------------------------------------------
  0             FSAA disabled
  1             2x Bilinear Multisampling
  2             2x Quincunx Multisampling
  3             FSAA disabled
  4             2 x 2 Supersampling
  5             FSAA disabled
  6             FSAA disabled
  7             FSAA disabled


__GL_FSAA_MODE  GeForce3, Quadro DCC, GeForce4 Ti, GeForce4 4200 Go,
                and Quadro4 700,750,780,900,980 XGL
-----------------------------------------------------------------------
  0             FSAA disabled
  1             2x Bilinear Multisampling
  2             2x Quincunx Multisampling
  3             FSAA disabled
  4             4x Bilinear Multisampling
  5             4x Gaussian Multisampling
  6             2x Bilinear Multisampling by 4x Supersampling
  7             FSAA disabled


__GL_FSAA_MODE  GeForce FX, Quadro FX
-----------------------------------------------------------------------
  0             FSAA disabled
  1             2x Bilinear Multisampling
  2             2x Quincunx Multisampling
  3             FSAA disabled
  4             4x Bilinear Multisampling
  5             4x Gaussian Multisampling
  6             2x Bilinear Multisampling by 4x Supersampling
  7             4x Bilinear Multisampling by 4x Supersampling
  8             4x Bilinear Multisampling by 2x Supersampling
               (available on GeForce FX and later GPUS; not available
                on Quadro GPUs)


ANISOTROPIC TEXTURE FILTERING

Automatic anisotropic texture filtering can be enabled by setting 
the environment variable __GL_LOG_MAX_ANISO.  The possible values 
are:

  0     No anisotropic filtering
  1     2x anisotropic filtering
  2     4x anisotropic filtering
  3     8x anisotropic filtering
  4     16x anisotropic filtering

4x and greater are only available on GeForce3 or newer GPUS; 16x is only
available on GeForce 6800 or newer GPUs.


VBLANK SYNCING

Setting the environment variable __GL_SYNC_TO_VBLANK to a non-zero value
will force glXSwapBuffers to sync to your monitor's vertical refresh rate
(perform a swap only during the vertical blanking period).

When using __GL_SYNC_TO_VBLANK with TwinView, OpenGL can only sync to one
of the display devices; this may cause tearing corruption on the display
device to which OpenGL is not syncing.  You can use the environment
variable __GL_SYNC_DISPLAY_DEVICE to specify to which display device
OpenGL should sync.  You should set this environment variable to the
name of a display device; for example "CRT-1".  Please look for the line
"Connected display device(s):" in your X log file for a list of the
display devices present and their names.


DISABLING CPU SPECIFIC FEATURES

Setting the environment variable __GL_FORCE_GENERIC_CPU to a non-zero
value will inhibit the use of CPU specific features such as MMX, SSE,
or 3DNOW!.  Use of this option may result in performance loss.  This
option may be useful in conjunction with software such as the Valgrind
memory debugger.

__________________________________________________________________________

(app-f) APPENDIX F: CONFIGURING AGP
__________________________________________________________________________

There are several choices for configuring the NVIDIA kernel module's
use of AGP on Linux. You can choose to either use NVIDIA's builtin
AGP driver (NvAGP), or the AGP driver that comes with the Linux kernel
(AGPGART). This is controlled through the "NvAGP" option in your X
config file:

    Option "NvAGP" "0"  ... disables AGP support
    Option "NvAGP" "1"  ... use NvAGP, if possible
    Option "NvAGP" "2"  ... use AGPGART, if possible
    Option "NvAGP" "3"  ... try AGPGART; if that fails, try NvAGP

The default is 3 (the default was 1 until after 1.0-1251).

You should use the AGP driver that works best with your AGP chipset.
If you are experiencing problems with stability, you may want to start
by disabling AGP and seeing if that solves the problems. Then you
can experiment with the AGP driver configuration.

You can query the current AGP status at any time via the '/proc'
filesystem interface (see Appendix M).

To use the Linux 2.4 AGPGART driver, you will need to compile it with
your kernel and either statically link it in, or build it as a
module and load it. To use the Linux 2.6 AGPGART driver, both the
AGPGART frontend module, 'apggart.ko', and the backend module for your
AGP chipset ('nvidia-agp.ko', 'intel-agp.ko', 'via-agp.ko', ...) need to
be statically linked into the kernel, or built as modules and loaded.
NVIDIA's builtin AGP support is unavailable if an AGPGART backend driver
is loaded into the kernel. On Linux 2.4, it is recommended that you
compile AGPGART as a module and make sure that it is not loaded
when trying to use NVIDIA's AGP driver. On Linux 2.6, the 'agpgart.ko'
frontend module will always be loaded, as it is used by the NVIDIA
kernel module to determine if an AGPGART backend module is loaded. When
the NVIDIA AGP driver is to be used on a Linux 2.6 system, it is
recommended that you make sure the AGPGART backend drivers are built as
modules and that they are not loaded.

Please also note that changing AGP drivers generally requires a reboot
before the changes actually take effect.

If you are using a recent Linux 2.6 kernel that has the Linux AGPGART
driver statically linked in (some distribution kernels do), you can pass
the

    agp=off

parameter to the kernel (via LILO or GRUB, for example) to disable
AGPGART support. As of Linux 2.6.11, most AGPGART backend drivers should
respect this parameter.

The following AGP chipsets are supported by NVIDIA's AGP; for all other
chipsets it is recommended that you use the AGPGART module.

  o Intel 440LX
  o Intel 440BX
  o Intel 440GX
  o Intel 815 ("Solano")   
  o Intel 820 ("Camino")   
  o Intel 830
  o Intel 840 ("Carmel")   
  o Intel 845 ("Brookdale")
  o Intel 845G
  o Intel 850 ("Tehama")
  o Intel 855 ("Odem")
  o Intel 860 ("Colusa")
  o Intel 865G ("Springdale")
  o Intel 875P ("Canterwood")
  o Intel E7205 ("Granite Bay")
  o Intel E7505 ("Placer")
  o AMD 751 ("Irongate")
  o AMD 761 ("IGD4")   
  o AMD 762 ("IGD4 MP")
  o AMD 8151 ("Lokar")
  o VIA 8371   
  o VIA 82C694X
  o VIA KT133
  o VIA KT266
  o VIA KT400
  o VIA P4M266
  o VIA P4M266A
  o VIA P4X400
  o VIA K8T800
  o RCC CNB20LE
  o RCC 6585HE
  o Micron SAMDDR ("Samurai") 
  o Micron SCIDDR ("Scimitar")
  o NVIDIA nForce
  o NVIDIA nForce2
  o NVIDIA nForce3
  o ALi 1621
  o ALi 1631
  o ALi 1647
  o ALi 1651
  o ALi 1671
  o SiS 630
  o SiS 633
  o SiS 635
  o SiS 645
  o SiS 646
  o SiS 648
  o SiS 648FX
  o SiS 650
  o SiS 655FX
  o SiS 730
  o SiS 733
  o SiS 735
  o SiS 745
  o SiS 755
  o ATI RS200M


If you are experiencing AGP stability problems, you should be aware of
the following:

  o Support for the processor's Page Size Extension on Athlon Processors

    Some linux kernels have a conflicting cache attribute bug that is 
    exposed by advanced speculative caching in newer AMD Athlon family 
    processors (AMD Athlon XP, AMD Athlong 4, AMD Athlon MP, and Models 6 
    and above AMD Duron). This kernel bug usually shows up under heavy use
    of accelerated 3D graphics with an AGP graphics card.

    Linux distributions based on kernel 2.4.19 and later *should* 
    incorporate the bug fix. But, older kernels require help from the user
    in ensuring that a small portion of advanced speculative caching is 
    disabled (normally done through a kernel patch) and a boot option is
    specified in order to apply the whole fix.

    NVIDIA's driver automatically disables the small portion of advanced
    speculative caching for the affected AMD processors without the need
    to patch the kernel; it can be used even on kernels which do already
    incorporate the kernel bug fix. Additionally, for older kernels the
    user performs the boot option portion of the fix by explicitly disabling
    4MB pages. This can be done from the boot command line by specifying:

        mem=nopentium

    Or by adding the following line to etc/lilo.conf:

        append = "mem=nopentium"

  o AGP drive strength BIOS setting (Via based mainboards)

    Many Via based mainboards allow adjusting the AGP drive strength in
    the system BIOS. The setting of this option largely affects system
    stability, the range between 0xEA and 0xEE seems to work best for
    NVIDIA hardware. Setting either nibble to 0xF generally restults in
    severe stability problems.

    If you decide to experiment with this, you need to be aware of
    the fact that you are doing so at your own risk and that you may
    render your system unbootable with improper settings until you
    reset the setting to a working value (w/ a PCI graphics card or
    by resetting the BIOS to its default values).

  o System BIOS version

    Make sure to have the latest system BIOS provided by the board
    manufacturer.

  o AGP Rate

    You may want to decrease the AGP rate setting if you are seeing
    lockups with the value you are currently using. You can do so by
    extracting the .run file:

        sh NVIDIA-Linux-x86_64-71.86.15-pkg1.run --extract-only
        cd NVIDIA-Linux-x86_64-71.86.15-pkg1/usr/src/nv/

    Then edit os-registry.c, and make the following changes:

        - static int NVreg_ReqAGPRate = 7;
        + static int NVreg_ReqAGPRate = 4;   /* force AGP Rate to 4x */
    or
        + static int NVreg_ReqAGPRate = 2;   /* force AGP Rate to 2x */
    or
        + static int NVreg_ReqAGPRate = 1;   /* force AGP Rate to 1x */

    and enable the "ReqAGPRate" parameter:

        - { NULL, "ReqAGPRate",     &NVreg_ReqAGPRate,      0 },
        + { NULL, "ReqAGPRate",     &NVreg_ReqAGPRate,      1 },

    Then recompile and load the new kernel module.


On Athlon motherboards with the VIA KX133 or 694X chip set, such as the
ASUS K7V motherboard, NVIDIA drivers default to AGP 2x mode to work around
insufficient drive strength on one of the signals.  You can force AGP 4x
by setting NVreg_EnableVia4x to 1.  Note that this may cause the system
to become unstable.

On ALi1541 and ALi1647 chipsets, NVIDIA drivers disable AGP to work
around timing issues and signal integrity issues. You can force AGP
to be enabled on these chipsets by setting NVreg_EnableALiAGP to 1.
Note that this may cause the system to become unstable.

Early SBIOS revisions for the ASUS A7V8X-X KT400 motherboard misconfigure
the chipset when an AGP 2.x graphics card is installed; if X hangs on
your ASUS KT400 system with either Linux AGPGART or NvAGP enabled and the
installed graphics card is not an AGP 8x device, make sure that you have
the lastest SBIOS installed.


__________________________________________________________________________

(app-g) APPENDIX G: ALI SPECIFIC ISSUES
__________________________________________________________________________

The following tips may help stabilize problematic ALI systems:

  o Disable TURBO AGP MODE in the BIOS.
 
  o When using a P5A upgrade to BIOS Revision 1002 BETA 2.
 
  o When using 1007, 1007A or 1009 adjust the IO Recovery Time to
    4 cycles.

  o AGP is disabled by default on some ALi chipsets (ALi1541, ALi1647)
    to work around severe system stability problems with these chipsets.
    See the comments for NVreg_EnableALiAGP in os-registry.c to force
    AGP on anyway.


__________________________________________________________________________

(app-h) APPENDIX H: TNT SPECIFIC ISSUES
__________________________________________________________________________

Most issues pertaining to SGRAM/SDRAM TNT cards should be resolved.
There is the rare chance, however, that your video card has the wrong
BIOS installed, and that this driver will continue to fail for you.

If this driver fails for you, do the following:

  o watch your monitor as the system boots. The very first, brief screen
    will identify the type of video memory your card has. This will be
    either SGRAM or SDRAM.

  o edit the file "os-registry.c" from the kernel module sources.  Look
    for the variable "NVreg_VideoMemoryTypeOverride".  Set the value of
    the variable to the type of memory you have (numerically, see the
    line just above it).

  o since we do not normally use this variable, change the "#if 0" that is
    about 10 lines above the variable to "#if 1".

  o rebuild and reinstall the new driver ("make")


__________________________________________________________________________

(app-i) APPENDIX I: CONFIGURING TWINVIEW
__________________________________________________________________________

The TwinView feature is only supported on NVIDIA GPUs that support
dual-display functionality, such as the GeForce2 MX, GeForce2 Go,
Quadro2 MXR, Quadro2 Go, and any of the GeForce4, Quadro4, GeForce
FX, or Quadro FX GPUs.  Please consult with your video card vendor
to confirm that TwinView is supported on your card.

TwinView is a mode of operation where two display devices (digital
flat panels, CRTs, and TVs) can display the contents of a single X screen
in any arbitrary configuration.  This method of multiple monitor use
has several distinct advantages over other techniques (such as Xinerama):

  o A single X screen is used.  The NVIDIA driver conceals all
    information about multiple display devices from the X server; as
    far as X is concerned, there is only one screen.

  o Both display devices share one frame buffer.  Thus, all the
    the functionality present on a single display (e.g. accelerated
    OpenGL) is available on TwinView.

  o No additional overhead is needed to emulate having a single
    desktop.

If you are interested in using each display device as a separate
X screen, please see (app-r)  APPENDIX R: CONFIGURING MULTIPLE X
SCREENS ON ONE CARD.


X CONFIG TWINVIEW OPTIONS

To enable TwinView, you must specify the following options in the Device
section of your X Config file:

    Option "TwinView"
    Option "MetaModes"                  "<list of metamodes>"

You must also specify either:

    Option "SecondMonitorHorizSync"     "<hsync range(s)>"
    Option "SecondMonitorVertRefresh"   "<vrefresh range(s)>"

or:

    Option "HorizSync"                  "<hsync range(s)>"
    Option "VertRefresh"                "<vrefresh range(s)>"

You may also use any of the following options, though they are not
required:

    Option "TwinViewOrientation"        "<relationship of head 1 to head 0>"
    Option "ConnectedMonitor"           "<list of connected display devices>"

Please see the detailed descriptions of each option below:

  o TwinView
        This option is required to enable TwinView; without it, all
        other TwinView related options are ignored.

  o SecondMonitorHorizSync, SecondMonitorVertRefresh
        You specify the constraints of the second monitor through these
        options.  The values given should follow the same convention as
        the "HorizSync" and "VertRefresh" entries in the Monitor section.
        As the XF86Config man page explains it: the ranges may be a
        comma separated list of distinct values and/or ranges of values,
        where a range is given by two distinct values separated by
        a dash.  The HorizSync is given in kHz, and the VertRefresh
        is given in Hz.  You may, if you trust your display devices'
        EDIDs, use the "UseEdidFreqs" option instead of these options
        (see APPENDIX D for a description of the "UseEdidFreqs" option).

  o HorizSync, VertRefresh
        Which display device is "first" and which is "second" is often
        unclear.  For this reason, you may use these options instead of
        the SecondMonitor versions.  With these options, you can specify
        a semicolon-separated list of frequency ranges, each optionally
        prepended with a display device name.  For example:

          Option "HorizSync"   "CRT-0: 50-110;  DFP-0: 40-70"
          Option "VertRefresh" "CRT-0: 60-120;  DFP-0: 60"

        Please see the Appendix on Display Device Names for more
        information.

  o MetaModes
        A single MetaMode describes what mode should be used on each
        display device at a given time.  Multiple MetaModes list the
        combinations of modes and the sequence in which they should be
        used.  When the NVIDIA driver tells X what modes are available,
        it is really the minimal bounding box of the MetaMode that is
        communicated to X, while the "per display device" mode is kept
        internal to the NVIDIA driver.  In MetaMode syntax, modes within
        a MetaMode are comma separated, and multiple MetaModes are
        separated by semicolons.  For example:

          "<mode name 0>, <mode name 1>; <mode name 2>, <mode name 3>"

        Where <mode name 0> is the name of the mode to be used on display
        device 0 concurrently with <mode name 1> used on display device 1.
        A mode switch will then cause <mode name 2> to be used on display
        device 0 and <mode name 3> to be used on display device 1.  Here
        is a real MetaMode entry from the X config sample config file:

          Option "MetaModes" "1280x1024,1280x1024; 1024x768,1024x768"

        If you want a display device to not be active for a certain
        MetaMode, you can use the mode name "NULL", or simply omit the
        mode name entirely:

          "1600x1200, NULL; NULL, 1024x768"

        or

          "1600x1200; , 1024x768"

        Optionally, mode names can be followed by offset information
        to control the positioning of the display devices within the
        virtual screen space; e.g.:

          "1600x1200 +0+0, 1024x768 +1600+0; ..."

        Offset descriptions follow the conventions used in the X
        "-geometry" command line option; i.e. both positive and negative
        offsets are valid, though negative offsets are only allowed when
        a virtual screen size is explicitly given in the X config file.

        When no offsets are given for a MetaMode, the offsets will be
        computed following the value of the TwinViewOrientation option
        (see below).  Note that if offsets are given for any one of the
        modes in a single MetaMode, then offsets will be expected for
        all modes within that single MetaMode; in such a case offsets
        will be assumed to be +0+0 when not given.

        When not explicitly given, the virtual screen size will be
        computed as the the bounding box of all MetaMode bounding boxes.
        MetaModes with a bounding box larger than an explicitly given
        virtual screen size will be discarded.

        A MetaMode string can be further modified with a "Panning Domain"
        specification; eg:

          "1024x768 @1600x1200, 800x600 @1600x1200"

        A panning domain is the area in which a display device's viewport
        will be panned to follow the mouse.  Panning actually happens on
        two levels with TwinView: first, an individual display device's
        viewport will be panned within its panning domain, as long as
        the viewport is contained by the bounding box of the MetaMode.
        Once the mouse leaves the bounding box of the MetaMode, the entire
        MetaMode (ie all display devices) will be panned to follow the
        mouse within the virtual screen.  Note that individual display
        devices' panning domains default to being clamped to the position
        of the display devices' viewports, thus the default behavior is
        just that viewports remain "locked" together and only perform
        the second type of panning.

        The most beneficial use of panning domains is probably to
        eliminate dead areas -- regions of the virtual screen that are
        inaccessible due to display devices with different resolutions.
        For example:

          "1600x1200, 1024x768"

        produces an inaccessible region below the 1024x768
        display. Specifying a panning domain for the second display
        device:

          "1600x1200, 1024x768 @1024x1200"

        provides access to that dead area by allowing you to pan the
        1024x768 viewport up and down in the 1024x1200 panning domain.

        Offsets can be used in conjunction with panning domains to
        position the panning domains in the virtual screen space (note
        that the offset describes the panning domain, and only affects
        the viewport in that the viewport must be contained within the
        panning domain).  For example, the following describes two modes,
        each with a panning domain width of 1900 pixels, and the second
        display is positioned below the first:

          "1600x1200 @1900x1200 +0+0, 1024x768 @1900x768 +0+1200"

        Because it is often unclear which mode within a MetaMode will be
        used on each display device, mode descriptions within a MetaMode
        can be prepended with a display device name.  For example:

          "CRT-0: 1600x1200,  DFP-0: 1024x768"

        If no MetaMode string is specified, then the X driver uses the
        modes listed in the relevant "Display" subsection, attempting
        to place matching modes on each display device.


  o TwinViewOrientation
        This option controls the positioning of the second display
        device relative to the first within the virtual X screen, when
        offsets are not explicitly given in the MetaModes.  The possible
        values are:

          "RightOf"  (the default)
          "LeftOf"
          "Above"
          "Below"
          "Clone"
 
        When "Clone" is specified, both display devices will be assigned
        an offset of 0,0.

        Because it is often unclear which display device is "first"
        and which is "second", TwinViewOrientation can be confusing.
        You can further clarify the TwinViewOrientation with display
        device names to indicate which display device is positioned
        relative to which display device.  For example:

          "CRT-0 LeftOf DFP-0"

  o ConnectedMonitor
        With this option you can override what the NVIDIA kernel
        module detects is connected to your video card.  This may be
        useful, for example, if any of your display devices do not
        support detection using Display Data Channel (DDC) protocols.
        Valid values are a comma-separated list of display device names;
        for example:

          "CRT-0, CRT-1"
          "CRT"
          "CRT-1, DFP-0"

        WARNING: this option overrides what display devices are
        detected by the NVIDIA kernel module, and is very seldom needed.
        You really only need this if a display device is not detected,
        either because it does not provide DDC information, or because
        it is on the other side of a KVM (Keyboard-Video-Mouse) switch.
        In most other cases, it is best not to specify this option.

Just as in all X config entries, spaces are ignored and all entries
are case insensitive.


FREQUENTLY ASKED TWINVIEW QUESTIONS:
 

Q: Nothing gets displayed on my second monitor; what is wrong?
 
A: Monitors that do not support monitor detection using Display Data
   Channel (DDC) protocols (this includes most older monitors) are not
   detectable by your NVIDIA card.  You need to explicitly tell the
   NVIDIA X driver what you have connected using the "ConnectedMonitor"
   option; e.g.:

        Option "ConnectedMonitor" "CRT, CRT"


Q: Will window managers be able to appropriately place windows
   (e.g. avoiding placing windows across both display devices, or in
   inaccessible regions of the virtual desktop)?

A: Yes.  The NVIDIA X driver provides a Xinerama extension that X clients
   (such as window managers) can use to discover the current TwinView
   configuration.  Note that the Xinerama protocol provides no way to
   inform clients of when a configuration change occurs.  So, if you
   modeswitch to a different MetaMode, your window manager will still
   think you have the previous configuration.  Using the Xinerama
   extension, in conjunction with the XF86VidMode extension to get
   modeswitch events, window managers should be able to determine the
   TwinView configuration at any given time.

   Unfortunately, the data provided by XineramaQueryScreens() appears to
   confuse some window managers; to workaround such broken window mangers,
   you can disable communication of the TwinView screen layout with the
   "NoTwinViewXineramaInfo" X config Option (please see Appendix D
   for details).

   Be aware that the NVIDIA driver cannot provide the Xinerama
   extension if the X server's own Xinerama extension is being used.
   Explicitly specifying Xinerama in the X config file or on the X server
   commandline will prohibit NVIDIA's Xinerama extension from installing,
   so make sure that the X server's log file does not contain:

     (++) Xinerama: enabled

   if you wish the NVIDIA driver to be able to provide the Xinerama 
   extension while in TwinView.

   Another solution is to use panning domains to eliminate inaccessible
   regions of the virtual screen (see the MetaMode description above).

   A third solution is to use two separate X screens, rather than use
   TwinView.  Please see (app-r) APPENDIX R: CONFIGURING MULTIPLE X
   SCREENS ON ONE CARD.


Q: Why can I not get a resolution of 1600x1200 on the second display
   device when using a GeForce2 MX?

A: Because the second display device on the GeForce2 MX was designed to
   be a digital flat panel, the Pixel Clock for the second display device
   is only 150 MHz.  This effectively limits the resolution on the second
   display device to somewhere around 1280x1024 (for a description of
   how Pixel Clock frequencies limit the programmable modes, see the
   XFree86 Video Timings HOWTO).  This constraint is not present on
   GeForce4 or GeForce FX chips -- the maximum pixel clock is the same i
   on both heads.


Q: Do video overlays work across both display devices?

A: Hardware video overlays only work on the first display device.
   The current solution is that blitted video is used instead on TwinView.


Q: How are virtual screen dimensions determined in TwinView?
 
A: After all requested modes have been validated, and the offsets
   for each MetaMode's viewports have been computed, the NVIDIA driver
   computes the bounding box of the panning domains for each MetaMode.
   The maximum bounding box width and height is then found.

   Note that one side effect of this is that the virtual width and
   virtual height may come from different MetaModes.  Given the following
   MetaMode string:

        "1600x1200,NULL; 1024x768+0+0, 1024x768+0+768"

   the resulting virtual screen size will be 1600 x 1536.


Q: Can I play full screen games across both display devices?

A: Yes.  While the details of configuration will vary from game to game,
   the basic idea is that a MetaMode presents X with a mode whose
   resolution is the bounding box of the viewports for that MetaMode.
   For example, the following:

        Option "MetaModes" "1024x768,1024x768; 800x600,800x600"
        Option "TwinViewOrientation" "RightOf"

   produce two modes: one whose resolution is 2048x768, and another whose
   resolution is 1600x600.  Games such as Quake 3 Arena use the VidMode
   extension to discover the resolutions of the modes currently available.
   To configure Quake 3 Arena to use the above MetaMode string, add the
   following to your q3config.cfg file:

        seta r_customaspect "1"
        seta r_customheight "600"
        seta r_customwidth  "1600"
        seta r_fullscreen   "1"
        seta r_mode         "-1"

   Note that, given the above configuration, there is no mode with a
   resolution of 800x600 (remember that the MetaMode "800x600, 800x600"
   has a resolution of 1600x600"), so if you change Quake 3 Arena to use
   a resolution of 800x600, it will display in the lower left corner of
   your screen, with the rest of the screen grayed out.  To have single
   head modes available as well, an appropriate MetaMode string might
   be something like:

        "800x600,800x600; 1024x768,NULL; 800x600,NULL; 640x480,NULL"

   More precise configuration information for specific games is beyond the
   scope of this document, but the above examples coupled with numerous
   online sources should be enough to point you in the right direction.


__________________________________________________________________________

(app-j) APPENDIX J: CONFIGURING TV-OUT
__________________________________________________________________________

NVIDIA GPU-based video cards with a TV-Out (S-Video) connector can be
employed to use a television as another display device, just like a CRT
or digital flat panel.  The TV can be used by itself, or (on appropriate
video cards) in conjunction with another display device in a TwinView
configuration.

If a TV is the only display device connected to your video card, it will
be used as the primary display when you boot your system (ie the console
will come up on the TV just as if it were a CRT).  To use your TV with X,
there are a few parameters that you should pay special attention to in
your X config file:

  o The VertRefresh and HorizSync values in your monitor section;
    please make sure these are appropriate for your television.
    Values are generally:

        HorizSync 30-50
        VertRefresh 60

  o The Modes in your screen section; the valid modes for your TV encoder
    will be reported in a verbose X log file (generated with `startx --
    -logverbose 5`) when X is run on a TV.  Some modes may be limited
    to certain TV Standards; if that is the case, it will be noted in
    the X log file.  Generally, atleast 800x600 and 640x480 are supported.

  o The "TVStandard" option should be added to your screen section; valid
    values are:

        "PAL-B"  : used in Belgium, Denmark, Finland, Germany, Guinea,
                   Hong Kong, India, Indonesia, Italy, Malaysia, The
                   Netherlands, Norway, Portugal, Singapore, Spain,
                   Sweden, and Switzerland
        "PAL-D"  : used in China and North Korea
        "PAL-G"  : used in Denmark, Finland, Germany, Italy, Malaysia,
                   The Netherlands, Norway, Portugal, Spain, Sweden,
                   and Switzerland
        "PAL-H"  : used in Belgium
        "PAL-I"  : used in Hong Kong and The United Kingdom
        "PAL-K1" : used in Guinea
        "PAL-M"  : used in Brazil
        "PAL-N"  : used in France, Paraguay, and Uruguay
        "PAL-NC" : used in Argentina
        "NTSC-J" : used in Japan
        "NTSC-M" : used in Canada, Chile, Colombia, Costa Rica, Ecuador,
                   Haiti, Honduras, Mexico, Panama, Puerto Rico, South
                   Korea, Taiwan, United States of America, and Venezuela
        "HD480i" : 480 line interlaced
        "HD480p" : 480 line progressive
        "HD720p" : 720 line progressive
        "HD1080i": 1080 line interlaced
        "HD1080p": 1080 line progressive
        "HD576i" : 576 line interlace
        "HD576p" : 576 line progressive

    The line in your X config file should be something like:

        Option "TVStandard" "NTSC-M"

    If you do not specify a TVStandard, or you specify an invalid value,
    the default "NTSC-M" will be used.  Note: if your country is not in
    the above list, select the country closest to your location.

  o The "ConnectedMonitor" option can be used to tell X to use the TV for
    display.  This should only be needed if your TV is not detected by
    the video card, or you use a CRT (or digital flat panel) as your
    boot display, but want to redirect X to use the TV.  The line in
    your config file should be:

        Option "ConnectedMonitor" "TV"

  o The "TVOutFormat" option can be used to force SVIDEO or COMPOSITE
    output.  Without this option the driver autodetects the output format.
    Unfortunately, it does not always do this correctly.  The output format
    can be forced with the options:

         Option "TVOutFormat" "SVIDEO"

                     or

         Option "TVOutFormat" "COMPOSITE"

  o The "TVOverScan" option can be used to enable Overscan where
    supported.  Valid values are decimal values in the range 1.0 (which
    means overscan as much as possible: make the image as large as
    possible) and 0.0 (which means disable overscanning: make the image
    as small as possible).  Overscanning is disabled (0.0) by default.

    Overscan is currently only available on GeForce4 or newer GPUs
    with either NVIDIA or Conexant TV encoders.

The NVIDIA X driver may not restore the console correctly with XFree86
versions older than 4.3 when the console is a TV.  This is due to binary
incompatibilities between XFree86 int10 modules.  If you use a TV as
your console it is recommended that you upgrade to XFree86 4.3 or later.


__________________________________________________________________________

(app-k) APPENDIX K: CONFIGURING A LAPTOP
__________________________________________________________________________

INSTALLATION AND CONFIGURATION

Installation and configuration of the NVIDIA Accelerated Linux Driver
Set on a laptop is the same as for any desktop environment, with a few
minor exceptions, listed below.

Starting in the 1.0-2802 release, information about the internal flatpanel
for use in initializing the display is by default generated on the fly
from data stored in the video BIOS.  This can be disabled by setting
the "SoftEDIDs" kernel option to 0.  If "SoftEDIDs" is turned off, then
hardcoded data will be chosen from a table, based on the value of the
"Mobile" kernel option.

The "Mobile" kernel option can be set to any of the following values:

    0xFFFFFFFF : let the kernel module auto detect the correct value
             1 : Dell laptops
             2 : non-Compal Toshiba laptops
             3 : all other laptops
             4 : Compal Toshiba laptops
             5 : Gateway laptops

Again, the "Mobile" kernel option is only needed if SoftEDIDs is
disabled; when it is used, it is usually safest to let the kernel
module auto detect the correct value (this is the default behavior).

Should you need to alter either of these options, this can be done by
doing any of the following:

      o editing os-registry.c in the usr/src/nv/ directory of the
        .run file.

      o setting the value on the modprobe command line (eg: `modprobe
        nvidia NVreg_SoftEDIDs=0 NVreg_Mobile=3`)

      o adding an "options" line to your module configuration file,
        usually /etc/modules.conf (eg: "options nvidia
        NVreg_Mobile=5")

ADDITIONAL FUNCTIONALITY

TWINVIEW

All mobile NVIDIA chips support TwinView. TwinView on a laptop can
be configured in the same way as on a desktop machine (please refer
to APPENDIX I above); note that in a TwinView configuration using
the laptop's internal flat panel and an external CRT, the CRT is the
primary display device (specify it's HorizSync and VertRefresh in
the Monitor section of your X config file) and the flat panel is
the secondary display device (specify it's HorizSync and VertRefresh
through the SecondMonitorHorizSync and SecondMonitorVertRefresh options).
You can also employ the UseEdidFreqs option to acquire the HorizSync and
VertRefresh from the EDID of each display devices, and not worry about
setting them in your X config file (this should only be done if you
trust your display device's reported EDIDs -- please see the description
of the UseEdidFreqs option in APPENDIX D for details).


HOTKEY SWITCHING OF DISPLAY DEVICES

Besides TwinView, mobile NVIDIA chips also have the capacity to react to
an LCD/CRT hotkey event, toggling between each of the connected display
devices and each possible combination of the connected display devices
(note that only 2 display devices may be active at a time).  TwinView as
configured in your X config file and hotkey functionality are mutually
exclusive -- if you enable TwinView in your X config file, then the
NVIDIA X driver will ignore LCD/CRT hotkey events.

Another important aspect of hotkey functionality is that you can
dynamically connect and remove display devices to/from your laptop and
hotkey to them without restarting X.

A concern with all of this is how to validate and determine what modes
should be programmed on each display device.  First, it is immensely
helpful to use the UseEdidFreqs so that the hsync and vrefresh for
each display device can be retrieved from the display devices' EDID --
otherwise, the semantics of what the contents of the monitor section
mean constantly changes with each hotkey event.

When X is started, or when a change is detected in the list of connected
display devices, a new hotkey sequence list is constructed -- this lists
what display devices will be used with each hotkey event.  When a hotkey
event occurs, then the next hotkey state in the sequence is chosen.
Each mode requested in the X config file is validated against each
display device's constraints, and the resulting modes are made available
for that display device.  If multiple display devices are to be active
at once, then the modes from each display device are paired together;
if an exact match (same resolution) cannot be found, then the closest fit
is found, and the display device with the smaller resolution is panned
within the resolution of the other display device.

When vt-switching away from X, the vga console will always be restored on
the display device on which it was present when X was started.  Similarly,
when vt-switching back into X, the same display device configuration
will be used as when you vt-switched away from X, regardless of what
LCD/CRT hotkey activity occurred while vt-switched away.


NON-STANDARD MODES ON LCD DISPLAYS

Some users have had difficulty programming a 1400x1050 mode (the native
resolution of some laptop LCDs).  In version 4.0.3, XFree86 added several
1400x1050 modes to its database of default modes, but if you are using
an older version of XFree86, here is a modeline that you can use:

# -- 1400x1050 --
# 1400x1050 @ 60Hz, 65.8 kHz hsync
Modeline "1400x1050"  129  1400 1464 1656 1960
                           1050 1051 1054 1100 +HSync +VSync


KNOWN LAPTOP ISSUES

  o LCD/CRT hotkey switching is not currently functioning on any
    Toshiba laptop, with the exception of the Toshiba Satellite
    3000 series.
  o TwinView on Satellite 2800 series Toshbia laptops is not currently
    functioning.
  o The video overlay only works on the first display device on which
    you started X.  For example, if you start X on the internal LCD,
    run a video application that uses the video overlay (uses the
    "Video Overlay" adaptor advertised through the XV extension), and
    then hotkey switch to add a second display device, the video will
    not appear on the second display device.  To work around this,
    you can either configure the video application to use the "Video
    Blitter" adaptor advertised through the XV extension (this is always
    available), or hotkey switch to the display device on which you want
    to use the video overlay *before* starting X.


__________________________________________________________________________

(app-l) APPENDIX L: PROGRAMMING MODES
__________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set supports all standard VGA and
VESA modes, as well as most user-written custom mode lines; double-scan
modes are supported on all hardware.  Interlaced modes are supported on
all GeForce FX/Quadro FX and newer GPUs, and certain older GPUs; the X
log file will contain a message "Interlaced video modes are supported
on this GPU" if interlaced modes are supported.

In general, your display device (monitor/flat panel/television) will be
a greater constraint on what modes you can use than either your NVIDIA
GPU-based video board or the NVIDIA Accelerated Linux Driver Set.

To request one or more standard modes for use in X, you can simply add a
"Modes" line such as:

        Modes "1600x1200" "1024x768" "640x480"

in the appropriate Display subsection of your X config file (please see
the XF86Config(5x) or xorg.conf(5x) man pages for details).  The following
documentation is primarily of interest if you compose your own custom
mode lines, experiment with xvidtune(1), or are just interested in
learning more.  Please note that this is neither an explanation nor a
guide to the fine art of crafting custom mode lines for X.  We leave that,
rather, to documents such as the XFree86 Video Timings HOWTO (which can
be found at www.tldp.org).


DEPTH, BITS PER PIXEL, AND PITCH

While not directly a concern when programming modes, the bits used per
pixel is an issue when considering the maximum programmable resolution;
for this reason, it is worthwhile to address the confusion surrounding
the terms "depth" and "bits per pixel".  Depth is how many bits of
data are stored per pixel.  Supported depths are 8, 15, 16, and 24.
Most video hardware, however, stores pixel data in sizes of 8, 16, or
32 bits; this is the amount of memory allocated per pixel.  When you
specify your depth, X selects the bits per pixel (bpp) size in which to
store the data.  Below is a table of what bpp is used for each possible
depth:

        depth    bpp
        =====   =====
          8       8
         15      16
         16      16
         24      32

Lastly, the "pitch" is how many bytes in the linear frame buffer there are
between one pixel's data, and the data of the pixel immediately below.
You can think of this as the horizontal resolution multiplied by the
bytes per pixel (bits per pixel divided by 8).  In practice, the pitch may
be more than this product due to alignment constraints.


MAXIMUM RESOLUTIONS

The NVIDIA Accelerated Linux Driver Set and NVIDIA GPU-based video boards
support resolutions up to 2048x1536, though the maximum resolution
your system can support is also limited by the amount of video memory
(see USEFUL FORMULAS for details) and the maximum supported resolution
of your display device (monitor/flat panel/television).  Also note that
while use of a video overlay does not limit the maximum resolution or
refresh rate, video memory bandwidth used by a programmed mode does
effect the overlay quality.


USEFUL FORMULAS

The maximum resolution is a function both of the amount of video memory
and the bits per pixel you elect to use:

        HR * VR * (bpp/8) = Video Memory Used

In other words, the amount of video memory used is equal to the horizontal
resolution (HR) multiplied by the vertical resolution (VR) multiplied by
the bytes per pixel (bits per pixel divided by eight).  Technically, the
video memory used is actually the pitch times the vertical resolution,
and the pitch may be slightly greater than (HR * (bpp/8)) to accommodate
hardware requirements that the pitch be a multiple of some value.

Please note that this is just memory usage for the frame buffer; video
memory is also used by other things such as OpenGL or pixmap caching.

Another important relationship is that between the resolution, the pixel
clock (aka dot clock) and the vertical refresh rate:

        RR = PCLK / (HFL * VFL)

In other words, the refresh rate (RR) is equal to the pixel clock (PCLK)
divided by the total number of pixels: the horizontal frame length (HFL)
multiplied by the vertical frame length (VFL) (note that these are the
frame lengths, and not just the visible resolutions).  As described in
the XFree86 Video Timings HOWTO, the above formula can be rewritten as:

        PCLK = RR * HFL * VFL

Given a maximum pixel clock, you can adjust the RR, HFL and VFL as
desired, as long as the product of the three is consistent.  The pixel
clock is reported in the log file when you run X with verbose logging:
`startx -- -logverbose 5`.  Your X log should contain several lines like:

(--) NVIDIA(0): Display Device 0: maximum pixel clock at  8 bpp: 350 MHz
(--) NVIDIA(0): Display Device 0: maximum pixel clock at 16 bpp: 350 MHz
(--) NVIDIA(0): Display Device 0: maximum pixel clock at 32 bpp: 300 MHz

which indicate the maximum pixel clock at each bit per pixel size.


HOW MODES ARE VALIDATED

During the PreInit phase of the X server, the NVIDIA X driver validates
all requested modes by doing the following:

  o Take the intersection of the HorizSync and VertRefresh ranges given
    by the user in the X config file with the ranges reported by the
    monitor in the EDID (Extended Display Identification Data); this
    behavior can be disabled by using the "IgnoreEDID" option in which
    case the X driver will blindly accept the HorizSync and VertRefresh
    ranges given by the user.

  o Call the xf86ValidateModes() helper function, which finds modes with
    the names the user specified in the X config file, pruning
    out modes with invalid horizontal sync frequencies or vertical
    refresh rates, pixel clocks larger than the maximum pixel clock
    for the video card, or resolutions larger than the virtual
    screen size (if a virtual screen size was specified in the
    X config file).  Several other constraints are applied; see
    xc/programs/Xserver/hw/xfree86/common/xf86Mode.c:xf86ValidateModes().

  o All modes returned from xf86ValidateModes() are then examined to make
    sure their resolutions are not larger than the largest mode reported
    by the monitor's EDID (this can be disabled with the "IgnoreEDID"
    option.  If the display is a TV, each mode is checked to make sure
    it has a resolution that is supported by the TV encoder (usually
    only 800x600 and 640x480 are supported by the encoder).

  o All modes are also tested to confirm that they fit within the
    hardware's memory bandwidth constraints.  This test can be disabled
    with the NoBandWidthTest X config file option.

  o All remaining modes are then checked to make sure they pass the
    constraints described below in ADDITIONAL MODE CONSTRAINTS.

The last three steps are also done when each mode is programmed, to
catch potentially invalid modes submitted by the XF86VidModeExtension
(eg xvidtune(1)).  For TwinView, the above validation is done for the
modes requested for each display device.


ADDITIONAL MODE CONSTRAINTS

Below is a list of additional constraints on a mode's parameters that
must be met.  In some cases these are specific to particular chips.

  o The horizontal resolution (HR) must be a multiple of 8 and be less
    than or equal to the value in the table below.
  o The horizontal blanking width (the maximum of the horizontal frame
    length and the horizontal sync end minus the minimum of the horizontal
    resolution and the horizontal sync start (max(HFL,HSE) - min(HR,HSS))
    must be a multiple of 8 and be less than or equal to the value in
    the table below.
  o The horizontal sync start (HSS) must be a multiple of 8 and be less
    than or equal to the value in the table below.
  o The horizontal sync width (the horizontal sync end minus the
    horizontal sync start (HSE - HSS)) must be a multiple of 8 and be
    less than or equal to the value in the table below.
  o The horizontal frame length (HFL) must be a multiple of 8, must be
    greater than or equal to 40, and must be less than or equal to the
    value in the table below.
  o The vertical resolution (VR) must be less than or equal to the
    value in the table below.
  o The vertical blanking width (the maximum of the vertical frame length
    and the vertical sync end minus the minimum of the vertical resolution
    and the vertical sync start (max(VFL,VSE) - min(VR,VSS)) must be
    less than or equal to the value in the table below.
  o The vertical sync start (VSS) must be less than or equal to the
    value in the table below.
  o The vertical sync width (the vertical sync end minus the vertical sync
    start (VSE - VSS)) must be less than or equal to the value in the
    table below.
  o The vertical frame length (VFL) must be greater than or equal to 2 and
    less than or equal to the value in the table below.

       Maximum DAC Values
       ------------------

          | GeForce/TNT   Geforce2 & 3   Geforce4 or newer
    ______|_______________________________________________
          |
    HR    | 4096          4096           8192
    HBW   | 1016          1016           2040
    HSS   | 4088          4088           8224
    HSW   | 256           256            512
    HFL   | 4128          4128           8224
    VR    | 2048          4096           8192
    VBW   | 128           128            256
    VSS   | 2047          4095           8192
    VSW   | 16            16             16
    VFL   | 2049          4097           8192


Here is an example mode line demonstrating the use of each abbreviation
used above:

# Custom Mode line for the SGI 1600SW Flatpanel
#        name           PCLK  HR   HSS  HSE  HFL  VR   VSS  VSE  VFL

Modeline "sgi1600x1024" 106.9 1600 1632 1656 1672 1024 1027 1030 1067


ENSURING IDENTICAL MODETIMINGS

Some functionality, such as Active Stereo with TwinView, requires
control over exactly what mode timings are used.  There are several
ways to accomplish that:

    o If you only want to make sure that both display devices use
      the same modes, you only need to make sure that both display
      devices use the same HorizSync and VertRefresh values when
      performing mode validation; this would be done by making sure the
      HorizSync and SecondMonitorHorizSync match, and that the VertRefresh
      and the SecondMonitorVertRefresh match.

    o A more explicit approach is to specify the modeline you wish
      to use (using one of the modeline generators available),
      and using a unique name.  For example, if you wanted to use
      1024x768 at 120 Hz on each monitor in TwinView with active
      stereo, you might add something like:

        # 1024x768 @ 120.00 Hz (GTF) hsync: 98.76 kHz; pclk: 139.05 MHz
        Modeline "1024x768_120"  139.05  1024 1104 1216 1408  768 769 772 823  -HSync +Vsync

      In the monitor section of your X config file, and then in the
      Screen section of your X config file, specify a MetaMode like this:

        Option "MetaModes" "1024x768_120, 1024x768_120"

SEE ALSO:

    An XFree86 modeline generator, conforming to the GTF Standard is
    available here:

        http://gtf.sourceforge.net/

    For additional modeline generators, try searching for "modeline"
    on freshmeat.net.


__________________________________________________________________________

(app-m) APPENDIX M: FLIPPING AND UBB
__________________________________________________________________________

The NVIDIA Accelerated Linux Driver Set supports Unified Back Buffer
(UBB) and OpenGL Flipping.  These features can provide performance
gains in certain situtations.

  o Unified Back Buffer (UBB): UBB is available only on the Quadro family 
        of GPUs (Quadro4 NVS excluded) and is enabled by default
        when there is sufficient video memory available.  This can be
        disabled with the UBB X config option described in Appendix D.
        When UBB is enabled, all windows share the same back, stencil and
        depth buffer.  When there are many windows, the back, stencil
        and depth usage will never exceed the size of that used by a
        full screen window.  However, even for a single small window
        the back, stencil and depth the video memory usage is that of a
        full screen window.  In that case video memory may be used less
        efficiently than in the non-UBB case.

  o Flipping: when OpenGL flipping is enabled, OpenGL can perform
        buffer swaps by changing which buffer the DAC scans out rather
        than copying the back buffer contents to the front buffer; this is
        generally a much higher performance mechanism and allows tearless
        swapping during the vertical retrace (when __GL_SYNC_TO_VBLANK
        is set).  The conditions under which OpenGL can flip are slightly
        complicated, but in general: on Geforce or newer hardware, OpenGL
        can flip when a single full screen unobscured OpenGL application
        is running, and __GL_SYNC_TO_VBLANK is enabled.  Additionally,
        OpenGL can flip on Quadro hardware even when an OpenGL window
        is partially obscured or not full screen or __GL_SYNC_TO_VBLANK
        is not enabled.

 
__________________________________________________________________________

(app-n) APPENDIX N: KNOWN ISSUES
__________________________________________________________________________

The following problems still exist in this release and are in the process
of being resolved.

  o OpenGL + Xinerama
        Currently, OpenGL will not display to anything other than the
        first head in a Xinerama environment.

  o OpenGL and dlopen()
        There are some issues with older versions of the glibc dynamic 
        loader (e.g., the version that shipped with Red Hat Linux 7.2) and 
        applications such as Quake3 and Radiant, that use dlopen().
        See the FREQUENTLY ASKED QUESTIONS section for more details.

  o DPMS and TwinView
        DPMS Modes "suspend" and "standby" do not work correctly on
        a second CRT when using TwinView.  The screen becomes blank
        instead of the monitor being set to the requested DPMS state.

  o DPMS and Flat Panel
        DPMS modes "suspend" and "standby" do not work correctly on a
        flat panel display.  The screen becomes blank instead of the
        flat panel being set to the requested DPMS state.

  o Multicard, Multimonitor
        In some cases, the secondary card is not initialized correctly
        by the NVIDIA kernel module. You can work around this by enabling
        the XFree86 Int10 module to soft-boot all secondary cards. See 
        "APPENDIX D: X CONFIG OPTIONS" for details.

  o Laptop
        If you are using a laptop please see the "Known Laptop Issues" in 
        APPENDIX D.

  o FSAA
        When FSAA is enabled (the __GL_FSAA_MODE environment variable
        is set to a value that enables FSAA and a multisample visual is
        chosen), the rendering may be corrupted when resizing the window.

  o Interaction with pthreads
        Single threaded applications that dlopen() NVIDIA's libGL
        library, and then dlopen() any other library that is linked
        against pthreads will crash in NVIDIA's libGL library.  This does
        not happen in NVIDIA's new ELF TLS OpenGL libraries (please see
        (app-c)  APPENDIX C: INSTALLED COMPONENTS for a description of
        the ELF TLS OpenGL libraries).  Possible work arounds for this
        problem are:

            1) Load the library that is linked with pthreads before
               loading libGL.so.
            2) Link the application with pthreads.

  o libGL DSO finalizer and pthreads
        When a multithreaded OpenGL application exits, it is possible for
        libGL's DSO finalizer (also known as the destructor, or "_fini")
        to be called while other threads are executing OpenGL code.  The
        finalizer needs to free resources allocated by libGL.  This can
        cause problems for threads that are still using these resources.
        Setting the environment variable "__GL_NO_DSO_FINALIZER" to
        "1" will work around this problem by forcing libGL's finalizer
        to leave its resources in place.  These resources will still
        be reclaimed by the operating system when the process exits.
        Note that the finalizer is also executed as part of dlclose(3),
        so if you have an application that dlopens(3) and dlcloses(3)
        libGL repeatedly, "__GL_NO_DSO_FINALIZER" will cause libGL to
        leak resources until the process exits.  Using this option can
        improve stability in some multithreaded applications, including
        Java3D applications.

  o XVideo and the Composite X extension
        XVideo will not work correctly when Composite is enabled.  See
        (app-u) APPENDIX U: THE COMPOSITE X EXTENSION.

  o The X86-64 platform (AMD64/EM64T) and 2.6 kernels
        Many 2.4 and 2.6 x86_64 kernels have an accounting issue in their
        implementation of the change_page_attr kernel interface. Early
        2.6 kernels include a check that triggers a BUG() when this
        situation is encountered (triggering a BUG() results in the
        current application being killed by the kernel; this application
        would be your opengl application or potentially the X Server).
        The accounting issue has been resolved in the 2.6.11 kernel.

        We have added checks to recognize that the NVIDIA kernel
        module is being compiled for the x86-64 platform on a kernel
        between 2.6.0 and 2.6.11. In this case, we will disable
        usage of the change_page_attr kernel interface. This will
        avoid the accounting issue but leaves the system in danger
        of cache-aliasing (see the entry below on Cache Aliasing for more
        information). Note that this change_page_attr accounting issue
        and BUG() can be triggered by other kernel subsystems that rely
        on this interface.

        If you are using a 2.6 x86_64 kernel, it is recommended that
        you upgrade to a 2.6.11 or later kernel.

  o IOMMU/SWIOTLB interaction on the X86-64 platform
   
        Linux does not currently provide a mechanism for allocating
        memory with addresses that fall within the first 4GB of the
        physical memory installed in a Linux/x86-64 system. Addresses
        within this range are necessary for 32-bit PCI hardware to
        provide DMA capabilities. Instead, the Linux kernel provides a
        software I/O TLB on Intel's EM64T and IOMMU support on AMD's
        AMD64 platform.

        Unfortunately, some problems exist with both interfaces. Early
        implementations of the Linux SWIOTLB set aside a very small
        amount of memory for its memory pool (only 4MB). Also, when this
        memory pool is exhausted, some SWIOTLB implementations forcibly
        panic the kernel. This is also true for some implementations
        of the IOMMU interface.
       
        Kernel panics and related stability problems on Intel's EM64T
        platform can be avoided by increasing the size of the SWIOTLB
        pool with the 'swiotlb' kernel parameter. This kernel parameter
        expects the desired size as a number of 4 KB pages. NVIDIA
        suggests raising the size of the SWIOTLB pool to 64MB; this is
        accomplished by passing 'swiotlb=16384' to the kernel. Note that
        newer Linux 2.6 kernels already default to a 64MB SWIOTLB pool,
        see below for more information.

        Starting with Linux 2.6.9, the default size of the SWIOTLB is
        64MB and overflow handling is improved. Both of these changes are
        expected to greatly improve stability on Intel's EM64T platform.
        If you consider upgrading your Linux kernel to benefit from these
        improvements, NVIDIA recommends that you upgrade to Linux 2.6.11
        or a more recent Linux kernel. Please see the previous entry
        for additional information.

        On AMD's AMD64 platform, the size of the IOMMU can be configured
        in the system BIOS or, if no IOMMU BIOS option is available,
        using the 'iommu=memaper' kernel parameter. This kernel parameter
        expects an order and instructs the Linux kernel to create an
        IOMMU of size 32MB^order overlapping physical memory. If the
        system's default IOMMU is smaller than 64MB, the Linux kernel
        automatically replaces it with a 64MB IOMMU.

        To reduce the risk of stability problems as a result of IOMMU or
        SWIOTLB exhaustion on the X86-64 platform, the NVIDIA Linux
        driver internally limits its use of these interfaces. By default,
        the driver will not use more than 60MB of IOMMU/SWIOTLB space,
        leaving 4MB for the rest of the system (assuming a 64MB
        IOMMU/SWIOTLB).

        This limit can be adjusted with the 'NVreg_RemapLimit' NVIDIA
        kernel module option. Specifically, if the IOMMU/SWIOTLB is
        larger than 64MB, the limit can be adjusted to take advantage of
        the additional space. The 'NVreg_RemapLimit' option expects the
        size argument in bytes.

        NVIDIA recommends leaving 4MB available for the rest of the
        system when changing the limit. For example, if the internal
        limit is to be relaxed to account for a 128MB IOMMU/SWIOTLB, the
        recommended remap limit is 124MB. This remap limit can be
        specified by passing 'NVreg_RemapLimit=0x7c00000' to the NVIDIA
        kernel module.

        Please also read the previous entry for information on additional
        stability problems on this platform.

  o Cache Aliasing

        Cache aliasing occurs when multiple mappings to a physical page
        of memory have conflicting caching states, such as cached and
        uncached. Due to these conflicting states, data in that physical
        page may become corrupted when the processor's cache is flushed.
        If that page is being used for DMA by a driver such as NVIDIA's
        graphics driver, this can lead to hardware stability problems and
        system lockups.

        NVIDIA has encountered bugs with some Linux kernel versions that
        lead to cache aliasing. Although some systems will run perfectly
        fine when cache aliasing occurs, other systems will experience
        severe stability problems, including random lockups. Users
        experiencing stability problems due to cache aliasing will
        benefit from updating to a kernel that does not cause cache
        aliasing to occur.

        NVIDIA has added driver logic to detect cache aliasing and to
        print a warning with a message similar to the following:

        NVRM: bad caching on address 0x1cdf000: actual 0x46 != expected 0x73

        If you see this message in your log files and are experiencing
        stability problems, you should update your kernel to the latest
        version.

        If the message persists after updating your kernel, please send a
        bug report to NVIDIA.

  o 64-Bit BARs (Base Address Registers)

        Starting with native PCI Express GPUs, NVIDIA's GPUs will
        advertise a 64-bit BAR capability (a Base Address Register stores
        the location of a PCI I/O region, such as registers or a frame
        buffer). This means that the GPU's PCI I/O regions (registers and
        frame buffer) can be placed above the 32-bit address space (the
        first 4 gigabytes of memory).

        The decision of where the BAR is placed is made by the system
        BIOS at boot time. If the BIOS supports 64-bit BARs, then the
        NVIDIA PCI I/O regions may be placed above the 32-bit address
        space. If the BIOS does not support this feature, then our PCI
        I/O regions will be placed within the 32-bit address space as
        they have always been.

        Unfortunately, current Linux kernels (as of 2.6.11.x) do not
        understand or support 64-bit BARs. If the BIOS does place any
        NVIDIA PCI I/O regions above the 32-bit address space, the kernel
        will reject the BAR and the NVIDIA driver will not work.

        There is no known workaround at this point.


HARDWARE ISSUES

This section describes problems that will not be fixed.  Usually, the
source of the problem is beyond the control of NVIDIA.  Following is
the list of problems:

  o Gigabyte GA-6BX Motherboard
        This motherboard uses a LinFinity regulator on the 3.3-V rail
        that is rated to only 5 A -- less than the AGP specification,
        which requires 6 A.  When diagnostics or applications are
        running, the temperature of the regulator rises, causing the
        voltage to the NVIDIA chip to drop as low as 2.2 V.  Under these
        circumstances, the regulator cannot supply the current on the
        3.3-V rail that the NVIDIA chip requires.

        This problem does not occur when the graphics board has a
        switching regulator or when an external power supply is connected
        to the 3.3-V rail.

  o VIA KX133 and 694X Chip sets with AGP 2x
        On Athlon motherboards with the VIA KX133 or 694X chip set, such
        as the ASUS K7V motherboard, NVIDIA drivers default to AGP 2x mode
        to work around insufficient drive strength on one of the signals.

  o Irongate Chip sets with AGP 1x
        AGP 1x transfers are used on Athlon motherboards with the Irongate
        chip set to work around a problem with the signal integrity of
        the chip set.

  o ALi chipsets, ALi1541 and ALi1647
        On ALi1541 and ALi1647 chipsets, NVIDIA drivers disable AGP to work
        around timing issues and signal integrity issues. See "APPENDIX G: 
        ALI SPECIFIC ISSUES" for more information on ALi chipsets.

  o I/O APIC (SMP)
        If you are experiencing stability problems with a Linux SMP machine 
        and seeing I/O APIC warning messages from the Linux kernel, system
        reliability may be greatly improved by setting the "noapic" kernel
        parameter.

  o Local APIC (UP)
        On some systems, setting the "Local APIC Support on Uniprocessors"
        kernel configuration option can have adverse effects on system
        stability and performance.  If you are experiencing lockups with
        a Linux UP machine and have this option set, try disabling local
        APIC support.

__________________________________________________________________________

(app-o) APPENDIX O: PROC INTERFACE
__________________________________________________________________________

You can use the /proc filesystem interface to obtain run-time information
about the driver, any installed NVIDIA graphics cards, and the AGP status.

This information is contained by several files in /proc/driver/nvidia:

  o /proc/driver/nvidia/version
        Lists the installed driver revision and the version of the GNU C
        compiler used to build the Linux kernel module.

  o /proc/driver/nvidia/cards/0...3
        Provides information about each of the installed NVIDIA graphics
        adapters (model name, IRQ, BIOS version, Bus Type). Please note
        that the BIOS version is only available while X is running.

  o /proc/driver/nvidia/agp/card
        Information about the installed AGP card's AGP capabilities.

  o /proc/driver/nvidia/agp/host-bridge
        Information about the host bridge (model and AGP capabilities).
  
  o /proc/driver/nvidia/agp/status
        The current AGP status. If AGP support has been enabled on your
        system, the AGP driver being used, the AGP rate and information
        about the status of AGP Fast Writes and Side Band Addressing is
        shown.

        The AGP driver is either one of NVIDIA (NVIDIA's built-in AGP
        driver) or AGPGART (the Linux kernel's agpgart.o driver). If
        you see "inactive" next to AGPGART, then this means that the
        AGP chipset was programmed by AGPGART, but is not currently in
        use.

        SBA and Fast Writes indicate whether either one of the features
        is currently in use. Please note that several factors decide if
        support for either will be enabled. First of all, both the AGP
        card and the host bridge must support the feature. Even if both
        do support it, the driver may decide not to use it in favor of
        system stability. This is particularly true of AGP Fast Writes.

__________________________________________________________________________

(app-p) APPENDIX P: XVMC SUPPORT 
__________________________________________________________________________

This release includes support for the X-Video Motion Compensation (XvMC)
version 1.0 API on GeForce4, GeForce FX and newer products.  There is a static
library "libXvMCNVIDIA.a" and a dynamic one "libXvMCNVIDIA_dynamic.so"
which is suitable for dlopening.  GeForce4 MX, GeForce FX and newer
products support both XvMC's "IDCT" and "motion-compensation" levels of
acceleration.  GeForce4 Ti products only support the motion-compensation
level.  AI44 and IA44 subpictures are supported.  4:2:0 Surfaces up to
2032x2032 are supported.

libXvMCNVIDIA observes the XVMC_DEBUG environment variable and will
provide some debug output to stderr when set to an appropriate integer
value.  '0' disables debug output.  '1' enables debug output for failure
conditions.  '2' or higher enables output of warning messages.

__________________________________________________________________________

(app-q) APPENDIX Q: GLX SUPPORT
__________________________________________________________________________

This release supports GLX 1.3 with the following extensions:
   GLX_EXT_visual_info
   GLX_EXT_visual_rating
   GLX_SGIX_fbconfig
   GLX_SGIX_pbuffer
   GLX_ARB_get_proc_address

For a description of these extensions, please see the OpenGL extension
registry at http://oss.sgi.com/projects/ogl-sample/registry/index.html

Some of the above extensions exist as part of core GLX 1.3 functionality,
however, they are also exported as extensions for backwards compatibility.

__________________________________________________________________________

(app-r)  APPENDIX R: CONFIGURING MULTIPLE X SCREENS ON ONE CARD
__________________________________________________________________________

Graphics chips that support TwinView (see (app-i) APPENDIX I: CONFIGURING
TWINVIEW) can also be configured to treat each connected display device
as a separate X screen.

While there are several disadvantages to this approach as compared to
TwinView (eg: windows cannot be dragged between X screens, hardware
accelerated OpenGL cannot span the two X screens), it does offer several
advantages over TwinView:

    o If each display device is a separate X screen, then properties
      that may vary between X screens may vary between displays (eg:
      depth, root window size, etc).

    o Hardware that can only be used on one display at a time (eg:
      video overlays, hardware accelerated RGB overlays), and which
      consequently cannot be used at all when in TwinView, can be
      exposed on the first X screen when each display is a separate
      X screen.

    o The 1-to-1 association of display devices to X screens is
      more historically in line with X.

To configure two separate X screens to share one graphics chip, here is
what you will need to do:

First, create two separate Device sections, each listing the BusID of
the graphics card to be shared, each listing the driver as "nvidia",
and assign each a separate screen:


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


(note: you'll also need to create a second Monitor section)

Finally, update the ServerLayout section to use and position both Screen
sections:


    Section "ServerLayout"
        ...
        Screen         0 "Screen0" 
        Screen         1 "Screen1" leftOf "Screen0"
        ...
    EndSection


For further details, please refer to the XF86Config(5x) or xorg.conf(5x)
manpages.

__________________________________________________________________________

(app-s)  APPENDIX S: POWER MANAGEMENT SUPPORT
__________________________________________________________________________

This release includes support for APM based power management. This
means that our driver will support suspend and resume, but will not
support standby.

Your laptop's system bios will need to support APM, rather than ACPI.  
Many, but not all, of the GeForce2 and GeForce4 based laptops include
APM support. You can check for APM support via the procfs interface
(check for the existance of /proc/apm) or via the kernel's boot output:

% dmesg | grep -i apm

a message similar to this indicates apm support:
apm: BIOS version 1.2 Flags 0x03 (Driver version 1.16)

or a message like this indicates no apm support:
No APM support in Kernel

Although ACPI support is advancing in development kernels and some
backported patches for 2.4 kernels exist, the NVIDIA graphics driver
does not yet provide support for ACPI. We hope to finish this support
in the near future.

Note that standby is not supported, but that the kernel will attempt
to enter standby if told to do so. When changing power levels, many
system services are alerted of the change so that they can handle the
change appropriately. For example, networking will be disabled before
suspending, then reenabled when resuming. When the kernel attempts to
enter standby, the bios will fail the attempt. The kernel will print out
the error message "standby: Parameter out of range", but will fail to
notify the system services of the failure.  As a result, the system will
not go into suspension, but all system services will be disabled and the
system will appear hung. The best way to recover from this situation is
to enter suspend, then resume.


Power management support is still under development and a beta feature. As
a result, some functionality is still missing or unreliable. Known problems
include:

Sometimes chipsets lose their AGP configuration during suspend, and may
cause corruption on the bus upon resume. The AGP driver is required to
save and restore relevant register state on such systems; NVIDIA's NvAGP
is notified of power management events and ensures its configuration is
kept intact across suspend/resume cycles.

Linux 2.4 AGPGART does not support power management, Linux 2.6 AGPGART
does, but only for a few select chipsets. If you use either of these two
AGP drivers and find your system fails to resume reliably, you may have
more success with NVIDIA's NvAGP driver.

Disabling AGP support (please see APPENDIX F: CONFIGURING AGP for more
details on disabling AGP) will also work around this problem.

For ACPI, only S3 "Suspend to Ram" is currently supported. This means that
S4 "Suspend to Disk", otherwise known as "Software Suspend" or "swsusp" 
does not currently work reliably.


__________________________________________________________________________

(app-t)  APPENDIX T: DISPLAY DEVICE NAMES
__________________________________________________________________________

A "Display Device" refers to some piece of hardware capable of displaying
an image.  Display devices are separated into the three general
categories: analog CRTs, digital flatpanels (DFPs), and TeleVisions.
Note that analog flatpanels are considered the same as analog CRTs by
the driver.

A "Display Device Name" is a string description that uniquely identifies
a display device; it follows the format "<type>-<number>", for example:
"CRT-0", "CRT-1", "DFP-0", or "TV-0".  Note that the number indicates
how the display device connector is wired on the graphics board, and
has nothing to do with how many of that display device type is
present.  This means, for example, that you may have a "CRT-1",
even if you do not have a "CRT-0".

To determine what display devices are currently connected, you can check
your X log file for a line like this:

    (II) NVIDIA(0): Connected display device(s): CRT-0, DFP-0

Display device names can be used in the MetaMode, HorizSync, and
VertRefresh X config options to indicate what display device a
setting should be applied to.  For example:

 Option "MetaModes"   "CRT-0: 1600x1200,  DFP-0: 1024x768"
 Option "HorizSync"   "CRT-0: 50-110;     DFP-0: 40-70"
 Option "VertRefresh" "CRT-0: 60-120;     DFP-0: 60"

Specifying the display device name in these options is not required;
if display device names are specified, then the driver attempts
to infer which display device a setting applies to.  In the case
of MetaModes, for example, the first mode listed is applied to the
"first" display device, and the second mode listed is applied to the
"second" display device.  Unfortunately, it is often unclear which
display device is the "first" or "second".  That is why specifying
the display device name is preferable.

When specifying display device names, you may also omit the number part
of the name, though this is only useful if you only have one of that
type of display device.  For example, if you have one CRT and DFP
connected, you may reference in the MetaMode string like this:

 Option "MetaModes"   "CRT: 1600x1200,  DFP: 1024x768"

__________________________________________________________________________

(app-u)  APPENDIX U: THE COMPOSITE X EXTENSION
__________________________________________________________________________

X.org version X11R6.8.0 contains experimental support for a new X protocol
extension called Composite.  This extension allows windows to be drawn into
pixmaps instead of directly onto the screen.  In conjuction with the DAMAGE
and RENDER extensions, this allows a program called a composite manager to
blend windows together to draw the screen.

Performance can be improved by enabling 'Option "RenderAccel"' in
xorg.conf.  See (app-d) APPENDIX D: X CONFIG OPTIONS for more details.

Full Composite support will require additional driver support.  Currently,
direct rendering clients such as GLX have no way of knowing that they are
supposed to render into a pixmap, and will draw directly to the screen
instead.  We are currently investigating what is necessary for such clients
to interoperate seamlessly with Composite.  In the meantime, GLX will be
disabled by default when the Composite extension is detected.  An option
has been provided to re-enable it.  See 'Option "AllowGLXWithComposite"' in
(app-d) APPENDIX D: X CONFIG OPTIONS.

This issue was discussed on the xorg mailing list:

    http://freedesktop.org/pipermail/xorg/2004-May/000607.html

Composite also causes problems with other driver components:

  o Xv cannot draw into pixmaps that have been redirected offscreen and
    will draw directly onto the screen instead.  For some programs you can
    work around this issue by using an alternative video driver.  For
    example, "mplayer -vo x11" will work correctly, as will "xine -V xshm".
    If you wish to use Xv, you can simply disable the compositing manager
    and re-enable it when you are finished.

  o Workstation overlays are incompatible with Composite.

More information about Composite can be found at
http://freedesktop.org/Software/CompositeExt

__________________________________________________________________________

(app-v)  APPENDIX V: NVIDIA-SETTINGS
__________________________________________________________________________

A graphical configuration utility, nvidia-settings, is included with the
NVIDIA Linux graphics driver.  After installing the driver and starting X,
you can run this configuration utility by running:

    nvidia-settings

in a terminal window.

Detailed information about the configuration options available are
documented in the help window in the utility.

For more information, please see the user guide available here:

    ftp://download.nvidia.com/XFree86/Linux-x86/nvidia-settings-user-guide.txt

The source code to nvidia-settings is released as GPL and is available
here:

    ftp://download.nvidia.com/XFree86/nvidia-settings/


__________________________________________________________________________

(app-w)  APPENDIX W: THE XRANDR X EXTENSION
__________________________________________________________________________

X.org version X11R6.8.1 contains support for the rotation component of
the XRandR extension.  This allows screens to be rotated at 90 degree
increments.

The driver supports rotation with the extension when 'Option "RandRRotation"'
is enabled in the X config file.

Workstation RGB or CI overlay visuals will function at lower
performance when RandRRotation is enabled.  The video overlay is
not available when RandRRotation is enabled.

You can query the available rotations using the 'xrandr' command line
interface to the RandR extension by running:

    xrandr -q

You can set the rotation orientation of the screen by running any of:

    xrandr -o left
    xrandr -o right
    xrandr -o inverted
    xrandr -o normal

Rotation may also be set through the nvidia-settings configuration
utility in the "Rotation Settings" panel.

