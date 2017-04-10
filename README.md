## OSM2VectorTiles Mapbox GL Styles

This project contains all freely available compatible Mapbox GL styles that work with [OSM2VectorTiles](http://osm2vectortiles.org/).

### Glyphs ###

Repository has been also extended with glyphs originated ang generated with [orangemug/font-glyphs](https://github.com/orangemug/font-glyphs) and _Arial Unicode MS Regular_ which is a default fallback font for Mapbox GL. 

```shell
git clone https://github.com/orangemug/font-glyphs.git
cd font-glyphs/
npm install
git submodule update --init --recursive
./generate.sh
```
`generate.sh` is then modified if any custom `.ttf` font is needed for glyph generation.

### Natural Earth Raster ###

Additionally natural earth raster datasource has been added and generated from [lukasmartinelli/naturalearthtiles](https://github.com/lukasmartinelli/naturalearthtiles). 

Jump to releases in [naturalearthtiles/releases](https://github.com/lukasmartinelli/naturalearthtiles/releases). Pick up the desired raster mbtiles bundle. This bundle contains `.webp` raster tiles. They need to be extracted from `.mbtiles` and then converted to `.png` since `.webp` rendering is not fully supported yet.  

```shell
git clone https://github.com/mapbox/mbutil
cd mbutil
./mb-util --image_format=webp ../mapbox-gl-styles/natural_earth_2_shaded_relief.raster.mbtiles ./output/
```
Now jump to [macports/install](https://www.macports.org/install.php) and install mackports. Then:

```shell
sudo port selfupdate
sudo port install webp
find ./ -name "*.webp" -exec dwebp {} -o {}.png \;
``` 
This will result in all `.webp` files now converted to png and available under `.webp.png`

## Serving
The **raster**, **glyphs**, **sprites** are available for hosting as it is. 
We just need to point to them in your `mapbox-gl-style.json`.

Example:

```json
{
	...
	"sprite": "OUR_SERVER_URL/mapbox-gl/sprites/bright-v9",
	"glyphs": "OUR_SERVER_URL/mapbox-gl/glyphs/{fontstack}/{range}.pbf",
	"sources": {
    "natural_earth_raster": {
      "type": "raster",
      "tiles": [
        "OUR_SERVER_URL/mapbox-gl/raster/natural_earth_2_shaded_relief/{z}/{x}/{y}.webp.png"
      ],
      "minzoom": 0,
      "maxzoom": 6
    }
  }
}
```
 
provided that say if we are hosting under Apache, the config which is usually under `/etc/apache2/sites-enabled/000-default.conf` should contain the rule under `<VirtualHost>` like:

```
Alias /mapbox-gl OUR_PATH_TO_MAPBOX_GL_STYLES
<Directory OUR_PATH_TO_MAPBOX_GL_STYLES>
    Order allow,deny
    Allow from all
</Directory>
```
 
## Copyright

The styles Basic and Bright are derived from [mapbox-gl-styles](https://github.com/mapbox/mapbox-gl-styles)
therefore Mapbox Open Styles are copyright (c) 2014, Mapbox, licensed under BSD. 

Natural Earth Tiles from [lukasmartinelli/naturalearthtiles](https://github.com/lukasmartinelli/naturalearthtiles) are licensed under BSD-3-Clause and thus require natural-earth attribution.

