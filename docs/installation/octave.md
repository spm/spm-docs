# GNU Octave

!!! note "What is GNU Octave?"
    [GNU Octave](https://en.wikipedia.org/wiki/GNU_Octave) is a high-level language that is mostly compatible with MATLAB. It is free open source software under the terms of the GNU General Public License.

## Octave

- Octave website: <https://octave.org/>
- Octave project: <http://hg.savannah.gnu.org/hgweb/octave/>
- Octave Help mailing list:
  <https://lists.gnu.org/mailman/listinfo/help-octave>
- Octave Maintainers mailing list:
  <https://lists.gnu.org/mailman/listinfo/octave-maintainers>
- Octave bug tracker: <http://savannah.gnu.org/bugs/?group=octave>
- Octave GitHub: <https://github.com/gnu-octave>
- Octave-Forge: <http://octave.sourceforge.net/>
- [GNU Octave and reproducible
  research](http://dx.doi.org/10.1016/j.jprocont.2012.04.006) by John W.
  Eaton

## MATLAB/Octave compatibility

GNU Octave is mostly compatible with MATLAB:

- <http://wiki.octave.org/FAQ#Differences_between_Octave_and_Matlab>
- <https://en.wikibooks.org/wiki/MATLAB_Programming/Differences_between_Octave_and_MATLAB>
- <http://wiki.octave.org/Compatibility>

Compatibility status from other neuroimaging MATLAB packages:

- EEGLAB: <https://sccn.ucsd.edu/wiki/Running_EEGLAB_on_Octave>
- FieldTrip:
  <http://www.fieldtriptoolbox.org/faq/can_i_use_octave_instead_of_matlab/>
- Psychtoolbox: <http://psychtoolbox.org/>

## Current status for SPM/Octave compatibility

SPM is currently **not** officially supported under Octave but many
modules are effectively compatible. Feedbacks about further evaluation
and validation are very welcome. In most situations, the [standalone
version of SPM](SPM/Standalone "wikilink") might be a sufficient
alternative.

Feel free to contact <fil.spm@ucl.ac.uk> if this is something you are
interested in. You need to use the latest versions of
[SPM12](https://www.fil.ion.ucl.ac.uk/spm/software/spm12/) and [GNU
Octave (8.1)](https://octave.org/download.html).

### Compilation

For compilation of the MEX files for Octave, run the following:
```matlab
cd /home/login/spm12/src
make PLATFORM=octave
make PLATFORM=octave install
```

An all-in-one Octave script to download, configure and install SPM in
the current directory is as follow (to be typed at the Octave prompt):
```matlab
%% Store current working directory
cwd = pwd;
%% Download SPM12 r7771
unzip ("https://github.com/spm/spm12/archive/r7771.zip", cwd);
%% Patch SPM12
urlwrite ("https://raw.githubusercontent.com/spm/spm-octave/main/spm12_r7771.patch", "spm12_r7771.patch");
system ("patch -p3 -d spm12-r7771 < spm12_r7771.patch");
%% Compile MEX files
cd (fullfile (cwd, "spm12-r7771", "src"));
system ("make PLATFORM=octave");
system ("make PLATFORM=octave install");
%% Add SPM12 to the function search path
addpath (fullfile (cwd, "spm12-r7771"));
cd (cwd);
%% Start SPM12
spm
```

### How to contribute

You can contribute to improve the support of SPM on GNU Octave in
several ways:

- [*Report bugs:*](https://octave.org/bugs.html) using the development
  version of Octave, report problems you encounter when using SPM to
  [SPM](mailto:fil.spm@ucl.ac.uk) or
  [Octave](https://octave.org/support-expectations.html) developers.
  Make sure to only report a bug in Octave when it's something that
  should not be fixed in SPM instead. Try to isolate the problem as much
  as possible.
- [*Propose patches*](https://octave.org/bugs.html) in Octave for bugs
  from the list below that have not been fixed yet.
- [*Contribute financially*](https://octave.org/donate.html) to the
  Octave community.

## Compatibility issues between SPM and Octave

### Requires changes in SPM

- Compilation of MEX files: use **"mkoctfile --mex"** and the
  MEX-file extension is ".mex" on all platforms.
- **do** is a reserved keyword in Octave (for a do-until loop) so cannot
  be used as variable name.
- **disp** should be called instead of **display** for standard
  variables (in OO programming).
- **builtin('display',obj)** does not work on user-defined objects
  (MATLAB returns 'classname object: x-by-y', i.e. it calls disp.m).
- **class** can only be called from the class constructor (and not in
  any other method function) (class(obj,'myclass') => myclass(obj)).
- The short-circuit **&&** and **||** operators should be used in if
  statements, instead of binary operators & and |. This is reported by
  MLINT.
- MEX files should include "mex.h" but not **"matrix.h"**, see
  [this](http://www.gnu.org/software/octave/doc/interpreter/Getting-Started-with-Mex_002dFiles.html)
- **cd** does not return current directory in Octave.
  Call **pwd** beforehand instead.

### Requires changes in Octave

- ~~It is not possible to create a **function handle** with a
  function name that does not exist (eg, x = @crash fails).~~
  [fixed](https://savannah.gnu.org/bugs/?31821)
- ~~Line continuation "**...**" does not ignore anything that
  appears after it, unless there is a comment sign **%** (MATLAB
  [does](http://www.mathworks.com/help/techdoc/matlab_env/f0-5789.html#f0-5857)).~~
  [fixed](https://savannah.gnu.org/bugs/?38653)
- ~~**strrep.m** works on strings but not cell arrays.~~
  [fixed](https://mailman.cae.wisc.edu/pipermail/bug-octave/2010-January/016627.html)
- ~~**isdeployed.m** does not exist (function X=isdeployed,
  X=false;).~~ [fixed](https://savannah.gnu.org/bugs/?32151)
- ~~**textscan.m**, **strread.m** and **textread.m** do not exist (in
  3.2.4; available in devel, see
  [this](http://octave.1599824.n4.nabble.com/textscan-wanted-td3002808.html)
  and [this](https://savannah.gnu.org/bugs/index.php?31380)). Note that
  in MATLAB, textscan should be preferred and replace strread and
  textread. The devel function crashes on
  *textscan('aaa.bbb','%s','delimiter','.')*~~
  [fixed](https://savannah.gnu.org/bugs/?32066)
- ~~A subfunction of a private class method does not access fields
  directly (i.e. it's a step further from
  [this](http://savannah.gnu.org/bugs/?30909)) and calls
  subsref/subsasgn instead.~~
  [fixed](https://savannah.gnu.org/bugs/?32060)
- **which.m** does not support the -ALL option (see
  [this](http://www.mathworks.com/help/techdoc/ref/which.html)). [in
  progress](https://savannah.gnu.org/bugs/?32088)
- ~~trailing **filesep** in **addpath**/**rmpath**~~
  [fixed](https://savannah.gnu.org/bugs/?32181)
- ~~*a='a';b={};c=**cellfun**(@(x)strcmp(a,x),b);* crashes with
  Octave (devel, not 3.2.4) while MATLAB returns c=[].~~
  [fixed](https://savannah.gnu.org/bugs/?32067)
- ~~Same error with *b=**get**(findobj(0,'Tag','xxx'),'a')*
  which should return [], i.e. problem with functions that return
  empty output.~~ [fixed](https://savannah.gnu.org/bugs/?32067)
- ~~**save.m** (and perhaps **load.m**) have trouble with MATLAB
  binary MAT-format.~~ (~~when some variables are not double
  precision (e.g. *clear a;a.field1=single(1);save a.mat a -v6;load
  a.mat* crashes on Octave dev ("error: load: invalid element type =
  0")~~ [fixed](https://savannah.gnu.org/bugs/?31942)).
  [fixed](https://savannah.gnu.org/bugs/?32090)
- ~~**dialog, errordlg, helpdlg, inputdlg, listdlg, msgbox, warndlg**
  do not exist in Octave.~~
- ~~**gco.m** now exists in Octave but creates a figure if none
  exists.~~ [fixed](https://savannah.gnu.org/bugs/?37211).
- **fcnchk.m** does not exist in Octave (can usually be replaced by
  **str2func.m**)
- ~~**isequal.m** does not work with objects (*error: find: wrong
  type argument `class', __isequal__.m at line 147*).~~
  [fixed](https://savannah.gnu.org/bugs/?32071)
- ~~**logm**(eye(3)) crashes, MATLAB returns zeros(3).~~
  [fixed](https://savannah.gnu.org/bugs/?32075)
- ~~**logm** sometimes returns complex numbers.~~
  [fixed](https://savannah.gnu.org/bugs/?32121)
- ~~**mat2str** fails on logical inputs.~~
  [fixed](https://savannah.gnu.org/bugs/?32102)
- Several problems with hierarchical **classes**.
  [fixed](https://savannah.gnu.org/bugs/?32107),
  [fixed](https://savannah.gnu.org/bugs/?32182),
  [fixed](https://savannah.gnu.org/bugs/?32210),
  [fixed](https://savannah.gnu.org/bugs/?32222),
  [fixed](https://savannah.gnu.org/bugs/?32242),
  [fixed](https://savannah.gnu.org/bugs/?32261), [in
  progress](https://savannah.gnu.org/bugs/?32296)
- ~~Assignment error with non-preallocated **sparse** matrices
  (*clear a; a(1,:)=sparse(1,3,1,1,3);* returns *A(I,J,...) = X:
  dimensions mismatch*.~~
  [fixed](https://savannah.gnu.org/bugs/?32263)
- ~~**tic/toc** do not handle input/output arguments as in MATLAB
  (*tStart=tic; any_statements; tElapsed=toc(tStart);*).~~
  [fixed](http://hg.savannah.gnu.org/hgweb/octave/rev/f8d5095fa90d)
- ~~**str2num**(',1,1') returns [1 1] in MATLAB and [] in
  Octave.~~ [fixed](https://savannah.gnu.org/bugs/?34346)
- ~~Objects not converted as structure when loaded from a
  **MAT-file** if class definition is not in path.~~
  [fixed](https://savannah.gnu.org/bugs/?32641)
- ~~Accessing graphics **object properties** from an empty handle
  displays obscure warning (*get([],'x')*).~~
  [fixed](https://savannah.gnu.org/bugs/?32642)
- ~~Compatibility: **save** with empty variable names.~~
  [fixed](https://savannah.gnu.org/bugs/?34259)
- ~~Test on **fileparts** input argument.~~
  [fixed](https://savannah.gnu.org/bugs/?40062)
- ~~**nargchk**(1,1,1,'struct') returns a 1x1 struct with no fields
  in Octave and a 0x1 struct with fields *message* and *identifier* in
  MATLAB.~~ [fixed](https://savannah.gnu.org/bugs/index.php?33808)
- ***[[];{'a'}]*** returns *{[];'a'}* instead of *{'a'}*,
  [won't fix](https://savannah.gnu.org/bugs/?37086)
- ~~Empty struct **struct([])** not preserved when saved in a
  MAT-file.~~ [fixed](https://savannah.gnu.org/bugs/?37087)
- ~~Problem in **regexprep** with **backslash** escape
  character.~~ [fixed](https://savannah.gnu.org/bugs/?37092)
- ~~Segmentation fault with [B,C]=**chol**(-speye(3)).~~
  [fixed](https://savannah.gnu.org/bugs/?37095)
- ~~**mwSignedIndex** is not defined => typedef int
  mwSignedIndex;~~ [fixed](https://savannah.gnu.org/bugs/?37133)
- ~~**cell arrays of cell arrays** as saved by matlabbatch are not
  parsed properly with Octave.~~
  [fixed](https://savannah.gnu.org/bugs/?37132)
- **mkoctfile** does not recognize argument **-outdir**. [in
  progress](https://savannah.gnu.org/bugs/?57486)
- ~~**desktop** function does not exist, particularly useful for
  calls **desktop('-inuse')**.~~
  [fixed](https://savannah.gnu.org/bugs/?37330)
- ~~Compilation with **SuiteSparse 3.2** fails.~~
  [fixed](https://savannah.gnu.org/bugs/?37031)
- ~~undefined symbol: **mexCallMATLAB**.~~
  [fixed](https://savannah.gnu.org/bugs/?37342)
- ~~Segmentation fault with **clf.m** test.~~
  [fixed](https://savannah.gnu.org/bugs/?44621)
- ~~**W** specifier in fopen does not work.~~
  [fixed](https://savannah.gnu.org/bugs/?38851)
- ~~Parser oddity with **if** statements.~~
  [fixed](https://savannah.gnu.org/bugs/?38758)
- ~~Problem with **mxArray** in MEX files.~~
  [fixed](https://savannah.gnu.org/bugs/?40429)
- ~~Parse error with **local functions** in classdef files.~~
  [fixed](https://savannah.gnu.org/bugs/?41723)
- ~~**ind2rgb** does not handle float inputs in the same way than
  MATLAB.~~ [fixed](https://savannah.gnu.org/bugs/?41851)
- ~~Invalid conversion from string to real scalar with **%c**.~~,
  [fixed](https://savannah.gnu.org/bugs/?42235)
- ~~Compilation error **yylex** was not declared in this
  scope.~~, [fixed](https://savannah.gnu.org/bugs/?43023)
- ~~Compilation failure due to **openGL**.~~
  [fixed](https://savannah.gnu.org/bugs/?44275)
- ~~**-depth** argument in **findobj**.~~,
  [fixed](https://savannah.gnu.org/bugs/?43136)
- ~~**evalc** is not defined.~~
  [fixed](https://savannah.gnu.org/patch/?8033)
- **[io64.h](http://www.mathworks.co.uk/help/matlab/matlab_external/large-file-i-o.html)**
  is not defined, [in progress](http://savannah.gnu.org/bugs/?45036).
- save **function handle** variables in MATLAB's binary data format,
  [in progress](https://savannah.gnu.org/bugs/?43215)
- ~~Support of **close all force**.~~,
  [fixed](https://savannah.gnu.org/bugs/?44324)
- ~~warning in **findobj** when using regexp.~~,
  [fixed](https://savannah.gnu.org/bugs/?44610)
- ~~Printing a figure not containing axes.~~
  [fixed](https://savannah.gnu.org/bugs/?44655)
- ~~Error following an error in a **callback** of a uimenu.~~
  [fixed](https://savannah.gnu.org/bugs/?44656)
- ~~SelectionType **open** for double-click non available
  (Qt-only).~~ [fixed](https://savannah.gnu.org/bugs/?44669)
- hgload can't open MATLAB figures, [in
  progress](https://savannah.gnu.org/bugs/?44670)
- Difference with figure/uicontrol between Octave and MATLAB, [in
  progress](https://savannah.gnu.org/bugs/?44672)
- Segmentation fault when loading a **MAT-file** containing a
  **function_handle** to a subfunction, [in
  progress](https://savannah.gnu.org/bugs/?44679)
- ~~Behaviour of figure property **ToolBar** when set to
  **auto**.~~ [fixed](https://savannah.gnu.org/bugs/?44686)
- ~~Properties of a **popupmenu** uicontrol.~~
  [fixed](https://savannah.gnu.org/bugs/?44687)
- ~~Callback execution of an **edit** uicontrol.~~
  [fixed](https://savannah.gnu.org/bugs/?44690)
- Mouse interaction with image objects don't work in fltk toolkit, [in
  progress](https://savannah.gnu.org/bugs/?44691)
- ~~Property **value** of **checkbox** uicontrol.~~
  [fixed](https://savannah.gnu.org/bugs/?44699)
- Changing the **style** of a uicontrol after creation, [in
  progress](https://savannah.gnu.org/bugs/?44700)
- Display of **popupmenu** uicontrol (Qt) [in
  progress](https://savannah.gnu.org/bugs/?44704)
- Mouse click callbacks of a **listbox** uicontrol, [in
  progress](https://savannah.gnu.org/bugs/?44748)
- ~~Interpretation of cell array in **String** property of a **text**
  uicontrol.~~ [fixed](https://savannah.gnu.org/bugs/?44749)
- ~~Incorrect output in **textscan/strread** with trailing
  delimiter.~~ [fixed](https://savannah.gnu.org/bugs/?44750)
- ~~Failure to exit when running Octave in **--no-gui** mode.~~
  [fixed](https://savannah.gnu.org/bugs/?44751)
- ~~Freeze with **drawnow**.~~
  [fixed](https://savannah.gnu.org/bugs/?44759)
- ~~Print options: **-noui, -painters, -opengl**.~~
  [fixed](https://savannah.gnu.org/bugs/?44760)
- Reset submenus of a **uicontextmenu**, [in
  progress](https://savannah.gnu.org/bugs/?44763)
- **Legend** object printed below lines in plot, [in
  progress](https://savannah.gnu.org/bugs/?44765)
- ~~Order of **uimenu**s.~~
  [fixed](https://savannah.gnu.org/bugs/?44770)
- ~~**CreateMode** argument for msgbox/errordlg/warndlg.~~
  [fixed](https://savannah.gnu.org/bugs/?44775)
- ~~Error **no method for 'scalar struct = scalar**'.~~
  [fixed](https://savannah.gnu.org/bugs/?44777)
- Print does not preserve **multiline** text and TeX markup, [in
  progress](https://savannah.gnu.org/bugs/?44797)
- ~~Removing a context menu.~~
  [fixed](https://savannah.gnu.org/bugs/?44801)
- ~~Case-sensitive listdlg's **SelectionMode** values.~~
  [fixed](https://savannah.gnu.org/bugs/?44822)
- Image display in a 3D view, [in
  progress](https://savannah.gnu.org/bugs/?44861)
- ~~Issues with copy to **clipboard**.~~
  [fixed](https://savannah.gnu.org/bugs/?44866)
- Error **base_graphics_object::get_properties: invalid graphics
  object**, [in progress](https://savannah.gnu.org/bugs/?44875)
- ~~**Visible** property of a uicontextmenu.~~
  [fixed](https://savannah.gnu.org/bugs/?44939)
- ~~make attempts to build libgui even with **--disable-gui**.~~
  [fixed](https://savannah.gnu.org/bugs/?45543)
- ~~function **localfunctions** not implemented.~~
  [fixed](https://savannah.gnu.org/bugs/?47705)
- function **import** not implemented, [in
  progress](https://savannah.gnu.org/bugs/?47753)
- Access to **object arrays**, [in
  progress](https://savannah.gnu.org/bugs/index.php?47755)
- Loading a **function handle** from a MAT-file, [in
  progress](https://savannah.gnu.org/bugs/?47763)
- ~~Segmentation fault with missing **warning state "all"**.~~
  [fixed](https://savannah.gnu.org/bugs/?47837)
- ~~**corrcoef** is missing.~~
  [fixed](https://savannah.gnu.org/bugs/?47824)
- ~~Third output of **uiputfile** undefined when user presses
  Cancel.~~ [fixed](https://savannah.gnu.org/bugs/?48171)
- ~~MEX object files "*.o" are not automatically deleted after
  compilation (they are with MATLAB's mex).~~
  [fixed](https://savannah.gnu.org/bugs/?54182)
- ~~Octave buffers output, which can be blocking. **fflush(stdout)**
  or
  [page_screen_output](http://www.gnu.org/software/octave/doc/interpreter/Paging-Screen-Output.html)
  has to be used.~~ [pager disabled by default in
  4.4](https://www.gnu.org/software/octave/NEWS-4.4.html)
- ~~/usr/X11R6/lib/**libGL.so**: could not read symbols on Suse 64
  bits.~~ [fixed](https://savannah.gnu.org/bugs/?32160)
- ~~**CHOLMOD_NOT_POSDEF** was not declared in this scope.~~
  [fixed](https://savannah.gnu.org/bugs/?37134)
- ~~**sortrows/sort**: only cell arrays of character strings may be
  sorted error.~~ [fixed](https://savannah.gnu.org/bugs/?42523)
- Error in concatenation of **classdef** objects, [in
  progress](https://savannah.gnu.org/bugs/?44665)
- ~~Restore window button does not trigger a **repaint event** for
  its content.~~ [fixed](https://savannah.gnu.org/bugs/?44776)
- ~~**ginput** doesn't correctly process shift/ctlr/alt key
  combinations.~~[fixed](https://savannah.gnu.org/bugs/?44833)
- GUI Command Window could support **syntax highlight**, [in
  progress](https://savannah.gnu.org/bugs/?44872)
- ~~**Ctrl+C** doesn't interrupt and causes SIGABRT at exit.~~
  [fixed](https://savannah.gnu.org/bugs/?44912)
- ~~**mxCreateNumericArray** with zero size.~~
  [fixed](https://savannah.gnu.org/bugs/?45319)
- ~~Build error with **mx-cdm-dm.cc**.~~
  [fixed](https://savannah.gnu.org/bugs/?45556)
- ~~**linspace()** incompatibility with Matlab when N < 2.~~
  [fixed](https://savannah.gnu.org/bugs/?45820)
- ~~Make display of coordinates in figure's **status bar**
  optional.~~ [fixed](https://savannah.gnu.org/bugs/?46025)
- ~~Crash when **uicontrol's callback** returns an error.~~
  [fixed](https://savannah.gnu.org/bugs/?46038)
- ~~Refresh when using **waitfor**.~~
  [fixed](https://savannah.gnu.org/bugs/?46039)
- ~~**ButtonDownFcn** callback of an image.~~
  [fixed](https://savannah.gnu.org/bugs/?46049)
- ~~**run** behaves differently from Matlab on function
  m-files.~~ [fixed](https://savannah.gnu.org/bugs/?47807)
- ~~doc build fails with **texi2dvi/texi2pdf** errors on Ubuntu
  14.04.~~ [fixed](https://savannah.gnu.org/bugs/?48172)
- ~~Figure handle input argument for **close(...,'force')**.~~
  [fixed](https://savannah.gnu.org/bugs/?48173)
- ~~"**parse error**" error message in GUI callbacks.~~
  [fixed](https://savannah.gnu.org/bugs/?48175)
- ~~**delete(allchild(fig))** in a "deletefcn" callback raises
  error.~~ [fixed](https://savannah.gnu.org/bugs/?48186)
- ~~missing **getframe** function.~~
  [fixed](https://savannah.gnu.org/bugs/?48195)
- ~~**Special characters** in uicontrol's string.~~
  [fixed](https://savannah.gnu.org/bugs/?48214)
- ~~**prefdir** should not be a private function.~~
  [fixed](https://savannah.gnu.org/bugs/?48221)
- ~~**Help menu** of Qt figures.~~
  [fixed](https://savannah.gnu.org/bugs/?48223)
- ~~Detection of **Qscintilla** libraries.~~
  [fixed](https://savannah.gnu.org/bugs/?48234)
- Root graphics property "**MonitorPositions**" not fully implemented,
  [in progress](https://savannah.gnu.org/bugs/?48239)
- ~~Default settings with **uicontrols**.~~
  [fixed](https://savannah.gnu.org/bugs/?48255)
- ~~Segmentation fault with **Qt figures**.~~
  [fixed](https://savannah.gnu.org/bugs/?48361)
- **uicontrols extent** is incorrect, [in
  progress](https://savannah.gnu.org/bugs/?48446)
- ~~**mexCallMATLABWithTrap** not implemented.~~
  [fixed](https://savannah.gnu.org/bugs/?48949)
- ~~**mxSetDimensions** for cell arrays.~~
  [fixed](https://savannah.gnu.org/bugs/?49010)
- ~~**realpow**: produced complex result.~~
  [fixed](https://savannah.gnu.org/bugs/?49036)
- Cannot set **breakpoints in subfunctions** from GUI editor when not
  using "endfunction" keyword, [in
  progress](https://savannah.gnu.org/bugs/?49068)
- Missing MEX function **mxArrayToUTF8String**, [in
  progress](https://savannah.gnu.org/bugs/?49077)
- **Backgroundcolor** ignored for pushbutton and radiobutton, [in
  progress](https://savannah.gnu.org/bugs/?49247)
- ~~Using exit() in batch mode throws
  **octave::exit_exception**.~~
  [fixed](https://savannah.gnu.org/bugs/?49271)
- ~~Display of images for **axes** partially outside a figure.~~
  [fixed](https://savannah.gnu.org/bugs/?49490)
- ~~uicontrol **popupmenu** sizing.~~
  [fixed](https://savannah.gnu.org/bugs/?49552)
- ~~Implementation of **containers.Map**.~~
  [fixed](https://savannah.gnu.org/bugs/?49559)
- ~~Error in the **unwind_protect_cleanup** section of
  print.m.~~[fixed](https://savannah.gnu.org/bugs/?49600)
- **strmatch**, incompatible result on 'empty' input, [in
  progress](https://savannah.gnu.org/bugs/?49601)
- **Patch** with zero area not displayed with OpenGL, [in
  progress](https://savannah.gnu.org/bugs/?49611)
- ~~**questdlg** displays buttons in reverse order.~~
  [fixed](https://savannah.gnu.org/bugs/?49644)
- ~~**waitfor** should silently accept an empty graphics
  handle.~~ [fixed](https://savannah.gnu.org/bugs/?49645)
- ~~Segmentation fault after a **caught error** in an Octave
  script.~~ [fixed](https://savannah.gnu.org/bugs/?49646)
- ~~Non-empty output for non-matching **regexp** with 'names'
  option.~~ [fixed](https://savannah.gnu.org/bugs/?49659)
- ~~Invalid FID and **fopen(FID)**.~~
  [fixed](https://savannah.gnu.org/bugs/?49695)
- Matlab **eval** function accepts a column vector string input, [in
  progress](https://savannah.gnu.org/bugs/?49696)
- ~~**__have_gnuplot__** does not return anything.~~
  [fixed](https://savannah.gnu.org/bugs/?49700)
- **uimenu** 's position is sometimes 0, [in
  progress](https://savannah.gnu.org/bugs/?49734)
- **rotate3d** compatibility with Matlab, [in
  progress](https://savannah.gnu.org/bugs/?49747)
- ~~Output of **uicontrol**.~~
  [fixed](https://savannah.gnu.org/bugs/?49751)
- ~~**isequalwithequalnans** is missing.~~
  [fixed](https://savannah.gnu.org/bugs/?50113)
- ~~**fwrite** input argument type.~~
  [fixed](https://savannah.gnu.org/bugs/?50157)
- ~~**set()** is case-sensitive.~~
  [fixed](https://savannah.gnu.org/bugs/?50163)
- ~~Update list of **missing functions**.~~
  [fixed](https://savannah.gnu.org/bugs/?50222)
- **sum, cumsum**, etc. mishandle integer inputs, [in
  progress](https://savannah.gnu.org/bugs/?50238)
- ~~Error using print and the **append** flag.~~
  [fixed](https://savannah.gnu.org/bugs/?50318)
- ${cmd} replacement operator in **regexprep**, [in
  progress](https://savannah.gnu.org/bugs/?50329)
- ~~Position of **uimenu**.~~
  [fixed](https://savannah.gnu.org/bugs/?50369)
- ~~**Loading of figures** (and other objects) from Octave IDE.~~
  [fixed](https://savannah.gnu.org/bugs/?50543)
- ~~**orderfields** is slow.~~
  [fixed](https://savannah.gnu.org/bugs/?50688)
- ~~Undefined input to a **classdef** method.~~
  [fixed](https://savannah.gnu.org/bugs/?50716)
- zeros: **like** keyword, [in
  progress](https://savannah.gnu.org/bugs/?50854)
- textscan option **MultipleDelimsAsOne** does not apply to space or tab
  characters, [in progress](https://savannah.gnu.org/bugs/?51093)
- ~~**ismember** fails if the string ends in a space.~~
  [fixed](https://savannah.gnu.org/bugs/?51187)
- ~~Infinite loop in **normest1**.~~
  [fixed](https://savannah.gnu.org/bugs/?51241)
- ~~**pinv(0)** different from Matlab.~~
  [fixed](https://savannah.gnu.org/bugs/?51246)
- ~~**center()** relies on broadcasting.~~
  [fixed](https://savannah.gnu.org/bugs/?51249)
- ~~**private** function in **classdef** file.~~
  [fixed](https://savannah.gnu.org/bugs/?51276)
- **max_recursion_depth** error in classdef constructor, [in
  progress](https://savannah.gnu.org/bugs/?51285)
- ~~**isequal** is slow.~~
  [fixed](https://savannah.gnu.org/bugs/?51403)
- display of multidimensional arrays uses **'ans**', [in
  progress](https://savannah.gnu.org/bugs/?51410)
- **strcmp** with multidimensional char arrays, [in
  progress](https://savannah.gnu.org/bugs/?51412)
- Implementation of class **categorical**, [in
  progress](https://savannah.gnu.org/bugs/?51516)
- Support for "**import**" keyword, [in
  progress](https://savannah.gnu.org/bugs/?51688)
- ~~**modal windowstyle** property not working.~~
  [fixed](https://savannah.gnu.org/bugs/?51846)
- ~~Missing keyword **help** for classdef keywords.~~
  [fixed](https://savannah.gnu.org/bugs/?52591)
- ~~**makeValidName** and **makeUniqueStrings**.~~
  [fixed](https://savannah.gnu.org/bugs/?52596)
- ~~PDF user manual uses a **backward apostrophe** ` in code
  examples.~~ [fixed](https://savannah.gnu.org/bugs/?52775)
- Implementation of **histcounts**, [in
  progress](https://savannah.gnu.org/bugs/?52855)
- Implement **jsondecode**, **jsonencode** functions, [in
  progress](https://savannah.gnu.org/bugs/?53100)
- Figure property **IntegerHandle** does not work fully with Qt toolkit,
  [in progress](https://savannah.gnu.org/bugs/?53342)
- ~~"**Number**" property for figure.~~
  [fixed](https://savannah.gnu.org/bugs/?53343)
- ~~Implementation of **movegui**.~~
  [fixed](https://savannah.gnu.org/bugs/?53400)
- ~~Implementation of **savefig**.~~
  [fixed](https://savannah.gnu.org/bugs/?53403)
- Octave 4.3.0+ can't **load** figures saved with previous versions,
  [in progress](https://savannah.gnu.org/bugs/?53468)
- ~~Unknown command `**codequoteundirected**' in help text.~~
  [fixed](https://savannah.gnu.org/bugs/?53473)
- ~~warning message when **qcollectiongenerator** and
  **qhelpgenerator** are not found.~~
  [fixed](https://savannah.gnu.org/bugs/?53474)
- ~~Syntax of documentation with **texinfo** 4.13.~~
  [fixed](https://savannah.gnu.org/bugs/?53479)
- ~~Segmentation fault when executing a **script** containing a
  figure.~~ [fixed](https://savannah.gnu.org/bugs/?53487)
- ~~warning from **opengl_renderer** about light object.~~
  [fixed](https://savannah.gnu.org/bugs/?53511)
- ~~uicontrol/uibuttongroup: **focusing** not implemented yet.~~
  [fixed](https://savannah.gnu.org/bugs/?53710)
- octave deadlocks with **deletefcn** callback that calls graphical
  function, [in progress](https://savannah.gnu.org/bugs/?53802)
- ~~doc: some **default Qt properties** are different between
  systems.~~ [fixed](https://savannah.gnu.org/bugs/?53805)
- ~~**MEX file** in a **private** directory of a class.~~
  [fixed](https://savannah.gnu.org/bugs/?53856)
- ~~**ismember** error with mixed numeric and char arrays
  inputs.~~ [fixed](https://savannah.gnu.org/bugs/?53924)
- ~~Error with **whos -file**: 'load' not found.~~
  [fixed](https://savannah.gnu.org/bugs/?54030)
- Behavior of **open** with unknown or non-existing files, [in
  progress](https://savannah.gnu.org/bugs/?54064)
- ~~Colors of a **uicontrol pushbutton**.~~
  [fixed](https://savannah.gnu.org/bugs/?54065)
- ~~**uipanel** doesn't show border in linux.~~
  [fixed](https://savannah.gnu.org/bugs/?54189)
- ~~**camlight** (axis_handle, ...) should work for Matlab
  Compatibility.~~ [fixed](https://savannah.gnu.org/bugs/?54372)
- ~~Implementation of **isfolder**.~~
  [fixed](https://savannah.gnu.org/bugs/?54456)
- **Private** directory in **+package**, [in
  progress](https://savannah.gnu.org/bugs/?54658)
- ~~Figure's Position when **MenuBar** is none.~~
  [fixed](https://savannah.gnu.org/bugs/?54678)
- **load()** should issue an error if specified variable does not exist
  in file, [in progress](https://savannah.gnu.org/bugs/?54736)
- **subsasgn** call when the subscripted expression contains the end
  keyword, [in progress](https://savannah.gnu.org/bugs/?54783)
- Implementation of **memmapfile**, [in
  progress](https://savannah.gnu.org/bugs/?54788)
- **File browser** unresponsive, [in
  progress](https://savannah.gnu.org/bugs/?54885)
- ~~uicontrol: validation of **cdata** property.~~
  [fixed](https://savannah.gnu.org/bugs/?54918)
- ~~**savefig** should accept a vector of figure handles for Matlab
  compatibility.~~ [fixed](https://savannah.gnu.org/bugs/?54924)
- ~~**colormap** property of a figure cannot be empty.~~
  [fixed](https://savannah.gnu.org/bugs/?54926)
- Interpreter cannot find **methods** in files of classdefs in
  **packages**, [in progress](https://savannah.gnu.org/bugs/?54941)
- ~~Implement uicontrol **focusing** behavior.~~
  [fixed](https://savannah.gnu.org/bugs/?54942)
- ~~Use of **camlight** when a patch is not visible.~~
  [fixed](https://savannah.gnu.org/bugs/?54970)
- Removal of **called_from_builtin**, [in
  progress](https://savannah.gnu.org/bugs/?54995)
- ~~Property **VertexNormals** not updated.~~
  [fixed](https://savannah.gnu.org/bugs/?55014)
- ~~**gunzip/bunzip2** error with cell array of strings input.~~
  [fixed](https://savannah.gnu.org/bugs/?55102)
- ~~DOCSTRING macro does not recognize
  **matlab.lang.makeValidName**.~~
  [fixed](https://savannah.gnu.org/bugs/?55209)
- ~~Change of a **togglebutton** uicontrol's value not reflected
  graphically.~~ [fixed](https://savannah.gnu.org/bugs/?55211)
- **Global** variable in a **MEX** file, [in
  progress](https://savannah.gnu.org/bugs/?55363)
- ~~warning: **popupmenu** value not within valid display
  range.~~ [fixed](https://savannah.gnu.org/bugs/?55368)
- Speed issue with **uicontrols**, [in
  progress](https://savannah.gnu.org/bugs/?55418)
- ~~gcbf and **HandleVisibility** property.~~
  [fixed](https://savannah.gnu.org/bugs/?55428)
- ~~Add support for more types for image's **cdata**.~~
  [fixed](https://savannah.gnu.org/bugs/?55436)
- Binary input image for **edge**, [in
  progress](https://savannah.gnu.org/bugs/?55438)
- colorbar properties need **listeners** to invoke actions, [in
  progress](https://savannah.gnu.org/bugs/?55441)
- ~~Implementation of **lightangle**.~~
  [fixed](https://savannah.gnu.org/bugs/?55446)
- ~~[MXE Octave] lib vs **lib64**.~~
  [fixed](https://savannah.gnu.org/bugs/?55608)
- **Path** management in the GUI, [in
  progress](https://savannah.gnu.org/bugs/?55623)
- ~~**isosurface** is slow.~~
  [fixed](https://savannah.gnu.org/bugs/?55642)
- ~~Image not displayed with **YDir** set to normal.~~
  [fixed](https://savannah.gnu.org/bugs/?55654)
- Conflict between package **namespace** and function name, [in
  progress](https://savannah.gnu.org/bugs/?55667)
- **subsref** called in a subscripted assignment operation, [in
  progress](https://savannah.gnu.org/bugs/?55856)
- statistical CDF functions lack **upper** argument support, [in
  progress](https://savannah.gnu.org/bugs/?43721)
- Closing **plots** much **slower** in Octave 5.1.0, [in
  progress](https://savannah.gnu.org/bugs/?55908)
- **dbup** and **dbdown** not working as expected, [in
  progress](https://savannah.gnu.org/bugs/?56020)

### Undecided yet

- `mkoctfile`'s option to define output file name is `-o` or
  `--output` while `mex`'s option is `-output`. If used, no file
  extension (`.mex`) is appended.
- **computer.m** returns different strings than on MATLAB (`PCWIN`,
  `GLNX86`, `PCWIN64`, `GLNXA64`, `MACI64`), e.g. `x86_64-unknown-linux-gnu`.
- **load.m** and **save.m** automatically add a `.mat` file extension if
  not provided with MATLAB (Octave doesn't).

### Miscellaneous

- **exist('OCTAVE_VERSION','builtin')** can be used to detect if
  running in Octave or MATLAB.
