Forked from dcthomson and added a few enchancements

`mkvdts2ac3.py` is a python script for linux, windows or os x which can be used
for converting the DTS in Matroska (MKV) files to AC3. It provides you with a set
of options for controlling the resulting file.

A recreation of mkvdts2ac3.sh which was created by Jake Wharton and Chris Hoekstra.
I figured that most people are using this with sabnzbd which requires python so a
non-os specific solution would help people out (as well as myself). There are a
few extras I have added such as, sabnzbd support, nzbget support, recursively decending
directories, and overwrite mode.

Installation
============

Prerequisites
-------------
Make sure the executables for the following libraries are accessible.

1. [python](http://www.python.org/) - Python
2. [mkvtoolnix](http://www.bunkus.org/videotools/mkvtoolnix/) - Matroska tools
3. [ffmpeg](http://ffmpeg.org/) - Audio conversion tool

*Note: If you are a Mac OS X user you may need to compile these libraries.*

Installation
------------
If you have `git` installed, you can just run
`git clone https://github.com/jobrien2001/mkvdts2ac3.py.git`.

You can download the script directly with wget or curl:

wget https://raw.githubusercontent.com/jobrien2001/mkvdts2ac3.py/master/mkvdts2ac3.py

  -or-

curl -O https://raw.githubusercontent.com/jobrien2001/mkvdts2ac3.py/master/mkvdts2ac3.py

Otherwise you can click the "Download" link on the GitHub project page and
download an archive and extract its contents.

Optional: If you want easy access to the script from any directory you can copy
or symlink the `mkvdts2ac3.py` file to a directory in your PATH variable or else
append the script's directory to the PATH variable.

Usage
=====

<pre>
usage: mkvdts2ac3.py [-h] [--aac] [--aaccustom TITLE]
                     [--aacchannelbitrate AACCHANNELBITRATE]
                     [--aacmaxchannels AACMAXCHANNELS]
                     [--channelbitrate CHANNELBITRATE]
                     [--maxchannels MAXCHANNELS] [-c TITLE] [-d]
                     [--destdir DIRECTORY] [-e] [-f] [--ffmpegpath DIRECTORY]
                     [-k] [--md5] [--mp4] [--mkvtoolnixpath DIRECTORY] [-n]
                     [--new] [--no-subtitles] [-o]
                     [-p {initial,last,afterdts}] [-r] [-s MODE]
                     [--sabdestdir DIRECTORY] [-t TRACKID] [--all-tracks]
                     [-w FOLDER] [-v] [-V] [--test] [--debug]
                     FileOrDirectory [FileOrDirectory ...]

convert matroska (.mkv) video files audio portion from dts to ac3

positional arguments:
  FileOrDirectory       a file or directory (wildcards may be used)

optional arguments:
  -h, --help            show this help message and exit
  --aac                 Also add aac track
  --aaccustom TITLE     Custom AAC track title
  --aacchannelbitrate AACCHANNELBITRATE
                        AAC Bitrate per channel (default: 80)
  --aacmaxchannels AACMAXCHANNELS
                        Maximum amount of channels of AAC track (default:6)
  --channelbitrate CHANNELBITRATE
                        AC3 Bitrate per channel (default is 80k per channel)
  --maxchannels MAXCHANNELS
                        Maximum amount of channels of AC3 track (default:6)
  -c TITLE, --custom TITLE
                        Custom AC3 track title
  -d, --default         Mark AC3 track as default
  --destdir DIRECTORY   Destination Directory
  -e, --external        Leave AC3 track out of file. Does not modify the
                        original matroska file. This overrides '-n' and '-d'
                        arguments
  -f, --force           Force processing when AC3 track is detected
  --ffmpegpath DIRECTORY
                        Path of ffmpeg
  -k, --keepdts         Keep external DTS track (implies '-n')
  --md5                 check md5 of files before removing the original if
                        destination directory is on a different device than
                        the original file
  --mp4                 create output in mp4 format
  --mkvtoolnixpath DIRECTORY
                        Path of mkvextract, mkvinfo and mkvmerge
  -n, --nodts           Do not retain the DTS track
  --new                 Do not copy over original. Create new adjacent file
  --no-subtitles        Remove subtitles
  -o, --overwrite       Overwrite file if already there. This only applies if
                        destdir or sabdestdir is set
  -p {initial,last,afterdts}, --position {initial,last,afterdts}
                        Set position of AC3 track. 'initial' = First track in
                        file, 'last' = Last track in file, 'afterdts' = After
                        the DTS track [default: afterdts]
  -r, --recursive       Recursively descend into directories
  -s MODE, --compress MODE
                        Apply header compression to streams (See mkvmerge's
                        --compression)
  --sabdestdir DIRECTORY
                        SABnzbd Destination Directory
  -t TRACKID, --track TRACKID
                        Specify alternate DTS track. If it is not a DTS track
                        it will default to the first DTS track found
  --all-tracks          Convert all DTS tracks
  -w FOLDER, --wd FOLDER
                        Specify alternate temporary working directory
  -v, --verbose         Turn on verbose output. Use more v's for more
                        verbosity. -v will output what it is doing. -vv will
                        also output the command that it is running. -vvv will
                        also output the command output
  -V, --version         Print script version information
  --test                Print commands only, execute nothing
  --debug               Print commands and pause before executing each
</pre>


Configuration File
------------------
If you want to set some options as default you can use the configuration file: mkvdts2ac3.cfg
<pre>
[mkvdts2ac3]

# These options take an argument.
# To enable you must remove the # at the beginning of the line and supply the
# argument. 
 
#sabdestdir =
#custom =
#compress =
#destdir =
#track =
#wd =
#verbose =


# These options don't take any arguments, they are only true or false
# false is the default
# remove the # at the beginning of the line to enable the option

#default = True
#external = True
#force = True
#initial = True
#keepdts = True
#nodts = True
#new = True
#overwrite = True
#recursive = True
#test = True
#debug = True
</pre>


SABnzbd
-------
Here is an example of how to set up with SABnzbd

1. make sure that mkvdts2ac3.py is in your sabnzbd post-processing scripts folder

2. set the category Script to mkvdts2ac3.py and Folder/Path to where you want the files downloaded to

    ###Renamer
If you are using something with a renamer / sorter such as Couch Potato set the Folder/Path to any folder you want the downloaded files to be put in while the conversion is being done. Then set the #sabdestdir in the mkvdts2ac3.cfg to the directory that Couch Potato looks for completed downloads.


NZBGet
------
If you do not already have a postprocessing script directory for nzbget:

1. Create the directory on your filesystem (usually 'nzbget MainDir'/ppscripts)

2. Set the path in the web frontend: Settings -> PATHS -> ScriptDir (usually ${MainDir}/ppscripts)

3. Click "Save all Changes" button at the bottom of the page

Now add mkvdts2ac3.py into nzbget

1. Copy mkvdts2ac3.py into your nzbget postprocessing scripts directory

2. Reload nzbget (Settings -> SYSTEM -> Reload)

3. Under settings go to POST-PROCESSING SCRIPTS and add mkvdts2ac3.py to DefScript

4. Click "Save all Changes" button at the bottom of the page

5. Reload nzbget again (Settings -> SYSTEM -> Reload)

    ###Defaults
Now under Settings there should be a MKVDTS2AC3.PY menu item. You can change the defaults there if needed.

  
Developed By
============
* Drew Thomson - <drooby at gmail dot com>

Git repository located at
[github.com/dcthomson/mkvdts2ac3.py](http://github.com/dcthomson/mkvdts2ac3.py)


Very Special Thanks
-------------------
* Jake Wharton
* Chris Hoekstra

License
=======

	Copyright (C) 2012  Drew Thomson
	
	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.
	
	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.
	
	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <http://www.gnu.org/licenses/>.