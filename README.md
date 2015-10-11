# ItemTracks
TF2 item schema decoder with support for multi-lingual attribute descriptions, along with TF2Items and other custom weapon configs.

Compiles items from the Valve TF2 Econ Items api and TF2 resource files into a usable JSON format. Has support for overwriting item attributes and properties with config files from popular mods such as TF2Items.


How to Use:
```php

$Valve_TF2_URL = 'api.steampowered.com/IEconItems_440/GetSchema/v0001/?key=';
//You must get your own API key from: https://steamcommunity.com/dev
$Valve_API_Key = '';
//Create full URL
$Valve_API_URL = $Valve_TF2_URL . $Valve_TF2_Key . '&format=json';

//TF2 resources files are encoded in 'UCS-2', which must be transferred to UTF-8 or other handlable encoding.
//English is required even if you're not using it, some item names and attribute descriptions do not have a translation
$english_utf8 = ucs2_to_utf8('C:\Program Files (x86)\steam\steamapps\common\tf2\tf\resource\tf_english.txt');
$japanese_utf8 = ucs2_to_utf8('C:\Program Files (x86)\steam\steamapps\common\tf2\tf\resource\tf_japanese.txt');

//Parse the files text from VDF into a PHP object
$english_object = vdf_parse($english_utf8);
$japanese_object = vdf_parse($japanese_utf8);

//Create array of languages with their language title, must always be an array even if only english is used.
$languages = array();
$languages['english'] = $english_object;
$languages['japanese'] = $japanese_object;

//For proper HTTPS, a CAcert file is required, http://curl.haxx.se/ca/cacert.pem
$cacertpath = 'cacert.pem';

//Create new ItemTracks object, add in languages and optional CAcert.
//WARNING: No cacert will disable HTTPS
$Tracks = new ItemTracks($languages, $cacertpath, $Valve_API_URL);
```



Custom weapon configs:
ItemTracks has a built-in parser for TF2items.
Example:

The parser will convert the values into a standardized 'ItemTracks Overwrite' config. This allows for almost any custom parser to be created that will overwrite normal TF2 attributes and properties like TF2Items.


VDF Parser
The VDF parser is based on the Python VDF parser, but in this case it is only a single function called 'vdf_parse($string)', which will take in any raw file string in proper VDF and return as an object of its keys and values. The speed of the parser is about 12,000 lines a second.

