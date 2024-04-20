# Personal Ephemeris - Observing the Sun, Moon and Planets (and More!)

As an amateur astronomer I like knowing what's going on in the night sky.  There are a number of web sites that will tell you bits and pieces of what I'd like to know, and an assortment of applications for mobile phones and tables that attempt to do so as well.  None of them was exactly what I was looking for, so I decided to write my own in Python.

Happily I discovered the excellent [PyEphem package](https://rhodesmill.org/pyephem/index.html), which provides Python classes and functions atop the excellent [XEphem](https://www.clearskyinstitute.com/xephem) library by Elwood Downey.

## Usage 

Personal Ephemeris needs to know the location for which you would like the observing information so it can use proper latitude, longitude, and altitude information in its calculations.  PyEphem has a built-in database of over 100 cities, and also provides a mechanism for adding your own custom locations.  Personal Ephemeris uses that mechansim to let users calculate observations for additional cities.

By default it will show you visibilty information at the current local time (based on the clock of your computer) but you can also specify a date and time to be used for the visibility calculations.

Either or both of these aspects can be configured using command-line options as follows:

```
ephemeris.py [--city 'city'] [--date 'YYYY/MM/DD HH:MM']
```

## Sample Output

```
***** Currently at Los Gatos (04/20/2024 14:16:14) *****

*** Sun and Moon ***
       BODY        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |   RA   |   DEC  |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+
Sun................| Yes |  60:17 | 216:08 | 04/20 06:25 | 04/20 19:48 | -26.8 |  1h57m |  11:55 |
Moon...............| No  |   ---  |   ---  | 04/20 17:00 | 04/21 05:23 |  ---  |  ----  |  ----  |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+

*** Lunar Phase information: ***
Current lunar illumination is 91.4%, lunation is 0.4126
Was just First Quarter at 04/15/2024 19:13:03.0 UT
New Moon     : 05/08/2024 03:21:53.0 UT (05/07/2024 20:21:52 Local time)
First Quarter: 05/15/2024 11:47:55.2 UT (05/15/2024 04:47:55 Local time)
Full Moon    : 04/23/2024 23:48:55.9 UT (04/23/2024 16:48:55 Local time)
Last Quarter : 05/01/2024 11:27:12.0 UT (05/01/2024 04:27:12 Local time)

*** Planets ***
       BODY        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |   RA   |   DEC  |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+
Mercury............| Yes |  48:22 | 230:09 | 04/20 05:51 | 04/20 18:34 |   2.6 |  1h2m |   6:29 |
Venus..............| Yes |  49:53 | 226:31 | 04/20 06:02 | 04/20 18:46 |  -3.8 |  1h14m |   6:17 |
Mars...............| Yes |  25:48 | 242:06 | 04/20 04:55 | 04/20 16:34 |   1.1 | 23h34m |  -4:-8 |
Jupiter............| Yes |  70:03 | 171:25 | 04/20 07:31 | 04/20 21:25 |  -1.9 |  3h18m |  17:28 |
Saturn.............| Yes |  19:21 | 244:19 | 04/20 04:41 | 04/20 16:00 |   1.2 | 23h10m |  -7:-12 |
Uranus.............| Yes |  70:34 | 171:31 | 04/20 07:29 | 04/20 21:26 |   5.8 |  3h18m |  17:58 |
Neptune............| Yes |  31:29 | 239:28 | 04/20 05:10 | 04/20 17:04 |   8.0 | 23h57m |  -1:-40 |
Pluto..............| No  |   ---  |   ---  | 04/21 02:39 | 04/21 12:16 |  ---  |  ----  |  ----  |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+

*** Close Approaches (may not be visible) ***
Sun     to Mercury =  14:23 (dd:mm)
Sun     to Venus   =  11:57 (dd:mm)
Mercury to Venus   =   2:49 (dd:mm)
Mars    to Saturn  =   6:45 (dd:mm)
Mars    to Neptune =   6:09 (dd:mm)
Jupiter to Uranus  =   0:31 (dd:mm)
Jupiter to M45     =   9:44 (dd:mm)
Saturn  to Neptune =  12:54 (dd:mm)
Uranus  to M45     =   9:27 (dd:mm)

*** Special Objects ***
      COMET        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |   RA   |   DEC  |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+
12P/Pons-Brooks....| Yes |  62:24 | 167:59 | 04/20 08:04 | 04/20 21:11 |   4.4 |  3h29m |  10:05 |
-------------------+-----+--------+--------+-------------+-------------+-------+--------+--------+

International Space Station visibility -- Next Pass
   RISE @        AZ      MAX ALT @      AZ        SET @        AZ
-------------+--------+-------------+--------+-------------+--------+
 04/21 10:24 | 160:57 | 04/21 10:28 |   4:09 | 04/21 10:31 |  85:31 |
-------------+--------+-------------+--------+-------------+--------+
```

## Configuration

Several aspects of ephemeris calculation can be customized or extended through additional information provided in a file "objects.json", which will be read by Personal Ephemeris at runtime.  An example version of this file is included here.  It contains:
* Comets of interest, which are specified using orbital elements formatted as used by the Minor Planet Center at http://www.minorplanetcenter.net/iau/Ephemerides/Soft03.html.  Because comet tracking can vary from time to time during their orbits, a 'display' attribute can be true or false to specify whether to show visibility information in the generated output.
* The next visible pass for Earth-orbiting satellites of interest, based on orbital elements in the "TLE" (Two-line Element) format.  A good source of satellite elements in TLE is [CelesTrak](http://celestrak.org/NORAD/elements/), which compiles and published data from NORAD.  The International Space Station is a great example of a satellite it can be interesting to track.  Output of satellite visibilty information is configurable using a 'display' attribute as well.
* Cities to use in calculating visibility information. PyEphem contains a database of over 100 cities worldwide, which may meet your needs but if not it supports extending that set through additional cities defined in the application.  To add a custom city for use with Personal Ephemeris you need to know its longitude and latitude in decimal degrees and its elevation above sea level in meters.  (Country information is included to help disambiguate cities with similar names but isn't used by Personal Ephemeris.)

The object configuration file also lets you specify the default city to be used in calculating ephemerides, simplifying use.

Here's an example
```
{
    "comets": [
        {
            "name" : "12P/Pons-Brooks",
            "db_info" : "12P/Pons-Brooks,e,74.1916,255.8559,198.9889,17.20583,0.0138099,0.95462126,359.8878,04/13.0/2024,2000,g  5.0,6.0",
            "element_source" : "https://minorplanetcenter.net/iau/Ephemerides/Comets/Soft03Cmt.txt",
            "display" : true
        }
    ],
    "satellites" : [
        {
            "name" : "International Space Station",             
            "tle_line1" : "1 25544U 98067A   24104.51883789  .00014574  00000+0  26142-3 0  9998",
            "tle_line2" : "2 25544  51.6398 276.0478 0004666  66.3077  33.1923 15.50152987448515",
            "display" : true
        }
    ],
    "cities" : [
        {
            "name" : "Nashville",
            "latitude" : 36.174465,
            "longitude" : -86.767960,
            "elevation" : 117,
            "country" : "Tennessee, USA"
        },
        {
            "name" : "Manaus",
            "latitude" : "-3.12854",
            "longitude" : "-60.00018",
            "elevation" : 41.4,
            "country" : "Amazonas, Brazil"
        }
    ],
    "default_city" : "Nashville"
}
```


## Version History
* 2.0: April 20, 2024 -- Updated to use Python3 and PyEphem 4.1.5, and introducing expanded configurability through the objects.json file.  Moved into its own separate public repository.
* 1.0: March 3, 2013 -- Made available as part of a collection of Python utilities
* 0.2: February 16, 2023 -- Initial version