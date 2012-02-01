---
layout: post
title: Setting up GeoCouch with CouchDB and Homebrew
type: [text]
categories: [mac os x, gis, couchdb]
---

Installing [CouchDB](http://couchdb.apache.org/) with [Homebrew](https://github.com/mxcl/homebrew) is pretty straightforward:

    brew install couchdb

The instructions for installing GeoCouch are easy to follow as well, but due Homebrew, the files have to be copied to different folders. I also noticed that the URL to clone the repository from is not correct in the README file.

**Get GeoCouch**

    git clone https://github.com/couchbase/geocouch.git
    cd geocouch
    git checkout couchdb1.1.x # choose the correct branch for your version

**Get CouchDB source**

from [apache CouchDB](http://couchdb.apache.org/downloads.html) and set `COUCH_SRC` to the directory containing the source, e.g.:

    export COUCH_SRC=/path/to/source/src/couchdb

**Installation**

Run `make` in the directory

    make

Copy the configuration file for GeoCouch from `etc/couchdb/local.d/` to `/usr/local/etc/couchdb/local.d/`

    cp etc/couchdb/local.d/geocouch.ini /usr/local/etc/couchdb/local.d/


**Futon tests**

To make sure your installation is working also copy the Futon tests over (from `share/www/script/test` to `/usr/local/share/couchdb/www/script/test`):

    cp share/www/script/test/* /usr/local/share/couchdb/www/script/test/

Add the tests to `/usr/local/share/couchdb/www/script/couch_tests.js`

    loadTest("spatial.js");
    loadTest("list_spatial.js");
    loadTest("etags_spatial.js");
    loadTest("multiple_spatial_rows.js");
    loadTest("spatial_compaction.js");
    loadTest("spatial_design_docs.js");
    loadTest("spatial_bugfixes.js");
