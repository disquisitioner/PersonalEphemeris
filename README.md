# Personal Ephemeris - Observing the Sun, Moon and Planets

As an amateur astronomer I like knowing what's going on in the night sky.  There are a number of web sites that will tell you bits and pieces of what I'd like to know, and an assortment of applications for mobile phones and tables that attempt to do so as well.  None of them was exactly what I was looking for, so I decided to write my own in Python.

Happily I discovered the excellent [PyEphem package](http://rodesmill.org/pyephem), which provides Python classes and functions atop the excellent [XEphem](http://www.clearskyinstitute.com/xephem) library by Elwood Downey.

## Usage 

Personal Ephemeris needs to know the location for which you would like the observing information so it can use proper latitude, longitude, and altitude information in its calculations.  PyEphem has a built-in database of over 100 cities, and also provides a mechanism for adding your own custom locations.  Personal Ephemeris uses that mechansim to let users calculate observations for additional cities.

By default it will show you visibilty information at the current local time (based on the clock of your computer) but you can also specify a date and time to be used for the visibility calculations.

Either or both of these aspects can be configured using command-line options as follows:

```
ephemeris.py [--city 'city'] [--date 'YYYY/MM/DD HH:MM']
```

## Sample Output

    ***** Currently at Los Gatos (03/09/2013 17:48:48) *****
    
    *** Sun and Moon ***
           BODY        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    Sun............... | Yes |   3:34 | 262:16 | 03/09 06:27 | 03/09 18:10 | -26.8 |
    Moon.............. | No  |   ---  |   ---  | 03/10 06:28 | 03/10 18:25 |  ---  |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    
    *** Lunar Phase information: ***
    Current lunar illumination is 3.9%, lunation is 0.9407
    Was just Last Quarter at 03/04/2013 21:52:48.4 UT
    New Moon     : 03/11/2013 19:51:00.1 UT (03/11/2013 12:51:00 Local time)
    First Quarter: 03/19/2013 17:26:34.4 UT (03/19/2013 10:26:34 Local time)
    Full Moon    : 03/27/2013 09:27:18.2 UT (03/27/2013 02:27:18 Local time)
    Last Quarter : 04/03/2013 04:36:33.7 UT (04/02/2013 21:36:33 Local time)
    
    *** Planets ***
           BODY        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    Mercury........... | No  |   ---  |   ---  | 03/10 06:45 | 03/10 18:11 |  ---  |
    Venus............. | No  |   ---  |   ---  | 03/10 07:22 | 03/10 18:45 |  ---  |
    Mars.............. | Yes |  11:35 | 259:17 | 03/09 06:53 | 03/09 18:49 |   1.2 |
    Jupiter........... | Yes |  73:14 | 199:53 | 03/09 10:13 | 03/10 00:35 |  -2.1 |
    Saturn............ | No  |   ---  |   ---  | 03/09 22:10 | 03/10 09:55 |  ---  |
    Uranus............ | Yes |  20:23 | 256:52 | 03/09 07:16 | 03/09 19:34 |   5.9 |
    Neptune........... | No  |   ---  |   ---  | 03/10 06:47 | 03/10 17:44 |  ---  |
    Pluto............. | No  |   ---  |   ---  | 03/10 03:42 | 03/10 13:41 |  ---  |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    
    *** Close Approaches (may not be visible) ***
    Sun     to Mercury =  11:20 (dd:mm)
    Sun     to Venus   =   4:21 (dd:mm)
    Sun     to Mars    =   8:33 (dd:mm)
    Moon    to Mercury =  12:29 (dd:mm)
    Moon    to Neptune =   8:46 (dd:mm)
    Mercury to Venus   =   7:60 (dd:mm)
    Mercury to Neptune =   6:30 (dd:mm)
    Venus   to Mars    =  12:41 (dd:mm)
    Venus   to Neptune =  12:15 (dd:mm)
    Mars    to Uranus  =   9:06 (dd:mm)
    
    *** Special Objects ***
           BODY        | VIS |   ALT  |   AZ   |    RISE     |     SET     |  MAG  |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    C/Pan-STARRS...... | Yes |  14:30 | 251:41 | 03/09 07:36 | 03/09 19:06 |   0.5 |
    C/ISON............ | Yes |  65:56 |  95:48 | 03/09 11:49 | 03/10 04:33 |  15.6 |
    -------------------+-----+--------+--------+-------------+-------------+-------+
    

## Configuration

Several aspects of ephemeris calculation can be customized or extended through additional information provided in a file "objects.json", which will be read by Personal Ephemeris at runtime.  An example version of this file is included here.  It contains:
* Comets of interest, which are specified using orbital elements formatted as used by the Minor Planet Center at http://www.minorplanetcenter.net/iau/Ephemerides/Soft03.html.  Because comet tracking can vary from time to time during their orbits, a 'display' attribute can be true or false to specify whether to show visibility information in the generated output.
* The next visible pass for Earth-orbiting satellites of interest, based on orbital elements in the "TLE" (Two-line Element) format.  A good source of satellite elements in TLE is [Celestrak](http://celestrak.org/NORAD/elements/), which compiles and published data from NORAD.  The International Space Station is a great example of a satellite it can be interesting to track.  Output of satellite visibilty information is configurable using a 'display' attribute as well.
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