[PHP - Date & Time](https://www.tutorialspoint.com/php/php_date_and_time.htm)

The built-in library of PHP has a wide range of functions that helps in programmatically handling and manipulating date and time information. Date and Time objects in PHP can be created by passing in a string presentation of date/time information, or from the current system's time.

PHP provides the DateTime class that defines a number of methods. In this chapter, we will have a detailed view of the various Date and Time related methods available in PHP.

The date/time features in PHP implements the ISO 8601 calendar, which implements the current leap-day rules from before the Gregorian calendar was in place. The date and time information is internally stored as a 64-bit number.

## Getting the Time Stamp with time()

PHP's time() function gives you all the information that you need about the current date and time. It requires no arguments but returns an integer.

```
time(): int
```


The integer returned by time() represents the number of seconds elapsed since midnight GMT on January 1, 1970. This moment is known as the UNIX epoch, and the number of seconds that have elapsed since then is referred to as a time stamp.

```php
<?php
   print time();
?>
```

It will produce the following **output** −

```
1699421347
```

We can convert a time stamp into a form that humans are comfortable with.

## Converting a Time Stamp with getdate()

The function getdate() optionally accepts a time stamp and returns an associative array containing information about the date. If you omit the time stamp, it works with the current time stamp as returned by time().

The following table lists the elements contained in the array returned by getdate().

|Sr.No|Key & Description|Example|
|---|---|---|
|1|**seconds**<br><br>Seconds past the minutes (0-59)|20|
|2|**minutes**<br><br>Minutes past the hour (0 - 59)|29|
|3|**hours**<br><br>Hours of the day (0 - 23)|22|
|4|**mday**<br><br>Day of the month (1 - 31)|11|
|5|**wday**<br><br>Day of the week (0 - 6)|4|
|6|**mon**<br><br>Month of the year (1 - 12)|7|
|7|**year**<br><br>Year (4 digits)|1997|
|8|**yday**<br><br>Day of year ( 0 - 365 )|19|
|9|**weekday**<br><br>Day of the week|Thursday|
|10|**month**<br><br>Month of the year|January|
|11|**0**<br><br>Timestamp|948370048|

Now you have complete control over date and time. You can format this date and time in whatever format you want.

Take a look at this following **example** −

```php
<?php
   $date_array = getdate();

   foreach ( $date_array as $key => $val ){
      print "$key = $val\n";
   }
   $formated_date  = "Today's date: ";
   $formated_date .= $date_array['mday'] . "-";
   $formated_date .= $date_array['mon'] . "-";
   $formated_date .= $date_array['year'];

   print $formated_date;
?>
```

It will produce the following **output** −

```
seconds = 0
minutes = 38
hours = 6
mday = 8
wday = 3
mon = 11
year = 2023
yday = 311
weekday = Wednesday
month = November
0 = 1699421880
Today's date: 8-11-2023
```

## Converting a Time Stamp with date()

The date() function returns a formatted string representing a date. You can exercise an enormous amount of control over the format that date() returns with a string argument that you must pass to it.

date(string $format, ?int $timestamp = null): string

The date() optionally accepts a time stamp if omitted then current date and time will be used. Any other data you include in the format string passed to date() will be included in the return value.

The following table lists the codes that a format string can contain −

|Sr.No|Format & Description|Example|
|---|---|---|
|1|**a**<br><br>'am' or 'pm' lowercase|pm|
|2|**A**<br><br>'AM' or 'PM' uppercase|PM|
|3|**d**<br><br>Day of month, a number with leading zeroes|20|
|4|**D**<br><br>Day of week (three letters)|Thu|
|5|**F**<br><br>Month name|January|
|6|**h**<br><br>Hour (12-hour format - leading zeroes)|12|
|7|**H**<br><br>Hour (24-hour format - leading zeroes)|22|
|8|**g**<br><br>Hour (12-hour format - no leading zeroes)|12|
|9|**G**<br><br>Hour (24-hour format - no leading zeroes)|22|
|10|**i**<br><br>Minutes ( 0 - 59 )|23|
|11|**j**<br><br>Day of the month (no leading zeroes|20|
|12|**l (Lower 'L')**<br><br>Day of the week|Thursday|
|13|**L**<br><br>Leap year ('1' for yes, '0' for no)|1|
|14|**m**<br><br>Month of year (number - leading zeroes)|1|
|15|**M**<br><br>Month of year (three letters)|Jan|
|16|**r**<br><br>The RFC 2822 formatted date|Thu, 21 Dec 2000 16:01:07 +0200|
|17|**n**<br><br>Month of year (number - no leading zeroes)|2|
|18|**s**<br><br>Seconds of hour|20|
|19|**U**<br><br>Time stamp|948372444|
|20|**y**<br><br>Year (two digits)|06|
|21|**Y**<br><br>Year (four digits)|2006|
|22|**z**<br><br>Day of year (0 - 365)|206|
|23|**Z**<br><br>Offset in seconds from GMT|+5|

Take a look at this following **example** −

```php
<?php
   print date("m/d/y G.i:s \n", time()) . PHP_EOL;
   print "Today is ";
   print date("j of F Y, \a\\t g.i a", time());
?>
```

It will produce the following **output** −

```
11/08/23 11.23:08

Today is 8 2023f November 2023, at 11.23 am
```

## Runtime Configuration

The behavior of the these functions is affected by settings in php.ini. All these parameters are available in PHP version 5 and onwards.

Date/Time configuration options:

|Name|Default|Description|Changeable|
|---|---|---|---|
|date.default_latitude|"31.7667"|Specifies the default latitude.|PHP_INI_ALL|
|date.default_longitude|"35.2333"|Specifies the default longitude|PHP_INI_ALL|
|date.sunrise_zenith|"90.83"|Specifies the default sunrise zenith|PHP_INI_ALL|
|date.sunset_zenith|"90.83"|Specifies the default sunset zenith|PHP_INI_ALL|
|date.timezone|""|Specifies the default timezone|PHP_INI_ALL|

**PHP** − indicates the earliest version of PHP that supports the function.

|Sr.No|Function & Description|PHP|
|---|---|---|
|1|[checkdate()](https://www.tutorialspoint.com/php/php_function_checkdate.htm)<br><br>Validates a Gregorian date|3|
|2|[date_create()](https://www.tutorialspoint.com/php/php_function_date_create.htm)<br><br>Returns new DateTime object|5|
|3|[date_date_set()](https://www.tutorialspoint.com/php/php_function_date_date_set.htm)<br><br>Sets the date|5|
|4|[date_default_timezone_get()](https://www.tutorialspoint.com/php/php_function_date_default_timezone_get.htm)<br><br>Returns the default time zone|5|
|5|[date_default_timezone_set()](https://www.tutorialspoint.com/php/php_function_date_default_timezone_set.htm)<br><br>Sets the default time zone|5|
|6|[date_format()](https://www.tutorialspoint.com/php/php_function_date_format.htm)<br><br>Returns date formatted according to given format|5|
|7|[date_isodate_set()](https://www.tutorialspoint.com/php/php_function_date_isodate_set.htm)<br><br>Sets the ISO date|5|
|8|[date_modify()](https://www.tutorialspoint.com/php/php_function_date_modify.htm)<br><br>Alters the timestamp|5|
|9|[date_offset_get()](https://www.tutorialspoint.com/php/php_function_date_offset_get.htm)<br><br>Returns the daylight saving time offset|5|
|10|[date_parse()](https://www.tutorialspoint.com/php/php_function_date_parse.htm)<br><br>Returns associative array with detailed info about given date|5|
|11|[date_sun_info()](https://www.tutorialspoint.com/php/php_function_date_sun_info.htm)<br><br>Returns an array with information about sunset/sunrise and twilight begin/end.|5|
|12|[date_sunrise()](https://www.tutorialspoint.com/php/php_function_date_sunrise.htm)<br><br>Returns the time of sunrise for a given day / location|5|
|13|[date_sunset()](https://www.tutorialspoint.com/php/php_function_date_sunset.htm)<br><br>Returns the time of sunset for a given day / location|5|
|14|[date_time_set()](https://www.tutorialspoint.com/php/php_function_date_time_set.htm)<br><br>Sets the time|5|
|15|[date_timezone_get()](https://www.tutorialspoint.com/php/php_function_date_timezone_get.htm)<br><br>Return time zone relative to given DateTime|5|
|16|[date_timezone_set()](https://www.tutorialspoint.com/php/php_function_date_timezone_set.htm)<br><br>Sets the time zone for the DateTime object|5|
|17|[date()](https://www.tutorialspoint.com/php/php_function_date.htm)<br><br>Formats a local time/date|3|
|18|[getdate()](https://www.tutorialspoint.com/php/php_function_getdate.htm)<br><br>Returns an array that contains date and time information for a Unix timestamp|3|
|19|[gettimeofday()](https://www.tutorialspoint.com/php/php_function_gettimeofday.htm)<br><br>Returns an array that contains current time information|3|
|20|[gmdate()](https://www.tutorialspoint.com/php/php_function_gmdate.htm)<br><br>Formats a GMT/UTC date/time|3|
|21|[gmmktime()](https://www.tutorialspoint.com/php/php_function_gmmktime.htm)<br><br>Returns the Unix timestamp for a GMT date|3|
|22|[gmstrftime()](https://www.tutorialspoint.com/php/php_function_gmstrftime.htm)<br><br>Formats a GMT/UTC time/date according to locale settings|3|
|23|[idate()](https://www.tutorialspoint.com/php/php_function_idate.htm)<br><br>Formats a local time/date as integer|5|
|24|[localtime()](https://www.tutorialspoint.com/php/php_function_localtime.htm)<br><br>Returns an array that contains the time components of a Unix timestamp|4|
|25|[microtime()](https://www.tutorialspoint.com/php/php_function_microtime.htm)<br><br>Returns the microseconds for the current time|3|
|26|[mktime()](https://www.tutorialspoint.com/php/php_function_mktime.htm)<br><br>Returns the Unix timestamp for a date|3|
|27|[strftime()](https://www.tutorialspoint.com/php/php_function_strftime.htm)<br><br>Formats a local time/date according to locale settings|3|
|28|[strptime()](https://www.tutorialspoint.com/php/php_function_strptime.htm)<br><br>Parses a time/date generated with strftime()|5|
|29|[strtotime()](https://www.tutorialspoint.com/php/php_function_strtotime.htm)<br><br>Parses an English textual date or time into a Unix timestamp|3|
|30|[time()](https://www.tutorialspoint.com/php/php_function_time.htm)<br><br>Returns the current time as a Unix timestamp|3|
|31|[timezone_abbreviations_list()](https://www.tutorialspoint.com/php/php_function_timezone_abbreviations_list.htm)<br><br>Returns associative array containing dst, offset and the timezone name|5|
|32|[timezone_identifiers_list()](https://www.tutorialspoint.com/php/php_function_timezone_identifiers_list.htm)<br><br>Returns numerically index array with all timezone identifiers|5|
|33|[timezone_name_from_abbr()](https://www.tutorialspoint.com/php/php_function_timezone_name_from_abbr.htm)<br><br>Returns the timezone name from abbrevation|5|
|34|[timezone_name_get()](https://www.tutorialspoint.com/php/php_function_timezone_name_get.htm)<br><br>Returns the name of the timezone|5|
|35|[timezone_offset_get()](https://www.tutorialspoint.com/php/php_function_timezone_offset_get.htm)<br><br>Returns the timezone offset from GMT|5|
|36|[timezone_open()](https://www.tutorialspoint.com/php/php_function_timezone_open.htm)<br><br>Returns new DateTimeZone object|5|
|37|[timezone_transitions_get()](https://www.tutorialspoint.com/php/php_function_timezone_transitions_get.htm)<br><br>Returns all transitions for the timezone|5|
|38|[date_add()](https://www.tutorialspoint.com/php/php_function_date_add.htm)<br><br>Adds an interval to a date.|5.3|
|39|[date_create_from_format()](https://www.tutorialspoint.com/php/php_function_date_create_from_format.htm)<br><br>Creates a date by parsing a timestring according to a specified format.|5.3|
|40|[date_diff()](https://www.tutorialspoint.com/php/php_function_date_diff.htm)<br><br>Calculates and returns the difference between two dates.|5.3|
|41|[date_parse_from_format()](https://www.tutorialspoint.com/php/php_function_date_parse_from_format.htm)<br><br>Returns information about the given date according to the specified format.|5.3|
|42|[date_parse()](https://www.tutorialspoint.com/php/php_function_date_parse.htm)<br><br>Returns an array congaing info about the given date.|5.2|
|43|[date_sub()](https://www.tutorialspoint.com/php/php_function_date_sub.htm)<br><br>Subtracts a time interval from a DateTime object.|5.3|
|44|[date_timestamp_get()](https://www.tutorialspoint.com/php/php_function_date_timestamp_get.htm)<br><br>Returns the Unix timestamp|5.3|
|45|[date_timestamp_set()](https://www.tutorialspoint.com/php/php_function_date_timestamp_set.htm)<br><br>Sets the date and time value as per the given timestamp.|5.3|
|46|[date_get_last_errors()](https://www.tutorialspoint.com/php/php_function_date_get_last_errors.htm)<br><br>Returns warnings and errors while creating a DateTime object.|5.3|
|47|[date_interval_create_from_date_string()](https://www.tutorialspoint.com/php/php_function_date_interval_create_from_date_string.htm)<br><br>Creates a date interval from a given string.|5|
|48|[date_interval_format()](https://www.tutorialspoint.com/php/php_function_date_interval_format.htm)<br><br>Formats the given interval.|5.5|
|49|[date_create_immutable_from_format()](https://www.tutorialspoint.com/php/php_function_date_create_immutable_from_format.htm)<br><br>Parses a timestring based on specified format.|5.5|
|50|[date_create_immutable()](https://www.tutorialspoint.com/php/php_function_date_create_immutable.htm)<br><br>Creates and returns a DateTimeImmutable object.|5.5|
|51|[timezone_version_get()](https://www.tutorialspoint.com/php/php_function_timezone_version_get.htm)<br><br>Returns the version of the current timezonedb.|5.3|

## PHP Date / Time Constants

|Sr.No|Constant & Description|
|---|---|
|1|**DATE_ATOM**<br><br>Atom (example: 2005-08-15T16:13:03+0000)|
|2|**DATE_COOKIE**<br><br>HTTP Cookies (example: Sun, 14 Aug 2005 16:13:03 UTC)|
|3|**DATE_ISO8601**<br><br>ISO-8601 (example: 2005-08-14T16:13:03+0000)|
|4|**DATE_RFC822**<br><br>RFC 822 (example: Sun, 14 Aug 2005 16:13:03 UTC)|
|5|**DATE_RFC850**<br><br>RFC 850 (example: Sunday, 14-Aug-05 16:13:03 UTC)|
|6|**DATE_RFC1036**<br><br>RFC 1036 (example: Sunday, 14-Aug-05 16:13:03 UTC)|
|7|**DATE_RFC1123RFC**<br><br>RFC 1123 (example: Sun, 14 Aug 2005 16:13:03 UTC)|
|8|**DATE_RFC2822**<br><br>RFC 2822 (Sun, 14 Aug 2005 16:13:03 +0000)|
|9|**DATE_RSS**<br><br>RSS (Sun, 14 Aug 2005 16:13:03 UTC)|
|10|**DATE_W3C**<br><br>World Wide Web Consortium (example: 2005-08-14T16:13:03+0000)|
|11|**SUNFUNCS_RET_TIMESTAMP**<br><br>Timestamp ( Available in 5.1.2 )|
|12|**SUNFUNCS_RET_STRING**<br><br>Hours:minutes (example: 08:02) ( Available in 5.1.2 )|
|13|**SUNFUNCS_RET_DOUBLE**<br><br>Hours as floating point number (example 8.75)( Available in 5.1.2 )|