# radiobroadcasts

## Intro

This is a global repository of streaming radio stations and scheduled radio
broadcasts.

The idea behind is keeping track of schedule changes commonly, while allowing
clients of all kinds to use those data.

All radio stations and broadcasts are divided by language of presentation, and
by main topics.

## Format

### Stations

All files in `stations.dist/` must be textfiles following this pattern:

~~~~
# station   name                        URL (comment via #)
~~~~

with those conditions:

* station: short name, [a-z]+ only (lowercase alphabetical chars), must be
  unique for one actual station over all files (but may be repeated)
* name: the official name of the station in double quotes (please make sure
  most people on last.fm use that station name, too)
* URL: direct stream address (optionally followed by `# comments` e. g. for
  official stream URLs)

Example:

~~~~
# station   name                        URL (comment via #)
aligrefm    "Aligre FM"                 http://sv3.vestaradio.com:9468 # http://aligrefm.org/aligrefm.m3u?download
bbc1xtra    "BBC Radio 1Xtra"           http://www.bbc.co.uk/radio/listen/live/r1x_aaclca.pls
bytefm      "ByteFM"                    http://streamingserver01.byte.fm:8000/ # http://www.byte.fm/stream/bytefm.m3u
ckut        "CKUT"                      http://stream.ckut.ca:8000/903fm-192-stereo # http://stream.ckut.ca:8000/903fm-192-stereo.m3u (http://ckut.ca/c/en/node/49 #montreal #gmt-5 #+6hours) **http://ckut.ca/archives.php**
~~~~

### Broadcasts

#### Overview

All files in `broadcasts.dist/` must be textfiles following this pattern:

~~~~
# station   broadcast           cron                year    weeks   min name                    tags                    url
#                               m   h   dom mon dow
~~~~

You will find examples within the `broadcasts.dist` directory.

See next sections for details on each column.

#### station

* corresponding to short names used in `stations.dist`

#### broadcast

* short name, [a-z]+ only (alphabetical, lowercase), each may occur
  several times (for multiple schedules that cannot be covered by one cron
  expression)

#### cron

* generally, see <http://en.wikipedia.org/wiki/Cron>

#### year

* may be `*` for any year, or a year or a year range, like `2012-2016`

#### weeks

* either weeks of month (wom) or weeks of year (woy)
* clients are to detect wom vs. woy automatically

weeks of month (wom):

* for a rhythm restarting every month, e. g. every 2nd TUE of a month
* formats: single number, comma separated, range
* example: `2,4` (every 2nd and 4th weeks of a month)

weeks of year (woy):

* for a fluent rhythm, no matter which week of month
* format: `%i=r` with
    * `i`: week rhythm (1 for each week, 2 for every second week etc.)
    * `r`: remainder (of ISO week number modulus interval)
* note: ISO week number in bash: `date +%V`

#### min

* duration of the broadcast in minutes

#### name

* official broadcast name in double quotes

#### tags

* [a-z]+, comma separated, no spaces
* there should not be more than seven tags per broadcast

#### url

* broadcast website, e. g. for playlist infos (optional)






Entries shall be sorted by those columns: `station`, `dow` (MON .. SUN), `h`, `m`, `weeks`.

*Please note* the `timezone` will be added with the next major version of these files.

Cron examples:

~~~~
0   0   0   1/2 FRI#2 *
~~~~

=> every second Friday of every second month (see <http://en.wikipedia.org/wiki/Cron#Special_characters>)

### Records

The record list is kept here just to give an example on how clients might
handle the selection of broadcasts.

Clients using the stations and broadcasts data should be able to be configured
to record / analyse / ... selected broadcasts only using a `record` file
following this format:

~~~~
# station   broadcast (comment via #)
~~~~

thus just a list of station and broadcast names (the short names used in
`stations.dist` and `broadcasts.dist`, optionally followed by a comment which
can contain short user or client notes on each radio show.

## How to contribute

Please create a pull request with your changes, or contact the maintainers (to
be outlined in each config file).

## Clients

If you maintain or know a client using these files, please let me know and I'll
list it here.

## Vendor usage / packet managers

The repository contains a `composer.json` already for using it as a vendor in
PHP applications. For usage with any other language and package manager, please
contribute the related file by creating a pull request.

