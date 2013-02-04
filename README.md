# Scientific setup on Mac
(Currently Mac OSX 10.8 Mountain Lion) 

## Status
The instructions are only complete and tested for Mac OSX lion up to Pandas. I need to test homebrew for the geospatial tools and R.

## Apple software update
- Click Apple Icon (top left) 
- Select Software Update ...

## XCode
### Minimalist Xcode with command line tools (preferred) https://developer.apple.com/downloads/index.action
### Full Xcode (12G)
Use the App Store (access from dock or Applications)
Apple developer website

## Paths
- Lion uses the /private directory rather than .bash_profile
    sudo nano /private/etc/paths
- put /usr/local/bin 1st

```
# Example of paths file
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
/Library/Frameworks/GDAL.framework/Programs
/usr/local/pgsql-9.1/bin
```

## Install software with the Homebrew package manager

Install [http://mxcl.github.com/homebrew/](Homebrew)

    ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"

    IMPORTANT: add /usr/local/bin to **top** of /private/etc/paths

```
brew install libjpeg
brew install giflib
brew install imagemagick
brew install readline sqlite gdbm pkg-config
brew install python --framework --universal
```

add: /usr/local/share/python to /private/etc/paths

change link to **current** Python version

```
cd /System/Library/Frameworks/Python.framework/Versions
sudo rm Current
```
## Setup virtual environments for Python
**Note:** you may need to sub your version number (e.g. 2.7.3)

```
sudo ln -s /usr/local/Cellar/python/2.7.3/Frameworks/Python.framework/Versions/Current
sudo easy_install pip
sudo pip install virtualenv
sudo pip install virtualenvwrapper

source /usr/local/share/python/virtualenvwrapper.sh
# or source /usr/local/bin/virtualenvwrapper.sh
```

Add this source virtualenvwrapper to .bash_profile and restart terminal

Create a new virtual environment (e.g. myenv) then switch (workon) to this environment wish to install Python libraries

```
mkvirtualenv myenv
workon myenv
```

You should now see the name of the environment (e.g. myenv) in the command prompt

### Install the Python libraries

```
pip install numpy
brew install gfortran

pip install -e git+https://github.com/scipy/scipy#egg=scipy-dev

pip install -e git+https://github.com/matplotlib/matplotlib#egg=matplotlib-dev

pip install ipython

brew install qt
```

    **Note:** the pip commands for Sip and PyQT will **FAIL**. Complete the install by hand

### Sip
```
pip install sip
cd ~/.virtualenvs/YOUR_ENV_NAME/build/sip
python configure.py
make
sudo make install
```

### PyQT
```
pip install pyqt
cd ~/.virtualenvs/YOUR_ENV_NAME/build/pyqt
python configure.py
make
sudo make install
```

### clean up
```
cd ~/.virtualenvs/YOUR_ENV_NAME/
rm -r build

brew install zmq

pip install pyzmq

pip install pygments

pip install tornado

# OPTION: brew install PySide
```

## Install Pandas
```
pip install nose cython
pip install pandas
#dev not working right now 19nov12
#pip install -e git+https://github.com/wesm/pandas#egg=pandas
```

## Install statsmodels
```
pip install statsmodels
#dev not working right now 19nov12
#pip install -e git+https://github.com/statsmodels/statsmodels#egg=statsmodels
```

### TEST
    ipython qtconsole --pylab=inline

### Intall X11
    http://xquartz.macosforge.org/landing/

### Experiment with the science brew tap (untested)
   https://github.com/Homebrew/homebrew-science

### Add doc conversion for ipython notebook 
    NBConvert is in active dev and not yet available via package manager

### Dependencies
```
pip install markdown
curl http://docutils.svn.sourceforge.net/viewvc/docutils/trunk/docutils/?view=tar > docutils.tgz
pip install -U docutils.tgz
```

### Install Pandoc
    http://johnmacfarlane.net/pandoc/installing.html

### Install NBConvert
    git clone git://github.com/ipython/nbconvert.git


## Intall Pygame
```
brew install sdl sdl_image sdl_mixer sdl_ttf smpeg portmidi 
## Download the dmg b/c pip not working with headers
brew install sdl sdl_image sdl_mixer sdl_ttf smpeg portmidi 
pip install hg+http://bitbucket.org/pygame/pygame

```

## Settings preferences
- Turn on left ctl caps switch (Keyboard Preferences > Modifier Keys)
- Finder>View>Show Status bar
- Preferences Dock > Auto-hide
- Drag Downloads to sidebar
- Terminal change to pro with 100% opacity
- Change machine name (hostname) (system preferences > sharing)


## Emacs/Auctex (install with homebrew) .. instead of healy (below)
```
export HOMEBREWW_KEEP_INFO=1

brew install emacs --cocoa --srgb

brew install auctex
```

## Mactex
- Install MacTeX from http://www.tug.org/mactex/
- Add the MacTeX directory to your path. For me it is /usr/local/texlive/2010/bin/x86_64-darwin/ for 64-bit Intel or /usr/local/texlive/2010/bin/universal-darwin/ for everyone else
- To make MacTeX play nice with Homebrew, change the owner of all files in /usr/local "sudo chown -R $USER:staff /usr/local"
- Install HeVeA "brew install hevea"
- Symlink HeVeA so that MacTeX can find it "ln -s /usr/local/lib/hevea /usr/local/texlive/texmf-local/tex/latex/hevea"
- Run "mktexlsr" so that MacTeX finds HeVeA


## Install Emacs 
Use Home Brew (above)
```
brew install curl
brew install aspell
brew install ack
```

### edit .emacs file
```
(require 'package)
(add-to-list 'package-archives
'("melpa" . "http://melpa.milkbox.net/packages/") t)
```

## add these to bash_profile
```
echo "emacs --daemon"
alias e=emacsclient -t
alias ec=emacsclient -c
alias vim=emacsclient -t
alias vi=emacsclient -t
```

add emacs packages
    M-x package-install [RET] ess [RET]

Install prelude
    PRELUDE_INSTALL_DIR="$HOME/.emacs.d" && curl -L https://github.com/bbatsov/prelude/raw/master/utils/installer.sh | sh

OLDER instructions
- Then follow [[http://kieranhealy.org/emacs-starter-kit.html][Kieren's Guide]] for installation and .emacs configs
- wget http://alpha.gnu.org/gnu/emacs/pretest/emacs-24.0.95.tar.gz

#./configure --x-includes=/usr/X11/include --x-libraries=/usr/X11/lib

## R
http://cran.r-project.org/

### RGDAL, from R
#downloaded from kyngchaos
- open dmg drag tgz to downloads
- then install from local source and select tgz

 this wasn't working
```
> setRepositories(ind=1:2)
>install.packages('rgdal')
```

===================================================================================


NOTE: some of the following instructions need to be updated

## Geo tools
### GDAL framework, QGIS, PostgreSQL/PostGIS, 

    brew install postgresql

Create db:

    initdb /usr/local/var/postgres -E utf8

If you have trouble with permissions, check that usr/local/var has group staff. If not

    sudo chown -R root:staff /usr/local/var

If you still have problems then make your username the owner of /usr/local/var

    sudo chown -R $USER:staff /usr/local/var


Test open a dbase:

    psql -d postgres

(Ctrl-D to exit)


    brew install postgis

    brew install --HEAD osm2pgsql


=============

## Update Ruby with Homebrew
```
brew install ruby
sudo gem install bundle
```

## Install RVM for gem management
    \curl -L https://get.rvm.io | bash -s stable --ruby

## Enable Apache

    sudo chown u+w /etc/apache2/httpd.conf

then emacs and add:

    ServerName localhost

## References
- Solution to Pyqt
http://blog.adamdklein.com/?p=416
[Homebrew: Installing Python, virtualenv, NumPy, SciPy, matplotlib and IPython on Lion](http://www.thisisthegreenroom.com/2011/installing-python-numpy-scipy-matplotlib-and-ipython-on-lion/)




