Permissions along absolute path
###############################
:date: 2014-01-17
:tags: bash, linux
:slug: rightpath
:author: Ino de Bruijn
:summary: Get the permissions of a file and its path

It happens very often that somebody asks me to share a file on our server. To
give read permissions to somebody, all the directories that have be traversed
to get to the file need to be readable as well. This bash function gives the
permissions of every directory along the full path so one can easily see if the
file is accessible.

.. raw:: html

  <script src="https://gist.github.com/inodb/8470322.js"></script>
