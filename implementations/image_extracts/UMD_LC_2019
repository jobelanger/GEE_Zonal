# Country-level extracts: UMD Landcover Data - 2019

Javascript script to export a raster to an area of interest. In this case, country boundaries

Dataset used: UMD Landcover 2019 (asset ID: ee.Image('projects/glad/GLCstrata2019'))
Timeframe: **2019**  
Precision: **30m**  

GEE Code Editor link - https://code.earthengine.google.com/?scriptPath=users%2Fjbelanger%2Fcmr%3Aenviro%2Fumd_lc_30m_extract_zonal
## Prerequisite

- Reference polygon layer (e.g. administrative divisions or vector grid). To add as an import named **table**. 

## Script

```javascript

Map.setOptions('SATELLITE');    
var shown = true;
var opacity = 0.5;
Map.addLayer(table, {color:'0000FF'}, "table", shown, opacity);
Map.centerObject(table, 8);

// get bbox from table bounds
var bbox = table.geometry().bounds();

// set export names 
var drive_folder = 'CMR_Enviro'
var asset_folder = 'users/jbelanger/CWA/raster'
var drive_export_map = 'UMD_LC_2019_30m_BEN_map'
var drive_export_strata = 'UMD_LC_2019_30m_BEN_strata'

//-----------------[ UMD Landcover Dataset @ 30m ]-----------------------//
print('//-----------------[ UMD Landcover Dataset @ 30m ]-----------------------//');

print("This data is made available by the Global Land Analysis and Discovery (GLAD) lab at the University of Maryland.",
"M.C. Hansen, P.V. Potapov, A.H. Pickens, A. Tyukavina, A. Hernandez-Serna, V. Zalles, S. Turubanova, I. Kommareddy, S.V. Stehman (2021).");

print("For more information please visit:",ui.Label(" https://glad.umd.edu/dataset/global-land-cover-land-use-v1",{},
"https://glad.umd.edu/dataset/global-land-cover-land-use-v1"),
  "The legend is available here:",ui.Label(" https://storage.googleapis.com/earthenginepartners-hansen/GLCLU_2019/legend.xlsx",{},
  "https://storage.googleapis.com/earthenginepartners-hansen/GLCLU_2019/legend.xlsx"));

var map = ee.ImageCollection('projects/glad/GLCmap2019').mosaic();
var strata = ee.ImageCollection('users/ahudson2/glcstrata16').mosaic();//ee.Image('projects/glad/GLCstrata2019');

var visParamStrata = {min:0,max:20,palette:['000000','ffffdd','aaaa00','ffff00',
  '008800', '005500','ff00ff', 'ff0000','dddddd', '999999','6699dd', '009999',
  '55bb77', 'bb4477','bb0000', 'ffffff','0000aa', 'ffaa00','00ffff', '111111',
  '000000']};
   
var visParamMap = {min:0,max:255,palette:['FEFECC','FDFDC8','FCFCC4','FAFAC0','F9F9BC','F7F7B8','F6F6B4','F4F4B0','F2F2AC','F1F1A8','EFEFA4',
'EEEEA1','ECEC9D','EBEB99','E9E995','E8E891','E6E68D','E5E589','E3E385','E2E281','E0E07D','DFDF7A','DDDD76','DBDB72','DADA6E','D8D86A','D5D564',
'D4D460','D2D25C','D1D158','CFCF54','CDCD50','CCCC4D','CACA49','C9C945','C7C741','C6C63D','C4C439','C3C335','C1C131','C0C02D','BEBE29','BDBD26',
'BBBB22','BABA1E','B8B81A','B6B616','B5B512','B3B30E','B2B20A','B0B006','609C60','5E9A5E','5C985C','5A965A','589458','569256','549054','528E52',
'508C50','4E8A4E','4C884C','4A864A','488448','468246','448044','427E42','407C40','3E7A3E','3C783C','3A763A','387438','367236','347034','326E32',
'316D31','2F6C2F','2C6A2C','296829','276627','246524','216321','1E611E','1B5E1B','175B17','145A14','115811','0E560E','0B540B','095309','065106',
'033303','FFABFF','FFA5FF','FF9EFF','FF98FF','FF91FF','FF8AFF','FF83FF','FF7DFF','FF76FF','FF6FFF','FF68FF','FF62FF','FF5AFF','FF53FF','FF4CFF',
'FF45FF','FF3EFF','FF38FF','FF31FF','FF2AFF','FF23FF','FF1CFF','FF16FF','FF0FFF','FF0000','000000','000000','000000','BFC0C0','BCBFC0','B8BEC2',
'B4BCC3','B1BBC4','ADBAC5','A9B9C6','A6B8C7','A2B7C9','9EB6CA','9AB5CB','97B4CC','93B2CD','90B2CE','8DB0CF','89AFD0','85AED1','82ADD3','7EACD4',
'7AABD5','77AAD6','73A9D7','70A8D8','6CA7D9','68A5DA','64A3DC','60A2DD','5CA0DE','589FDF','559EE0','519DE1','4E9CE2','4A9BE3','469AE4','4399E6',
'3F98E7','3B97E8','3895E9','3494EA','3193EB','2E92EC','2A91ED','2690EE','238FF0','1F8EF1','1C8DF2','188CF3','148BF4','118AF5','0D88F6','0986F7',
'9DC7C7','99C5C5','95C3C3','90C1C1','8CBFBF','87BDBD','83BBBB','7FB9B9','7AB7B7','76B5B5','72B3B3','6DB1B1','67AEAE','62ABAB','5EA9A9','5AA7A7',
'55A5A5','51A3A3','4DA1A1','489F9F','449D9D','3F9B9B','3A9999','369797','327C7C','327A7B','31787A','317678','307477','2F7275','2F7074','2F6E73',
'2C6A6F','2B696E','2B676D','2B656C','2A636A','296169','295F67','285D66','275B65','8C7BF0','8B77EF','8B74EF','8A71EE','8A6DEE','896AED','8967ED',
'8864EC','8861EC','875DEB','875AEB','8657EA','8351E7','834EE7','824BE6','8248E6','8144E5','8141E5','803EE4','803BE4','7F38E3','7F34E3','7E31E2',
'7D2EE1','C80000','000000','000000','000000','00F4F4','00E8E8','00DDDD','00D0D0','00C5C5','00B7B7','00ACAC','009F9F','009494','008888','00007D',
'FFFFFF','FF7D00','000000','000032','010101']};
  
//print('visualizing global mosaic')
//Map.addLayer(strata,visParamStrata,'Global Land Strata');
//Map.addLayer(map,visParamMap,'Global land cover and land use');


//----------------- clip to AOI -----------------------// 
print('//----------------- clip to AOI -----------------------//');

// clip Map mosaic
var clip_map = map.clip(bbox); 
print("clip_map", clip_map);
Map.addLayer(clip_map, visParamMap, "GLC clipped mosaic (map)");

// clip Strata mosaic
var clip_strata = strata.clip(bbox); 
print("clip_strata", clip_strata);
Map.addLayer(clip_strata, visParamStrata, "GLC clipped mosaic (strata)");

// raster to vector? should we convert this by classification and then export? 


//----------------- export -----------------//
print('//----------------- export -----------------------//');

Export.image.toDrive({
  image: clip_map,
  description: drive_export_map,
  scale: 30,
  region: bbox,
  maxPixels: 1210692142
});

Export.image.toDrive({
  image: clip_strata,
  description: drive_export_strata,
  scale: 30,
  region: bbox,
  maxPixels: 1210692142
});

/*
Export.image.toAsset({
  image: clip_map,
  description: drive_export_map,
  assetId: asset_folder + drive_export_map,
  scale: 30,
  region: bbox,
  maxPixels: 128704274, // set this value to be 10 pixels larger than the total
  pyramidingPolicy: {'b1': 'sample'}
});


Export.image.toAsset({
  image: clip_strata,
  description: drive_export_strata,
  assetId: asset_folder + drive_export_strata,
  scale: 30,
  region: bbox,
  maxPixels: 128704274,
  pyramidingPolicy: {'b1': 'sample'}
});


//----------------- 5. visualize cmr asset -----------------------//
print('//----------------- 5. visualize cmr asset -----------------------//');

//Map.addLayer(image, visParamMap, "outputted asset!")
*/

```
## Output

The scripts extracts an image (raster .tif) to Google Drive. Raster values are the same as the original dataset, where each pixel that is built-up contains a value from 
1984 to 2019 to indicate the year a dataste became built-up. 
