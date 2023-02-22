If the provided **shortpath.dll** and **uigetpath.dll** don\'t work
it\'s probably because they were compiled against a previous version of
Matlab. But the source code is included so you can re-compile with your
current version of Matlab, first setup mex, at the Matlab prompt type:

`   >> mex <span style="color:#a020f0;">-setup</span>`

and Matlab will say

`   Please choose your compiler for building external interface (MEX) files: `  
  
`   Would you like mex to locate installed compilers [y]/n? `

so choose \'y\' Matlab then lists the available compilers:

`   Select a compiler: `  
`   [1] Lcc-win32 C 2.4.1 in C:\PROGRA~1\MATLAB\R2007a\sys\lcc `  
`   [2] Microsoft Visual C++ .NET 2003 in C:\Program Files\Microsoft Visual Studio .NET 2003`  
`   [0] None`  
`   Compiler: `

**Note:** I have Visual Studio 2003 installed, that\'s why I have 2
options. If you have Borland installed you would also see it listed
above. \<br\>\<br\> The compiler used with MinGW and Cygwin (gcc) isn\'t
listed, because it\'s not registered in Windows the same way that
Matlab, Visual Studio etc. are. This is the very reason for using Gnumex
to setup options for gcc. Choose \[1\] to use the compiler provided with
Matlab.

Now change directories into **c:\gnumex\src** and compile the .c files:

`   mex <span style="color:#a020f0;">shortpath.c -output shortpath.dll</span>`  
`   mex <span style="color:#a020f0;">uigetpath.c -output uigetpath.dll</span>`

You might get an error like this when compiling uigetpath.c:

`c:\docume~1\beau\locals~1\temp\mex_c58982ee-6281-4e1f-e2ac-723987a01052\uigetpath.obj .text: undefined reference to '_SHGetMalloc@4' `  
`c:\docume~1\beau\locals~1\temp\mex_c58982ee-6281-4e1f-e2ac-723987a01052\uigetpath.obj .text: undefined reference to '_SHBrowseForFolder@4' `  
`c:\docume~1\beau\locals~1\temp\mex_c58982ee-6281-4e1f-e2ac-723987a01052\uigetpath.obj .text: undefined reference to '_SHGetPathFromIDList@8' `  
  
`  C:\PROGRA~1\MATLAB\R2007A\BIN\MEX.PL: Error: Link of 'uigetpath.dll' failed. `  
  
`<span style="color:#ff0000;">??? Error using ==> mex at 206`  
`Unable to complete successfully.</span>`

But luckily uigetpath.dll is not crucial to the function of Gnumex. I
believe it\'s only used when a \"Browse\" button is pushed, to present
the user with a browsable folder tree. To avoid using it, just type
paths into the text boxes in Gnumex. This error does not occur when I
use the Visual Studio compiler most likely because its mexopts.bat file
is setup to properly link to Microsoft libraries for the \_SH functions
listed. \<br\>\<br\> Now that you have a working shortpath.dll (and
possibly uigetpath.dll) copy it/them to the root Gnumex folder
(c:\gnumex).
