# DarkSky International forecast formatting script - multilingual

Note that the DarkSky API only provides Daily forecasts, so no night-time forecast icons are available. While DarkSky provides international versions of the forecast, it is only one short sentence and used as the icon description. The DS-forecast-lang.php provides translation capabilities for Saratoga Template languages for additional text added to the text forecast from data provided by DarkSky API (i.e. Temperature High/Low, Probibility of precipation, Wind direction, speed and gust, UV index ). The DS-forecast-lang.php is provided as a separate script for easy update as new languages are added. Currently the following languages are supported (in addition to English: af,bg,cs,ct,de,dk,el,es,fi,fr,he,hu,it,nl,no,pl,pt,ro,se,si,sk,sr

In order to use this script you need to:

1.  Register for and acquire a free DarkSky API key.
    1.  Browse to [https://darksky.net/dev](https://darksky.net/dev) and sign in to acquire an API key
    2.  insert the API key in **$DSAPIkey** in the DS-forecast.php script or as **$SITE['DSAPIkey']** in _Settings.php_ for Saratoga template users.
    3.  Customize the **$DSforecasts** array (or **$SITE['DSforecasts']** in _Settings.php_) with the location names, latitude/longitude for your forecasts. The first entry will be the default one for forecasts.
2.  Use this script ONLY on your personal, non-commercial weather station website.
3.  Leave attribution (and hotlink) to DarkSky as the source of the data in the output of the script.

Adhere to these three requirements, and you should have fair use of this data from DarkSky.

## Settings in the DS-forecast.php script

```
// Settings ---------------------------------------------------------------
// REQUIRED: a darksky.net API KEY.. sign up at https://darksky.net/dev
$DSAPIkey = 'specify-for-standalone-use-here'; // use this only for standalone / non-template use
// NOTE: if using the Saratoga template, add to Settings.php a line with:
//    $SITE['DSAPIkey'] = 'your-api-key-here';
// and that will enable the script to operate correctly in your template
//
$iconDir ='./forecast/images/';	// directory for carterlake icons './forecast/images/'
$iconType = '.jpg';				// default type='.jpg'
//                        use '.gif' for animated icons fromhttp://www.meteotreviglio.com/
//
// The forecast(s) .. make sure the first entry is the default forecast location.
// The contents will be replaced by $SITE['DSforecasts'] if specified in your Settings.php

$DSforecasts = array(
 // Location|lat,long  (separated by | characters)
'Saratoga|37.27465,-122.02295',
'Auckland|-36.910,174.771', // Awhitu, Waiuku New Zealand
);

//
$maxWidth = '640px';                      // max width of tables (could be '100%')
$maxIcons = 10;                           // max number of icons to display
$maxForecasts = 14;                       // max number of Text forecast periods to display
$maxForecastLegendWords = 4;              // more words in forecast legend than this number will use our forecast words
$numIconsInFoldedRow = 5;                 // if words cause overflow of $maxWidth pixels, then put this num of icons in rows
$autoSetTemplate = true;                  // =true set icons based on wide/narrow template design
$cacheFileDir = './';                     // default cache file directory
$cacheName = "DS-forecast-json.txt";      // locally cached page from DS
$refetchSeconds = 3600;                   // cache lifetime (3600sec = 60 minutes)
//
// Units:
// si: SI units (C,m/s,hPa,mm,km)
// ca: same as si, except that windSpeed and windGust are in kilometers per hour
// uk2: same as si, except that nearestStormDistance and visibility are in miles, and windSpeed and windGust in miles per hour
// us: Imperial units (F,mph,inHg,in,miles)
//
$showUnitsAs  = 'ca'; // ='us' for imperial, , ='si' for metric, ='ca' for canada, ='uk2' for UK
//
$charsetOutput = 'ISO-8859-1';        // default character encoding of output
//$charsetOutput = 'UTF-8';            // for standalone use if desired
$lang = 'en';	// default language
$foldIconRow = false;  // =true to display icons in rows of 5 if long texts are found
// ---- end of settings ---------------------------------------------------
```

For Saratoga template users, you normally do not have to customize the script itself as the most common configurable settings are maintained in your _Settings.php_ file. This allows you to just replace the _DS-forecast.php_ on your site when new versions are released.  
You DO have to add a **$SITE['DSAPIkey'] = '_your-key-here_';** and a **$SITE['DSforecasts] = array( ...);** entries to your _Settings.php_ file to support this and future releases of the script.

<dl>

<dt>**$DSAPIkey = 'specify-for-standalone-use-here';**</dt>

<dd>This setting is for **standalone** use (do not change this for Saratoga templates).  
Register for a DarkSky API Key at **[https://www.darksky.net/account/create](https://www.darksky.net/account/create)** and replace _specify-for-standalone-use-here_ with the registered API key. The script will nag you if this has not been done.  

**For Saratoga template users**, do the registration at the DarkSky API site above, then put your API key in your _Settings.php_ as:  

$SITE['DSAPIkey'] = '_your-key-here_';  

to allow easy future updates of the DS-forecast.php script by simple replacement.</dd>

<dt>**$iconDir**</dt>

<dd>This setting controls whether to display the NOAA-styled icons on the forecast display.  
Set $iconDir to the relative file path to the Saratoga Icon set (same set as used with the WXSIM plaintext-parser.php script).  
Be sure to include the trailing slash in the directory specification as shown in the example above.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['fcsticonsdir']** to specify this value.</dd>

<dt>**$iconType**</dt>

<dd>This setting controls the extension (type) for the icon to be displayed.  
**='.jpg';** for the default Saratoga JPG icon set.  
**='.gif';** for the Meteotriviglio animated GIF icon set.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['fcsticonstype']** to specify this value.</dd>

<dt>**$DSforecasts = array(  
// Location|forecast-URL (separated by | characters)  
'Saratoga|37.27465,-122.02295',  
'Auckland|-36.910,174.771', // Awhitu, Waiuku New Zealand  

...  
);**</dt>

<dd>This setting is the primary method of specifying the locations for forecasts. It allows the viewer to choose between forecasts for different areas based on a drop-down list box selection.  
**Saratoga template users**: Use the _Settings.php_ entry for **$SITE['DSforecasts'] = array(...);** to specify the list of sites and URLs.</dd>

<dt>**$maxWidth**</dt>

<dd>This variable controls the maximum width of the tables for the icons and text display. It may be in pixels (as shown), or '100%'. The Saratoga/NOAA icons are 55px wide and there are up to 8 icons, so beware setting this width too small as the display may be quite strange.</dd>

<dt>**$maxIcons**</dt>

<dd>This variable specifies the maximum number of icons to display in the graphical part of the forecast. Some forecast locations may have up to 8 days of forecast (8 icons) so be careful how wide the forecast may become on the page.</dd>

<dt>**$cacheFileDir**</dt>

<dd>This setting specifies the directory to store the cache files. The default is the same directory in which the script is located.  
Include the trailing slash in the directory specification.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['cacheFileDir']** to specify this value.</dd>

<dt>**$cacheName**</dt>

<dd>This variable specifies the name of the cache file for the DS forecast page.</dd>

<dt>**$refetchSeconds**</dt>

<dd>This variable specifies the cache lifetime, or how long to use the cache before reloading a copy from DarkSky. The default is 3600 seconds (60 minutes). Forecasts don't change very often, so please don't reduce it below 60 minutes to minimize your API access count and keep it to the free Developer API range.</dd>

<dt>**$showUnitsAs**</dt>

<dd>This setting controls the units of measure for the forecasts.  
**='si'** SI units (C,m/s,hPa,mm,km)  
**='ca'** same as si, except that windSpeed and windGust are in kilometers per hour  
**='uk2'** same as si, except that nearestStormDistance and visibility are in miles, and windSpeed and windGust in miles per hour  
**='us'** Imperial units (F,mph,inHg,in,miles)  
**Saratoga template users:** This setting will be overridden by the **$SITE['DSshowUnitsAs']** specified in your _Settings.php_.  
</dd>

<dt>**$foldIconRow**</dt>

<dd>This setting controls 'folding' of the icons into two rows if the aggregate width of characters exceeds the $maxSize dimension in pixels.  
**= true;** is the default (fold the row)  
**= false;** to select not to fold the row.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['foldIconRow']** to specify this value.</dd>

</dl>

More documentation is contained in the script itself about variable names/arrays made available, and the contents. The samples below serve to illustrate some of the possible usages on your weather website.

## Usage samples

```
<?php  
$doIncludeDS = true;  
include("DS-forecast.php"); ?>
```

You can also include it 'silently' and print just a few (or all) the contents where you'd like it on the page

```
<?php  
$doPrintDS = false;  
require("DS-forecast.php"); ?>  
```

then on your page, the following code would display just the current and next time period forecast:

```
<table>  
<tr align="center" valign="top">  
<?php print "<td>$DSforecasticons[0]</td><td>$DSforecasticons[1]</td>\n"; ?>  
</tr>  
<tr align="center" valign="top">  
<?php print "<td>$DSforecasttemp[0]</td><td>$DSforecasttemp[1]</td>\n"; ?>  
</tr>  
</table>
```

Or if you'd like to include the immediate forecast with text for the next two cycles:

```
<table>  
<tr valign="top">  
<?php print "<td align=\"center\">$DSforecasticons[0]<br />$DSforecasttemp[0]</td>\n"; ?>  
<?php print "<td align=\"left\" valign=\"middle\">$DSforecasttext[0]</td>\n"; ?>  
</tr>  
<tr valign="top">  
<?php print "<td align=\"center\">$DSforecasticons[1]<br />$DSforecasttemp[1]</td>\n"; ?>  
<?php print "<td align=\"left\" valign=\"middle\">$DSforecasttext[1]</td>\n"; ?>  
</tr>  
</table>
```

If you'd like to style the output, you can easily do so by setting a CSS for class **DSforecast** either in your CSS file or on the page including the DS-forecast.php (in include mode):


```
<style type="text/css">    
.DSforecast {    
    font-family: Verdana, Arial, Helvetica, sans-serif;    
    font-size: 9pt;    
}    
</style>
```

**Note:** as of DS-forecast.php V1.09, pages that _include_() the DS-forecast.php script **must have an added CSS** in the <head>...</.head> part of the page. You can download the needed CSS from [here](./DS-forecast.css). Either include that inline in the calling page, or add it to the existing CSS for your page.

## Installation of DS-forecast.php

Download **DS-forecast.php** and **DS-forecast-lang.php** .

Download the [ **Icon Set** ](https://saratoga-weather.org/saratoga-icons2.zip) , and upload to /forecast/images directory. This icon set is also used by advforecast2.php, WXSIM plaintext-parser.php and the deprecated WU-forecast.php scripts -- if you'd used any of them, you likely have the correct icon set installed already.

Change settings in **DS-forecast.php** for the **$DSforecast** address(s) and the address of the icons if necessary and upload the modified DS-forecast.php to your website.

Ensure the permission on "DS-forecast.txt" cache file are at least 666 or 766 so the file is writable by the DS-forecst.php script

**Demo:**[ **DS-forecast-demo.php** ](https://saratoga-weather.org/DS-forecast-demo.php)(note: uses UTF-8 only mode)  

If Standalone and running in a custom page:  
**Download:**[ **DS-forecast.css**](./DS-forecast.css) (Version 1.00 - 24-Jan-2019)  

**Download**:[ **Icon Set** ](https://saratoga-weather.org/saratoga-icons2.zip)(upload to your website in the **/forecast/images** directory)  

# Sample output

<img sce="./sample-output.png" alt="sample output">
