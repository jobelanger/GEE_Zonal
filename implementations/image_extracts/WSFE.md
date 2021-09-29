# Country-level extracts: World Settlement Footprint - Evolution (WSFE)

Javascript script to export monthly NDVI statistics by feature in a featureCollection.

Dataset used: [https://developers.google.com/earth-engine/datasets/catalog/NOAA_CDR_AVHRR_NDVI_V5](https://developers.google.com/earth-engine/datasets/catalog/NOAA_CDR_AVHRR_NDVI_V5)  
Timeframe: **1984 - 2019**  
Precision: **30m**  

## Prerequisite

- Reference polygon layer (e.g. administrative divisions or vector grid). To add as an import named **table**. 

## Script

```javascript
var wsfe_afr = ee.ImageCollection("users/mattia80/Africa/WSFevolution"),
    table = ee.FeatureCollection("users/jbelanger/CWA/vector/CMR_ADM0_OCHA");
    
// create feature collection to calculate zonal stats 
Map.setOptions('SATELLITE');  
var shown = true;
var opacity = 0.5;
var table = ee.FeatureCollection(table);
Map.addLayer(table, {color:'0000FF'}, "table", shown, opacity);
Map.centerObject(table, 7);

// get bbox from table bounds
var bbox = table.geometry().bounds();

// set map viz parameters
var palettes = require('users/gena/packages:palettes');
var palette = palettes.colorbrewer.RdYlGn[9];

var visParams = {
  min: 1984,
  max: 2019,
  palette: palette,
};

var drive_folder = 'CMR_Urban'
var drive_export = 'WSFE_CMR'

//-----------------[ WORLD SETTLEMENT FOOTPRINT EVOLUTION ]-----------------------//
print('//-----------------[ WORLD SETTLEMENT FOOTPRINT EVOLUTION ]-----------------------//');


//----------------- 1. filter image collection -----------------------//
print('//----------------- 1. filter image collection -----------------------//');

// first, let's filter our image collection to the bounds of your AOI
var filter = wsfe_afr.filterBounds(bbox);
Map.addLayer(filter, visParams, "wsfe_afr")
print(filter);

//----------------- 2. mosiac all images in collection -----------------------// 
print('//----------------- 2. mosaic all images in collection -----------------------//');

// mosaic
var mosaic = ee.ImageCollection(filter).mosaic();
Map.addLayer(mosaic, visParams, 'wsf mosaic');

//----------------- 3. clip to AOI -----------------------// 
print('//----------------- 3. clip to AOI -----------------------//');

// clip
var clip = mosaic.clip(table);
print("clip", clip);
Map.addLayer(clip, visParams, "wsf clipped mosaic");

//----------------- 4. export to asset -----------------------//
print('//----------------- 4. export to asset -----------------------//');


Export.image.toDrive({
  image: clip,
  folder: drive_folder,
  description: drive_export,
  scale: 10,
  region: bbox,
  maxPixels: 10895803460 // set to 10 more than needed 
});

/*

Export.image.toAsset({
  image: clip,
  description: drive_export,
  assetId: drive_export,
  scale: 10,
  region: bbox,
  maxPixels: 10895803460,
  pyramidingPolicy: {'year': 'sample'}
});
*/

//----------------- 5. visualize cmr asset -----------------------//
print('//----------------- 5. visualize cmr asset -----------------------//');

//Map.addLayer(image, visParams, "outputted CMR asset!")

```
## Output

The scripts extracts an image (raster .tif) to Google Drive. Raster values are the same as the original dataset, where each pixel that is built-up contains a value from 
1984 to 2019 to indicate the year a dataste became built-up. 
