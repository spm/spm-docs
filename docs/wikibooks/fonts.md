## SPM-FONT-FIXES

Some users may experience problems with fonts being too small. This
entry is a compilation of suggested fixes.

**Note:** Some of these these fixes may work on some platforms and not
others. You may have to combine several fixes.

\<h4\>Fix \#1\<ref\>\</ref\>\</h4\> This fix has worked on some Windows
machines, but does not resolve all problems.

1.  Open spm.m
    - This file is generally located under ~/spm8/spm.m on \*NIX
      systems, and under C:\spm\spm8\spm.m on Windows
2.  On or around line 631 replace:

`   if nargin<2, FS=1:36; else FS=varargin{2}; end`

with:

`   if nargin<2, FS=2:36; else FS=varargin{2}; end`

\<h4\>Fix \#2\<ref\>\</ref\>\</h4\>

1.  Open spm.m
    - This file is generally located under ~/spm8/spm.m on \*NIX
      systems, and under C:\spm\spm8\spm.m on Windows
2.  On or around line 636 replace:

`   sf  = offset + 0.85*(min(spm('WinScale'))-1);`

with:

`   sf  = offset - 0.85*(min(spm('WinScale'))-1);`

- 

Combining Fix \#1 and Fix \#2 may resolve the problem in some
situations.\<ref\>\</ref\>

\<h4\>Mac specific fix\</h4\> If you are using a Mac open up the spm.m
file as indicated above, scroll to line 664 and replace

`   offset     = 1;`

with:

`   offset     = 1.4;`

and this will solve all your font issues.

Other suggestions for font and display issues have included turning on
the Graphics window resize property and viewing the output after
printing to a ps file.\<ref\>\</ref\>

## References

Return to [SPM](SPM "wikilink")
