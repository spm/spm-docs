Linux 32 bit is a supported platform up to SPM8. You might have to
recompile SPM MEX files (\*.mexglx) if those provided with the SPM
distribution do not appear to be compatible with your system. The [
Troubleshooting](SPM/Installation_on_64bit_Linux#Troubleshooting "wikilink")
section of the Linux 64 bit installation page might also contain some
useful information.

## SPM12

SPM12 is not officially supported on Linux 32 bit, as this platform is
about to be phased out (see [MATLAB Platform
Roadmap](http://www.mathworks.com/support/sysreq/roadmap.html)). This
means that precompiled MEX files (\*.mexglx) are not included in the SPM
distribution.

## SPM8

### Installation

Download
[spm8.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm8_updates_rxxxx.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm8_updates/)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm8.zip unzip -o
spm8_updates_rxxxx.zip -d spm8 \</nowiki\>\</pre\> Start MATLAB and add
SPM into your path, either using *File \> Set Path \> Add Folder\...* or
typing \<pre\>\<nowiki\> addpath /home/login/spm8 \</nowiki\>\</pre\> in
MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM8 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm8/src make distclean make && make
install make toolbox-distclean make toolbox && make toolbox-install make
external-distclean make external && make external-install
\</nowiki\>\</pre\>

## SPM5

### Installation

Download
[spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updated MEX files
[SPM5_Matlab7.1_LINUX32_MEX.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm5_updates/SPM5_Matlab7.1_LINUX32_MEX.zip)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm5.zip unzip -o
SPM5_Matlab7.1_LINUX32_MEX.zip -d spm5 \</nowiki\>\</pre\> Start MATLAB
and add SPM into your path, either using *File \> Set Path \> Add
Folder\...* or typing \<pre\>\<nowiki\> addpath /home/login/spm5
\</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM5 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm5/src make distclean make && make
install \</nowiki\>\</pre\>

## SPM2

### Installation

Download
[spm2.tar.gz](http://www.fil.ion.ucl.ac.uk/spm/software/download.html)
and its updated MEX files
[SPM2_LINUX32_MEX.tar.gz](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm2_updates/SPM2_LINUX32_MEX.tar.gz)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login tar xvfz spm2.tar.gz tar xvfz
SPM2_LINUX32_MEX.tar.gz -C spm2 \</nowiki\>\</pre\> Start MATLAB and add
SPM into your path, either using *File \> Set Path \> Add Folder\...* or
typing \<pre\>\<nowiki\> addpath /Users/login/spm2 \</nowiki\>\</pre\>
in MATLAB\'s workspace.

### Compilation

In a Terminal, type: \<pre\>\<nowiki\> cd /home/login/spm2 make Linux
\</nowiki\>\</pre\>
