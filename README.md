gpx-tools
=========

Some Python scripts to deal with GPX files

gpx-split
---------

Usage: gpx-split <filename>

This script splits out tracks from a GPX file into different files, one file
per date. It removes all non-track items (routes, waypoints, extensions),
so only tracks are retained. It does not modify the original file, but if
a file for a given date already exists, it merges the result into the existing
file.

For example, if you have a GPX file, that contains tracks from several dates
(the date for a track is taken from the <time> tag from the first <trkpt> in
the track), for example 2, 3 and 4 January 2014. Running 'gpx-split' on the
file will give you the following new files:

* 2014-01-02-000.gpx
* 2014-01-03-000.gpx
* 2014-01-04-000.gpx

If you have a second GPX file, that contains tracks for 4 and 5 January, the
following happens:

* tracks from January 4th are merged into 2014-01-04-000.gpx
* a new file named '2014-01-05-000.gpx' is created

If some of the tracks in the second file have the same names as tracks from the
first file, the number of track points in the track is examined, and the higest
number wins. So if the track from the second file has more points than the
existing track, it will completely replace the existing track from the first
file, otherwise it is discarded. No effort is made to merge identically named
tracks together. In other words: if identically named tracks differ in number
of points, the difference is expected to be at the end, not at the beginning.

This is convenient for some Garmin GPS devices (most notably the Zumo), that
create [weird track archives](http://garminzumo.wikispaces.com/BUGS#Triplogs%20&%20tracks%20handling--Double%20data%20in%20archives,%20and/or%20incomplete%20files).


gpx-merge
---------

Usage: gpx-merge <file1> <file2>

This script merges tracks from <file2> into <file1> and optionally deletes
<file2>. If <file1> contains non-track items (routes, waypoints), they are
left untouched, but the script does not regard non-track items from <file2>.

It first analyzes <file1> and then scans <file2> for tracks. If a certain track
name from <file2> does not occur in <file1>, the track is simply appended to
<file1>. If the name does already occur, the number of track points in the
track is examined, and the higest number wins. So if the track from <file2>
has more points than the track from <file1>, it will completely replace the
existing track, and otherwise it is discarded.

The script is interactive, which means that if the merge operation leads to
changes in <file1>, the user is asked whether <file1> may be overwritten and
<file2> removed. If <file1> is not to be modified, the user is still offered
the option to delete <file2>.


gpxtools.py
-----------

The scripts describes above are merely wrappers around functions in the
gpxtools.py module. All the real functionality is in the module.


Dependencies
------------

The scripts are developed and tested on Python 2.7. Other versions are
untested. All XML operations are done with lxml.etree. There are no external
dependencies.

License
-------

The gpx-tools are licensed under the Apache License, version 2.0. A copy of the
license can be found in the 'LICENSE' file and [on the web](http://www.apache.org/licenses/LICENSE-2.0).

