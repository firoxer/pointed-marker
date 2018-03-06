Pointed Marker
==============

A script that generates pointed markers for use in e.g. Google Maps. It uses a template image and ImageMagick to do its job.

### Try it out

You may try the script on my server. Open [this link](http://pointed-marker.oriba.xyz/?label=A&size=80&rotation=75&marker_color=FF0000&label_color=FFFFFF) and change the parameters in the URL as you wish.

![Sample](http://pointed-marker.oriba.xyz/?label=A&size=80&rotation=75&marker_color=FF0000&label_color=FFFFFF)

Sample image generated with `http://pointed-marker.oriba.xyz/?label=A&size=80&rotation=75&marker_color=FF0000&label_color=FFFFFF`

### Prerequisites

You should have installed ImageMagick, ensured that the font Liberation Sans Bold is available (or changed the font in `generate-marker`), cloned the repository, and checked that the user running the script has execute rights to the repository directory.

### CLI

To run the script, execute it with five arguments:

0. label (1–3 characters)
0. rotation (0–360 degrees)
0. size (pixels)
0. marker color (hex)
0. label color (hex)

### Web

To use the script on the web, you may wrap the thing in another language. For example, PHP:

```php
<?php
/**
 * Generate markers using generate-marker
 *
 * Only generates images if they don't exist, and assumes that
 * generate-marker writes them into MARKER_ROOT.
 */

// Same as in generate-marker
define('MARKER_ROOT', 'markers');

$label = $_GET['label'];
$rotation = $_GET['rotation'];
$size = $_GET['size'];
$marker_color = $_GET['marker_color'];
$label_color = $_GET['label_color'];

$file_path = MARKER_ROOT . "/$label-$rotation-$size-$marker_color-$label_color.png";

if (!file_exists($file_path)) {
    // Generate image; subsequent requests will skip this block
    $args = [$label, $rotation, $size, $marker_color, $label_color];
    $args = array_map('escapeshellarg', $args);
    $args = implode(' ', $args);

    $err = shell_exec("./generate-marker $args");

    if ($err) {
        die("Error: $err");
    }
}

header('Content-Type: image/png');
readfile($file_path);
```
