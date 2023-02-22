## GNU Octave

[GNU Octave](w:GNU_Octave "wikilink") is a high-level language that is
mostly compatible with MATLAB. It is free open source software under the
terms of the GNU General Public License.

### Octave

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

### MATLAB/Octave compatibility

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
Octave (7.3)](https://octave.org/download.html).

### Compilation

For compilation of the MEX files for Octave, run the following:
\<pre\>\<nowiki\> cd /home/login/spm12/src make PLATFORM=octave make
PLATFORM=octave install \</nowiki\>\</pre\>

An all-in-one Octave script to download, configure and install SPM in
the current directory is as follow (to be typed at the Octave prompt):
\<pre\>\<nowiki\> %% Store current working directory cwd = pwd; %%
Download SPM12 r7771 unzip
(\"<https://github.com/spm/spm12/archive/r7771.zip>\", cwd); %% Patch
SPM12 urlwrite
(\"<https://raw.githubusercontent.com/spm/spm-octave/main/spm12_r7771.patch>\",
\"spm12_r7771.patch\"); system (\"patch -p3 -d spm12-r7771 \<
spm12_r7771.patch\"); %% Compile MEX files cd (fullfile (cwd,
\"spm12-r7771\", \"src\")); system (\"make PLATFORM=octave\"); system
(\"make PLATFORM=octave install\"); %% Add SPM12 to the function search
path addpath (fullfile (cwd, \"spm12-r7771\")); cd (cwd); %% Start SPM12
spm \</nowiki\>\</pre\>

### How to contribute

You can contribute to improve the support of SPM on GNU Octave in
several ways:

- [*Report bugs:*](https://octave.org/bugs.html) using the development
  version of Octave, report problems you encounter when using SPM to
  [SPM](mailto:fil.spm@ucl.ac.uk) or
  [Octave](https://octave.org/support-expectations.html) developers.
  Make sure to only report a bug in Octave when it\'s something that
  should not be fixed in SPM instead. Try to isolate the problem as much
  as possible.
- [*Propose patches*](https://octave.org/bugs.html) in Octave for bugs
  from the list below that have not been fixed yet.
- [*Contribute financially*](https://octave.org/donate.html) to the
  Octave community.

## Compatibility issues between SPM and Octave

### Requires changes in SPM

- Compilation of MEX files: use **\"mkoctfile \--mex\"** and the
  MEX-file extension is \".mex\" on all platforms.
- **do** is a reserved keyword in Octave (for a do-until loop) so cannot
  be used as variable name.
- **disp** should be called instead of **display** for standard
  variables (in OO programming).
- **builtin(\'display\',obj)** does not work on user-defined objects
  (MATLAB returns \'classname object: x-by-y\', i.e. it calls disp.m).
- **class** can only be called from the class constructor (and not in
  any other method function) (class(obj,\'myclass\') =\> myclass(obj)).
- The short-circuit **&&** and **\|\|** operators should be used in if
  statements, instead of binary operators & and \|. This is reported by
  MLINT.
- MEX files should include \"mex.h\" but not **\"matrix.h\"**, see
  [this](http://www.gnu.org/software/octave/doc/interpreter/Getting-Started-with-Mex_002dFiles.html)
- **cd** does not return current directory in Octave (unlikely to
  change, see [this
  thread](https://mailman.cae.wisc.edu/pipermail/bug-octave/2010-November/017273.html)).
  Call **pwd** beforehand instead.

### Requires changes in Octave

- \<s\>It is not possible to create a **function handle** with a
  function name that does not exist (eg, x = @crash fails).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?31821)
- \<s\>Line continuation \"**\...**\" does not ignore anything that
  appears after it, unless there is a comment sign **%** (MATLAB
  [does](http://www.mathworks.com/help/techdoc/matlab_env/f0-5789.html#f0-5857)).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?38653)
- \<s\>**strrep.m** works on strings but not cell arrays.\</s\>
  [fixed](https://mailman.cae.wisc.edu/pipermail/bug-octave/2010-January/016627.html)
- \<s\>**isdeployed.m** does not exist (function X=isdeployed,
  X=false;).\</s\> [fixed](https://savannah.gnu.org/bugs/?32151)
- \<s\>**textscan.m**, **strread.m** and **textread.m** do not exist (in
  3.2.4; available in devel, see
  [this](http://octave.1599824.n4.nabble.com/textscan-wanted-td3002808.html)
  and [this](https://savannah.gnu.org/bugs/index.php?31380)). Note that
  in MATLAB, textscan should be preferred and replace strread and
  textread. The devel function crashes on
  *textscan(\'aaa.bbb\',\'%s\',\'delimiter\',\'.\')*\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32066)
- \<s\>A subfunction of a private class method does not access fields
  directly (i.e. it\'s a step further from
  [this](http://savannah.gnu.org/bugs/?30909)) and calls
  subsref/subsasgn instead.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32060)
- **which.m** does not support the -ALL option (see
  [this](http://www.mathworks.com/help/techdoc/ref/which.html)). [in
  progress](https://savannah.gnu.org/bugs/?32088)
- \<s\>trailing **filesep** in **addpath**/**rmpath**\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32181)
- \<s\>*a=\'a\';b={};c=**cellfun**(@(x)strcmp(a,x),b);* crashes with
  Octave (devel, not 3.2.4) while MATLAB returns c=\[\].\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32067)
- \<s\>Same error with *b=**get**(findobj(0,\'Tag\',\'xxx\'),\'a\')*
  which should return \[\], i.e. problem with functions that return
  empty output.\</s\> [fixed](https://savannah.gnu.org/bugs/?32067)
- \<s\>**save.m** (and perhaps **load.m**) have trouble with MATLAB
  binary MAT-format.\</s\> (\<s\>when some variables are not double
  precision (e.g. *clear a;a.field1=single(1);save a.mat a -v6;load
  a.mat* crashes on Octave dev (\"error: load: invalid element type =
  0\")\</s\> [fixed](https://savannah.gnu.org/bugs/?31942)).
  [fixed](https://savannah.gnu.org/bugs/?32090)
- \<s\>**dialog, errordlg, helpdlg, inputdlg, listdlg, msgbox, warndlg**
  do not exist in Octave.\</s\>
- \<s\>**gco.m** now exists in Octave but creates a figure if none
  exists.\</s\> [fixed](https://savannah.gnu.org/bugs/?37211).
- **fcnchk.m** does not exist in Octave (can usually be replaced by
  **str2func.m**)
- \<s\>**isequal.m** does not work with objects (*error: find: wrong
  type argument \`class\', \_\_isequal\_\_.m at line 147*).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32071)
- \<s\>**logm**(eye(3)) crashes, MATLAB returns zeros(3).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32075)
- \<s\>**logm** sometimes returns complex numbers.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32121)
- \<s\>**mat2str** fails on logical inputs.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32102)
- Several problems with hierarchical **classes**.
  [fixed](https://savannah.gnu.org/bugs/?32107),
  [fixed](https://savannah.gnu.org/bugs/?32182),
  [fixed](https://savannah.gnu.org/bugs/?32210),
  [fixed](https://savannah.gnu.org/bugs/?32222),
  [fixed](https://savannah.gnu.org/bugs/?32242),
  [fixed](https://savannah.gnu.org/bugs/?32261), [in
  progress](https://savannah.gnu.org/bugs/?32296)
- \<s\>Assignment error with non-preallocated **sparse** matrices
  (*clear a; a(1,:)=sparse(1,3,1,1,3);* returns *A(I,J,\...) = X:
  dimensions mismatch*.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32263)
- \<s\>**tic/toc** do not handle input/output arguments as in MATLAB
  (*tStart=tic; any_statements; tElapsed=toc(tStart);*).\</s\>
  [fixed](http://hg.savannah.gnu.org/hgweb/octave/rev/f8d5095fa90d)
- \<s\>**str2num**(\',1,1\') returns \[1 1\] in MATLAB and \[\] in
  Octave.\</s\> [fixed](https://savannah.gnu.org/bugs/?34346)
- \<s\>Objects not converted as structure when loaded from a
  **MAT-file** if class definition is not in path.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32641)
- \<s\>Accessing graphics **object properties** from an empty handle
  displays obscure warning (*get(\[\],\'x\')*).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?32642)
- \<s\>Compatibility: **save** with empty variable names.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?34259)
- \<s\>Test on **fileparts** input argument.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?40062)
- \<s\>**nargchk**(1,1,1,\'struct\') returns a 1x1 struct with no fields
  in Octave and a 0x1 struct with fields *message* and *identifier* in
  MATLAB.\</s\> [fixed](https://savannah.gnu.org/bugs/index.php?33808)
- ***\[\[\];{\'a\'}\]*** returns *{\[\];\'a\'}* instead of *{\'a\'}*,
  [won\'t fix](https://savannah.gnu.org/bugs/?37086)
- \<s\>Empty struct **struct(\[\])** not preserved when saved in a
  MAT-file.\</s\> [fixed](https://savannah.gnu.org/bugs/?37087)
- \<s\>Problem in **regexprep** with **backslash** escape
  character.\</s\> [fixed](https://savannah.gnu.org/bugs/?37092)
- \<s\>Segmentation fault with \[B,C\]=**chol**(-speye(3)).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37095)
- \<s\>**mwSignedIndex** is not defined =\> typedef int
  mwSignedIndex;\</s\> [fixed](https://savannah.gnu.org/bugs/?37133)
- \<s\>**cell arrays of cell arrays** as saved by matlabbatch are not
  parsed properly with Octave.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37132)
- **mkoctfile** does not recognize argument **-outdir**. [in
  progress](https://savannah.gnu.org/bugs/?57486)
- \<s\>**desktop** function does not exist, particularly useful for
  calls **desktop(\'-inuse\')**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37330)
- \<s\>Compilation with **SuiteSparse 3.2** fails.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37031)
- \<s\>undefined symbol: **mexCallMATLAB**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37342)
- \<s\>Segmentation fault with **clf.m** test.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44621)
- \<s\>**W** specifier in fopen does not work.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?38851)
- \<s\>Parser oddity with **if** statements.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?38758)
- \<s\>Problem with **mxArray** in MEX files.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?40429)
- \<s\>Parse error with **local functions** in classdef files.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?41723)
- \<s\>**ind2rgb** does not handle float inputs in the same way than
  MATLAB.\</s\> [fixed](https://savannah.gnu.org/bugs/?41851)
- \<s\>Invalid conversion from string to real scalar with **%c**.\</s\>,
  [fixed](https://savannah.gnu.org/bugs/?42235)
- \<s\>Compilation error **yylex** was not declared in this
  scope.\</s\>, [fixed](https://savannah.gnu.org/bugs/?43023)
- \<s\>Compilation failure due to **openGL**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44275)
- \<s\>**-depth** argument in **findobj**.\</s\>,
  [fixed](https://savannah.gnu.org/bugs/?43136)
- \<s\>**evalc** is not defined.\</s\>
  [fixed](https://savannah.gnu.org/patch/?8033)
- **[io64.h](http://www.mathworks.co.uk/help/matlab/matlab_external/large-file-i-o.html)**
  is not defined, [in progress](http://savannah.gnu.org/bugs/?45036).
- save **function handle** variables in MATLAB\'s binary data format,
  [in progress](https://savannah.gnu.org/bugs/?43215)
- \<s\>Support of **close all force**.\</s\>,
  [fixed](https://savannah.gnu.org/bugs/?44324)
- \<s\>warning in **findobj** when using regexp.\</s\>,
  [fixed](https://savannah.gnu.org/bugs/?44610)
- \<s\>Printing a figure not containing axes.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44655)
- \<s\>Error following an error in a **callback** of a uimenu.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44656)
- \<s\>SelectionType **open** for double-click non available
  (Qt-only).\</s\> [fixed](https://savannah.gnu.org/bugs/?44669)
- hgload can\'t open MATLAB figures, [in
  progress](https://savannah.gnu.org/bugs/?44670)
- Difference with figure/uicontrol between Octave and MATLAB, [in
  progress](https://savannah.gnu.org/bugs/?44672)
- Segmentation fault when loading a **MAT-file** containing a
  **function_handle** to a subfunction, [in
  progress](https://savannah.gnu.org/bugs/?44679)
- \<s\>Behaviour of figure property **ToolBar** when set to
  **auto**.\</s\> [fixed](https://savannah.gnu.org/bugs/?44686)
- \<s\>Properties of a **popupmenu** uicontrol.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44687)
- \<s\>Callback execution of an **edit** uicontrol.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44690)
- Mouse interaction with image objects don\'t work in fltk toolkit, [in
  progress](https://savannah.gnu.org/bugs/?44691)
- \<s\>Property **value** of **checkbox** uicontrol.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44699)
- Changing the **style** of a uicontrol after creation, [in
  progress](https://savannah.gnu.org/bugs/?44700)
- Display of **popupmenu** uicontrol (Qt) [in
  progress](https://savannah.gnu.org/bugs/?44704)
- Mouse click callbacks of a **listbox** uicontrol, [in
  progress](https://savannah.gnu.org/bugs/?44748)
- \<s\>Interpretation of cell array in **String** property of a **text**
  uicontrol.\</s\> [fixed](https://savannah.gnu.org/bugs/?44749)
- \<s\>Incorrect output in **textscan/strread** with trailing
  delimiter.\</s\> [fixed](https://savannah.gnu.org/bugs/?44750)
- \<s\>Failure to exit when running Octave in **\--no-gui** mode.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44751)
- \<s\>Freeze with **drawnow**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44759)
- \<s\>Print options: **-noui, -painters, -opengl**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44760)
- Reset submenus of a **uicontextmenu**, [in
  progress](https://savannah.gnu.org/bugs/?44763)
- **Legend** object printed below lines in plot, [in
  progress](https://savannah.gnu.org/bugs/?44765)
- \<s\>Order of **uimenu**s.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44770)
- \<s\>**CreateMode** argument for msgbox/errordlg/warndlg.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44775)
- \<s\>Error **no method for \'scalar struct = scalar**\'.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44777)
- Print does not preserve **multiline** text and TeX markup, [in
  progress](https://savannah.gnu.org/bugs/?44797)
- \<s\>Removing a context menu.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44801)
- \<s\>Case-sensitive listdlg\'s **SelectionMode** values.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44822)
- Image display in a 3D view, [in
  progress](https://savannah.gnu.org/bugs/?44861)
- \<s\>Issues with copy to **clipboard**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44866)
- Error **base_graphics_object::get_properties: invalid graphics
  object**, [in progress](https://savannah.gnu.org/bugs/?44875)
- \<s\>**Visible** property of a uicontextmenu.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44939)
- \<s\>make attempts to build libgui even with **\--disable-gui**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?45543)
- \<s\>function **localfunctions** not implemented.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?47705)
- function **import** not implemented, [in
  progress](https://savannah.gnu.org/bugs/?47753)
- Access to **object arrays**, [in
  progress](https://savannah.gnu.org/bugs/index.php?47755)
- Loading a **function handle** from a MAT-file, [in
  progress](https://savannah.gnu.org/bugs/?47763)
- \<s\>Segmentation fault with missing **warning state \"all\"**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?47837)
- \<s\>**corrcoef** is missing.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?47824)
- \<s\>Third output of **uiputfile** undefined when user presses
  Cancel.\</s\> [fixed](https://savannah.gnu.org/bugs/?48171)
- \<s\>MEX object files \"\*.o\" are not automatically deleted after
  compilation (they are with MATLAB\'s mex).\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54182)
- \<s\>Octave buffers output, which can be blocking. **fflush(stdout)**
  or
  [page_screen_output](http://www.gnu.org/software/octave/doc/interpreter/Paging-Screen-Output.html)
  has to be used.\</s\> [pager disabled by default in
  4.4](https://www.gnu.org/software/octave/NEWS-4.4.html)
- \<s\>/usr/X11R6/lib/**libGL.so**: could not read symbols on Suse 64
  bits.\</s\> [fixed](https://savannah.gnu.org/bugs/?32160)
- \<s\>**CHOLMOD_NOT_POSDEF** was not declared in this scope.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?37134)
- \<s\>**sortrows/sort**: only cell arrays of character strings may be
  sorted error.\</s\> [fixed](https://savannah.gnu.org/bugs/?42523)
- Error in concatenation of **classdef** objects, [in
  progress](https://savannah.gnu.org/bugs/?44665)
- \<s\>Restore window button does not trigger a **repaint event** for
  its content.\</s\> [fixed](https://savannah.gnu.org/bugs/?44776)
- \<s\>**ginput** doesn\'t correctly process shift/ctlr/alt key
  combinations.\</s\>[fixed](https://savannah.gnu.org/bugs/?44833)
- GUI Command Window could support **syntax highlight**, [in
  progress](https://savannah.gnu.org/bugs/?44872)
- \<s\>**Ctrl+C** doesn\'t interrupt and causes SIGABRT at exit.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?44912)
- \<s\>**mxCreateNumericArray** with zero size.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?45319)
- \<s\>Build error with **mx-cdm-dm.cc**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?45556)
- \<s\>**linspace()** incompatibility with Matlab when N \< 2.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?45820)
- \<s\>Make display of coordinates in figure\'s **status bar**
  optional.\</s\> [fixed](https://savannah.gnu.org/bugs/?46025)
- \<s\>Crash when **uicontrol\'s callback** returns an error.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?46038)
- \<s\>Refresh when using **waitfor**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?46039)
- \<s\>**ButtonDownFcn** callback of an image.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?46049)
- \<s\>**run** behaves differently from Matlab on function
  m-files.\</s\> [fixed](https://savannah.gnu.org/bugs/?47807)
- \<s\>doc build fails with **texi2dvi/texi2pdf** errors on Ubuntu
  14.04.\</s\> [fixed](https://savannah.gnu.org/bugs/?48172)
- \<s\>Figure handle input argument for **close(\...,\'force\')**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48173)
- \<s\>\"**parse error**\" error message in GUI callbacks.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48175)
- \<s\>**delete(allchild(fig))** in a \"deletefcn\" callback raises
  error.\</s\> [fixed](https://savannah.gnu.org/bugs/?48186)
- \<s\>missing **getframe** function.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48195)
- \<s\>**Special characters** in uicontrol\'s string.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48214)
- \<s\>**prefdir** should not be a private function.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48221)
- \<s\>**Help menu** of Qt figures.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48223)
- \<s\>Detection of **Qscintilla** libraries.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48234)
- Root graphics property \"**MonitorPositions**\" not fully implemented,
  [in progress](https://savannah.gnu.org/bugs/?48239)
- \<s\>Default settings with **uicontrols**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48255)
- \<s\>Segmentation fault with **Qt figures**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48361)
- **uicontrols extent** is incorrect, [in
  progress](https://savannah.gnu.org/bugs/?48446)
- \<s\>**mexCallMATLABWithTrap** not implemented.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?48949)
- \<s\>**mxSetDimensions** for cell arrays.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49010)
- \<s\>**realpow**: produced complex result.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49036)
- Cannot set **breakpoints in subfunctions** from GUI editor when not
  using \"endfunction\" keyword, [in
  progress](https://savannah.gnu.org/bugs/?49068)
- Missing MEX function **mxArrayToUTF8String**, [in
  progress](https://savannah.gnu.org/bugs/?49077)
- **Backgroundcolor** ignored for pushbutton and radiobutton, [in
  progress](https://savannah.gnu.org/bugs/?49247)
- \<s\>Using exit() in batch mode throws
  **octave::exit_exception**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49271)
- \<s\>Display of images for **axes** partially outside a figure.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49490)
- \<s\>uicontrol **popupmenu** sizing.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49552)
- \<s\>Implementation of **containers.Map**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49559)
- \<s\>Error in the **unwind_protect_cleanup** section of
  print.m.\</s\>[fixed](https://savannah.gnu.org/bugs/?49600)
- **strmatch**, incompatible result on \'empty\' input, [in
  progress](https://savannah.gnu.org/bugs/?49601)
- **Patch** with zero area not displayed with OpenGL, [in
  progress](https://savannah.gnu.org/bugs/?49611)
- \<s\>**questdlg** displays buttons in reverse order.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49644)
- \<s\>**waitfor** should silently accept an empty graphics
  handle.\</s\> [fixed](https://savannah.gnu.org/bugs/?49645)
- \<s\>Segmentation fault after a **caught error** in an Octave
  script.\</s\> [fixed](https://savannah.gnu.org/bugs/?49646)
- \<s\>Non-empty output for non-matching **regexp** with \'names\'
  option.\</s\> [fixed](https://savannah.gnu.org/bugs/?49659)
- \<s\>Invalid FID and **fopen(FID)**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49695)
- Matlab **eval** function accepts a column vector string input, [in
  progress](https://savannah.gnu.org/bugs/?49696)
- \<s\>**\_\_have_gnuplot\_\_** does not return anything.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49700)
- **uimenu** \'s position is sometimes 0, [in
  progress](https://savannah.gnu.org/bugs/?49734)
- **rotate3d** compatibility with Matlab, [in
  progress](https://savannah.gnu.org/bugs/?49747)
- \<s\>Output of **uicontrol**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?49751)
- \<s\>**isequalwithequalnans** is missing.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50113)
- \<s\>**fwrite** input argument type.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50157)
- \<s\>**set()** is case-sensitive.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50163)
- \<s\>Update list of **missing functions**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50222)
- **sum, cumsum**, etc. mishandle integer inputs, [in
  progress](https://savannah.gnu.org/bugs/?50238)
- \<s\>Error using print and the **append** flag.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50318)
- \${cmd} replacement operator in **regexprep**, [in
  progress](https://savannah.gnu.org/bugs/?50329)
- \<s\>Position of **uimenu**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50369)
- \<s\>**Loading of figures** (and other objects) from Octave IDE.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50543)
- \<s\>**orderfields** is slow.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50688)
- \<s\>Undefined input to a **classdef** method.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?50716)
- zeros: **like** keyword, [in
  progress](https://savannah.gnu.org/bugs/?50854)
- textscan option **MultipleDelimsAsOne** does not apply to space or tab
  characters, [in progress](https://savannah.gnu.org/bugs/?51093)
- \<s\>**ismember** fails if the string ends in a space.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51187)
- \<s\>Infinite loop in **normest1**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51241)
- \<s\>**pinv(0)** different from Matlab.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51246)
- \<s\>**center()** relies on broadcasting.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51249)
- \<s\>**private** function in **classdef** file.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51276)
- **max_recursion_depth** error in classdef constructor, [in
  progress](https://savannah.gnu.org/bugs/?51285)
- \<s\>**isequal** is slow.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51403)
- display of multidimensional arrays uses **\'ans**\', [in
  progress](https://savannah.gnu.org/bugs/?51410)
- **strcmp** with multidimensional char arrays, [in
  progress](https://savannah.gnu.org/bugs/?51412)
- Implementation of class **categorical**, [in
  progress](https://savannah.gnu.org/bugs/?51516)
- Support for \"**import**\" keyword, [in
  progress](https://savannah.gnu.org/bugs/?51688)
- \<s\>**modal windowstyle** property not working.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?51846)
- \<s\>Missing keyword **help** for classdef keywords.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?52591)
- \<s\>**makeValidName** and **makeUniqueStrings**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?52596)
- \<s\>PDF user manual uses a **backward apostrophe** \` in code
  examples.\</s\> [fixed](https://savannah.gnu.org/bugs/?52775)
- Implementation of **histcounts**, [in
  progress](https://savannah.gnu.org/bugs/?52855)
- Implement **jsondecode**, **jsonencode** functions, [in
  progress](https://savannah.gnu.org/bugs/?53100)
- Figure property **IntegerHandle** does not work fully with Qt toolkit,
  [in progress](https://savannah.gnu.org/bugs/?53342)
- \<s\>\"**Number**\" property for figure.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53343)
- \<s\>Implementation of **movegui**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53400)
- \<s\>Implementation of **savefig**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53403)
- Octave 4.3.0+ can\'t **load** figures saved with previous versions,
  [in progress](https://savannah.gnu.org/bugs/?53468)
- \<s\>Unknown command \`**codequoteundirected**\' in help text.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53473)
- \<s\>warning message when **qcollectiongenerator** and
  **qhelpgenerator** are not found.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53474)
- \<s\>Syntax of documentation with **texinfo** 4.13.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53479)
- \<s\>Segmentation fault when executing a **script** containing a
  figure.\</s\> [fixed](https://savannah.gnu.org/bugs/?53487)
- \<s\>warning from **opengl_renderer** about light object.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53511)
- \<s\>uicontrol/uibuttongroup: **focusing** not implemented yet.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53710)
- octave deadlocks with **deletefcn** callback that calls graphical
  function, [in progress](https://savannah.gnu.org/bugs/?53802)
- \<s\>doc: some **default Qt properties** are different between
  systems.\</s\> [fixed](https://savannah.gnu.org/bugs/?53805)
- \<s\>**MEX file** in a **private** directory of a class.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?53856)
- \<s\>**ismember** error with mixed numeric and char arrays
  inputs.\</s\> [fixed](https://savannah.gnu.org/bugs/?53924)
- \<s\>Error with **whos -file**: \'load\' not found.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54030)
- Behavior of **open** with unknown or non-existing files, [in
  progress](https://savannah.gnu.org/bugs/?54064)
- \<s\>Colors of a **uicontrol pushbutton**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54065)
- \<s\>**uipanel** doesn't show border in linux.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54189)
- \<s\>**camlight** (axis_handle, \...) should work for Matlab
  Compatibility.\</s\> [fixed](https://savannah.gnu.org/bugs/?54372)
- \<s\>Implementation of **isfolder**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54456)
- **Private** directory in **+package**, [in
  progress](https://savannah.gnu.org/bugs/?54658)
- \<s\>Figure\'s Position when **MenuBar** is none.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54678)
- **load()** should issue an error if specified variable does not exist
  in file, [in progress](https://savannah.gnu.org/bugs/?54736)
- **subsasgn** call when the subscripted expression contains the end
  keyword, [in progress](https://savannah.gnu.org/bugs/?54783)
- Implementation of **memmapfile**, [in
  progress](https://savannah.gnu.org/bugs/?54788)
- **File browser** unresponsive, [in
  progress](https://savannah.gnu.org/bugs/?54885)
- \<s\>uicontrol: validation of **cdata** property.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54918)
- \<s\>**savefig** should accept a vector of figure handles for Matlab
  compatibility.\</s\> [fixed](https://savannah.gnu.org/bugs/?54924)
- \<s\>**colormap** property of a figure cannot be empty.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54926)
- Interpreter cannot find **methods** in files of classdefs in
  **packages**, [in progress](https://savannah.gnu.org/bugs/?54941)
- \<s\>Implement uicontrol **focusing** behavior.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54942)
- \<s\>Use of **camlight** when a patch is not visible.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?54970)
- Removal of **called_from_builtin**, [in
  progress](https://savannah.gnu.org/bugs/?54995)
- \<s\>Property **VertexNormals** not updated.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55014)
- \<s\>**gunzip/bunzip2** error with cell array of strings input.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55102)
- \<s\>DOCSTRING macro does not recognize
  **matlab.lang.makeValidName**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55209)
- \<s\>Change of a **togglebutton** uicontrol\'s value not reflected
  graphically.\</s\> [fixed](https://savannah.gnu.org/bugs/?55211)
- **Global** variable in a **MEX** file, [in
  progress](https://savannah.gnu.org/bugs/?55363)
- \<s\>warning: **popupmenu** value not within valid display
  range.\</s\> [fixed](https://savannah.gnu.org/bugs/?55368)
- Speed issue with **uicontrols**, [in
  progress](https://savannah.gnu.org/bugs/?55418)
- \<s\>gcbf and **HandleVisibility** property.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55428)
- \<s\>Add support for more types for image\'s **cdata**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55436)
- Binary input image for **edge**, [in
  progress](https://savannah.gnu.org/bugs/?55438)
- colorbar properties need **listeners** to invoke actions, [in
  progress](https://savannah.gnu.org/bugs/?55441)
- \<s\>Implementation of **lightangle**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55446)
- \<s\>\[MXE Octave\] lib vs **lib64**.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55608)
- **Path** management in the GUI, [in
  progress](https://savannah.gnu.org/bugs/?55623)
- \<s\>**isosurface** is slow.\</s\>
  [fixed](https://savannah.gnu.org/bugs/?55642)
- \<s\>Image not displayed with **YDir** set to normal.\</s\>
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

- mkoctfile\'s option to define output file name is \"-o\" or
  \"\--output\" while mex\'s option is \"-output\". If used, no file
  extension (\'.mex\') is appended.
- **computer.m** returns different strings than on MATLAB (PCWIN,
  GLNX86, PCWIN64, GLNXA64, MACI64), e.g. x86_64-unknown-linux-gnu.
- **load.m** and **save.m** automatically add a *.mat* file extension if
  not provided with MATLAB (Octave doesn\'t).

### Miscellaneous

- **exist(\'OCTAVE_VERSION\',\'builtin\')** can be used to detect if
  running in Octave or MATLAB.
