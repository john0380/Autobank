# Autobank
Minimalist Grammar and Treebank Building Software
Autobank version: beta
Created by John Torr, University of Edinburgh, 2015-2018
Email: john.torr@cantab.net
Funded by Nuance Communications Inc and the Engineering and Physical Sciences Reseacrh Council (EPSRC)
All rights reserved


Autobank was originally built and tested on a Mac but has recently also been made compatible with Linux (thanks to Milos Stanojevic for his help with this).  The interface may look slightly different on Linux compared with how it looks on a Mac in the tutorial videos, but the differences should be merely aesthetic.  There are quite a few dependencies required for Autobank to run, and details of how to install these are given below.  If you hit any problems with installation or any bugs in the software please do let me know at the email above.  Please also bear in mind that this is currently a beta version of Autobank so there are likely to still be a few creases here and there that need ironing out.

First, if using a Mac, install Xcode from the App Store (if installing on Linux skip this step and go to the step below where Git gets installed).. if Xcode is already installed, update it to the latest version, again from the App Store.  Then install the command line tools by opening Terminal, and executing the following command:

xcode-select --install

You also need git.  To check if you have it type "git --version" in the terminal.. if you don't it will promp you to install it.

—Now update brew by executing the following commands in the terminal:

sudo chown -R $USER:admin /usr/local

cd /usr/local

git reset --hard origin/master

brew update

-You will need python 2.7.

brew install python 2

-alternatively, if you already have this, you can use 'upgrade' instead of 'install' to get the latest version.

-Note that although OSX comes with python 2.7 already installed, this system version is now out of date.  In particular, pip no longer works properly for many packages, so you will still need to download a more recent version (I am using 2.7.13).  This will not replace your system version (stored at /Library/Frameworks/Python.framework/Versions/2.7/bin/python) but will add a new version alongside it (stored at /usr/local/bin/python).  Do not try to delete the system version as this will still be used by certain internal programs.  Once the new version of Python 2.7 is downloaded, you must also update pip for python 2.7 as follows:

curl https://bootstrap.pypa.io/get-pip.py | python2.7

Note that when you install Python 3 it will probably become your default when running Python scripts using 'python' which we do not actually want.  You can fix this by adding the following line to your ~/.bash_profile file:

alias python=/usr/local/bin/python2.7

-if nltk is not already installed, or it is not nltk version 3, execute the following:

sudo pip2 install -U nltk

-Then install a few things (if they are already installed, you can update to the latest version with e.g. brew upgrade PackageNameHere, or pip2 install PackageNameHere --upgrade):

sudo pip2 install fibonacci-heap-mod

sudo pip2 install simplediff

sudo pip2 install cython

sudo pip2 install jnius

brew install gcc

brew install cmake

brew install mercurial

You should also install java jdk 8 (not 9).  You can get it here: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html.  Make sure that your java and javac are from the same JDK.  (To test this type "java -version" and "javac -version" in a terminal.  Make sure that your $JAVA_PATH is correctly pointing to the right version of Java.  Mine was pointing to version 6 which does not work with the supertagger, so I had to redirect it to version 8.  I did this by entering the following two lines in my .bash_profile file in my home directory:

export PATH=/Library/Java/JavaVirtualMachines/jdk1.8.0_172.jdk/Contents/Home/bin:$PATH
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_172.jdk/Contents/Home

now cd into the folder named Autobank/MGParse/LSTMsupertagger and execute the following command (if the folders lib or dependencies are in this folder then delete them first):

./scripts/install_sbt_and_dynet.sh

then execute:

./dependencies/sbt/bin/sbt assembly

-Now execute:

pip2 install pycrypto

-you may or may not find that nltk has downloaded the relevant corpora when you try to run gen_enriched_PTB.py (see below).. if python throws an error you can download the relevant nltk corpora as follows:

-type ‘idle2’ into the terminal line to open the python IDE.  Then in the window that has opened, type:

import nltk
nltk.download()

-Then navigate to the ‘corpora’ toolbar and download the nombank, propbank and names corpora, as well as wordnet and punkt tokenizer models.

-Next you will need to download CCGbank, the Penn Treebank 3 (make sure the outer folder is called TreeBank3 - rename it if not), and Ontonotes Release 5 from the Linguistic Data Consortium, unless your institution has copies of these already (they probably do)).  Included in the Autobank folder are the Coordination annotation extension for the Penn Treebank that was added by Ficler and Goldberg 2016 (https://github.com/Jess1ca/CoordinationExtPTB), as well as the additional NP annotation by Vadas and Curran 2007 (http://www.cs.usyd.edu.au/~dvadas1/?Noun_Phrases).  I have added the line:

#!/usr/bin/env python2

to the top of Ficler and Goldberg’s run.py file, and if you download it yourself, then do the same.  Move the Ontonotes wsj folder containing the Penn Treebank section folders into TreeBank3/parsed/ontonotes/mrg/.  You can delete the remaining Ontonotes folders now as they will not be needed. You should also delete the file /TreeBank3/parsed/mrg/wsj/MERGE.LOG if it exists. Now execute the following command:

python gen_enriched_PTB.py

This will take about 20 minutes and will synthesise these various annotations to generate a greatly enriched version of the Penn Treebank that serves as the starting point for converting the corpus into a Minimalist Treebank.

The new version of the PTB will be in a folder called ‘UltimateTreebank’.. inside this you’ll find a folder ‘wsj’ which you need to move to the MGParse folder. Now grab the folder data/auto from within the CCGbank folder, rename it to ‘ccg’, and place it inside the MGParse folder; you can delete the rest of the original CCGbank folder system now if you wish.  So the folder MGParse/ccg should now contain the section folder 00, 01, 02 etc, which contain the .auto files.  Note that the first time you start Autobank it will create a new folder called CCGbank with the CCGbank trees slightly modified and in a different format and this is the folder that the system will actually use, but you should keep both ccg and CCGbank.  You can now cd into MGParse from the terminal and start Autobank by executing the following:

python autobank.py <name-of-your-MGbank>

If the name of the MGbank that you give already corresponds to some MGbank you have been working on, it will load that treebank data.. otherwise it will start a new MG treebank project.  I have included my version of MGbank for you to use/modify/play with.  So to open this you type:

python autobank.py MGbank

but to start your own you could type:

python autobank.py wsj ccg myMGbank
