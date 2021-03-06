# d3.geo2rect*hex*
## Morphing geojson polygons into rectangles and hexagons

![DemoClip](https://raw.githubusercontent.com/BobHarper1/d3.geo2recthex/master/thumb.gif)

The package has two modules: compute (function) and draw (class).
For using this one needs to include the d3 and turf.js library.

It extends [Sebastian Meier's d3.geo2rect](https://github.com/sebastian-meier/d3.geo2rect) package to allow morphing into hexagon shapes.

## compute

Compute for morphing a geojson into rectangles or hexagons we need to clean the geojson first. All MultiPolygons will be transformed into Polygons (largest polygon will be used). Holes will be removed. The original data is stored in .geometry.ocoordinates.
Afterwards a second set of coordinates is being generated representing either a rectangle or a hexagon. The rectangle has the bounding box 0,0|1,1 so its easy to transform, whereas the hexagon is a bit more complex. In addition each coordinate has its centroid in the geo-space stored within. The rectangle/hexagon coordinates are stored in .geometry.qcoordinates.

```
d3.json('./data/de.geojson', function(err, data){
  var geojson = geo2rect.compute(data);
});
```

## draw

Draw takes care of drawing the data received from the compute function.

Simply create a new draw instance:
```
var g2r = new geo2rect.draw();
```

Send the configs, including the `shapes` value of either `'hex'` or `'rect'`:
```
var config = {
  width : 700,
  height : 700,
  padding : 70,
  shapes: 'hex',
  projection : d3.geoMercator(),
  duration : 1000,
  key:function(d){return d.properties.Kurz; },
  grid : {
    SH:{x:1,y:0},
    HB:{x:0,y:1},
    HH:{x:1,y:1},
    MV:{x:2,y:1},
    NI:{x:0,y:2},
    BB:{x:1,y:2},
    BE:{x:2,y:2},
    NW:{x:0,y:3},
    ST:{x:1,y:3},
    SN:{x:2,y:3},
    RP:{x:0,y:4},
    HE:{x:1,y:4},
    TH:{x:2,y:4},
    SL:{x:0,y:5},
    BW:{x:1,y:5},
    BY:{x:2,y:5}
  }
};
g2r.config = config;
```
If not set, the default `config.shapes` setting will be for rectangles.

Please note, that this script does not compute the grid layout. See [nmap](https://github.com/sebastian-meier/nmap.js) and [nmap-squared](https://github.com/sebastian-meier/nmap-squared.js) for examples of how to do this automatically.

For hexagons, you can use the same grid as for rectangles, the script will offset the columns so that the regular pattern is achieved.

Then send the computed data from the compute function:
```
g2r.data = geojson;
```

An d3 svg object:
```
g2r.svg = svg.append('g');
```

And then draw the whole thing:
```
g2r.draw();
```

You can simply switch the mode by calling:
```
g2r.toggle();
```

The current mode can be checked via:
```
g2r.mode
```

To update and draw again, simply call the draw function again:
```
g2r.draw();
```

## Examples

In the examples folder you find three examples for the states of Germany and the US as well as London boroughs.

Live examples can be found here:

[Germany](https://cdn.rawgit.com/BobHarper1/d3.geo2recthex/master/example/index_de.html)
[US](https://cdn.rawgit.com/BobHarper1/d3.geo2recthex/master/example/index_us.html)
[London](https://cdn.rawgit.com/BobHarper1/d3.geo2recthex/master/example/index_ldn.html)
