### Based on the code for <a href="http://crayonmap.herokuapp.com">Crayon Map</a>

# drawn-layer

An extension of Leaflet's canvas tile layer and Mapzen's vector tiles, to help you make
colorful, custom-rendered maps.

Library includes GitHub's open source <a href="https://github.com/github/fetch">polyfill for fetch</a>

Minified version created by jsmin.

## Example

Suppose you wanted to draw shapes where OpenStreetMap returned landuse with ```kind=residential```

```javascript
var map = L.map('map');

// pass these values
L.drawnLayer('MAPZEN_KEY', 'landuse', function(geojson, canvasctx) {
  for (var f = 0; f < geojson.features.length; f++) {
    var feature = geojson.features[f];
    if (feature.properties.kind === 'residential') {
      CustomDrawFunction(feature.geometry, canvasctx);
    }
  }
}).addTo(map);

// your custom renderer
function CustomDrawFunction(geometry, canvasctx) {
  // only draw outer ring of polygons
  var coords = (geometry.type === 'Polygon') ? geometry.coordinates[0] : geometry.coordinates;

  // draw shape
  canvasctx.beginPath();
  canvasctx.moveTo(coords[0].x, coords[0].y);
  for (var pt = 1; pt < coords.length; pt++) {
    canvasctx.lineTo(coords[pt].x, coords[pt].y);
  }
  canvasctx.closePath();

  // fill in the shape
  canvasctx.fillStyle = 'darkgreen';
  canvasctx.fill();

  // if you just wanted to draw an outline
  canvasctx.strokeStyle = 'darkgreen';
  canvasctx.stroke();
}
```

## License

Open source, MIT license
