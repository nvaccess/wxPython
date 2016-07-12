===========================
wxPython "Classic" for NVDA
===========================

This is the source tree for wxPython, a set of wrapper modules that enable
the use of wxWidgets from the Python language. This is the "Classic" or
legacy verson of wxPython, most new development is happening in the "Phoenix"
version of wxPython, which is a from the ground up reimplementation of much
of the foundation of wxPython.

This branch has been modified for NVDA.

Build Instructions on Windows 10
--------------------------------

- Make sure that you have Python 2.7 32 bit installed and in your PATH
- Download `Microsoft Visual C++ Compiler for Python 2.7` and Install
  - You will need to search for this. It should be available from Microsoft.
  - Use `Visual C++ 2008 32-bit Command Prompt` that is added to your system for all compilation steps
- Install `.net Framework 3.5`
  - You will need to search for this. It should be available from Microsoft.
- Make a build directory eg `C:\wxBuild`
- Clone wxPython source from: `https://github.com/nvaccess/wxPython` into your wxBuild directory
  - No need to checkout a particular branch, if the source came from nvaccess `master` should be the correct version.
  - You should now have `C:\wxBuild\wxPython`
- Get wxWidgets pre-built:
  - Download `wxWidgets3.0-devel-win32-3.0.2.0.exe`
  - See the `32-bit binaries for MSVC 9` link on `https://wxpython.org/download.php`
  - Install to a path like `C:\wxWidgets\3.0.2.0-win32`
  - Copy from `C:\wxWidgets\3.0.2.0-win32\*` to `C:\wxBuild\wxWidgets`
  - Copy `C:\wxBuild\wxWidgets\lib\vc90_dll` to `C:\wxBuild\wxPython\lib\vc90_dll`
- Ensure that we are building the pdb files for wxPython:
  - Edit `C:\wxBuild\wxPython\wxPython\config.py`
    - at around line 998 you will find the extra compile / link args.
- Get some other dependencies:
  - Start a terminal that has access to `bash` and `svn`
  - navigate to `C:\wxBuild\wxPython\wxPython`
  - run `bin/subrepos-make`
  - This will checkout some other dependencies that are required.

**Start the build**
  - Open the visual c++ 2008 command prompt
  - Navigate to `C:\wxBuild\wxPython\wxPython`
  - Set the following environment variables:
    - SET DISTUTILS_USE_SDK=1
    - SET MSSdk=1
  - Remember to set these again each time you restart the command prompt.
  - run `python build-wxpython.py --no_wxbuild --install > build.log`
  - You could then tail build.log to see progress.

**Check the results**
  - The outputs of the build should be in `C:\BuildWx\wxPython\wxPython\build\lib.win32-2.7\wx`
    - There will also be outputs in `C:\wxBuild\wxPython\wxPython\wx` and `<your python install>\Lib\site-packages` but we want the one in `...build\lib.win32-2.7\wx` since this matches best with what is currently in nvda
  - Compare checksums for the wxWidgets dlls with their source (in `C:\wxBuild\wxWidgets\lib\vc90_dll`)
  - Check that there are pdb files for the pyd files

**Finish up**
- Copy dll and pdb files from wxWidgets source (`C:\wxBuild\wxWidgets\lib\vc90_dll`) into the output.

**Errors?**
- `error: Unable to find vcvarsall.bat`
  - look at http://stackoverflow.com/questions/2667069/cannot-find-vcvarsall-bat-when-running-a-python-script
  - Try updating your python setup tools
    - pip install -U setuptools
- `...\wxPython\include\wx\platform.h(183) : fatal error C1083: Cannot open include file: 'wx/setup.h': No such file or directory`
  - Remember to copy the `wxWidgets\lib\vc90_dll` to wxPython (see above)
- `error: package directory 'wx\lib\agw' does not exist`
  - Remember to run `bin/subrepos-make` from the right directory (see above)
