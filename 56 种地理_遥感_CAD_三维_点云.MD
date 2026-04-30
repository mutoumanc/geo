下面给出你列出的 **56 种地理/遥感/CAD/三维/点云格式**的样例数据来源、公开链接、以及可创建或转换文件的脚本/命令建议。

> 说明：
>
> 1. 你给出的部分格式存在拼写或归属不明确，例如 `adf.pg`、`tiftbil`、`0sgosgb`、`wp. wt. w`。我按常见 GIS/CAD/三维数据格式进行了拆分和解释。
> 2. 一些格式是 **商业软件专有格式**，例如 `DWG`、`MDB/ACCDB`、`ECW`、`SID`、`POD`、`3MX` 等，开源工具通常只能读取或有限写入，完整创建一般需要 ArcGIS、AutoCAD、Bentley、SuperMap、MapGIS 等软件。
> 3. 表中“创建/转换方式”给出的是可操作方向，很多格式可以通过 **GDAL/OGR、Python、QGIS、ArcGIS、FME、AutoCAD、CloudCompare、PDAL、Cesium 工具链**生成。

---

# 一、通用公开数据源

这些数据源可作为后续格式转换的原始数据。

| 数据源                          | 内容                           | 链接                                               |
| ------------------------------- | ------------------------------ | -------------------------------------------------- |
| Natural Earth                   | 国家边界、城市、河流、栅格底图 | https://www.naturalearthdata.com/downloads/        |
| OpenStreetMap Geofabrik         | 道路、建筑、POI、水系          | https://download.geofabrik.de/                     |
| GADM                            | 全球行政区划                   | https://gadm.org/                                  |
| USGS EarthExplorer              | Landsat、DEM、遥感影像         | https://earthexplorer.usgs.gov/                    |
| Copernicus Data Space           | Sentinel 遥感影像              | https://dataspace.copernicus.eu/                   |
| NASA Earthdata                  | MODIS、HDF、NetCDF 等          | https://earthdata.nasa.gov/                        |
| OpenTopography                  | DEM、LAS/LAZ 点云              | https://opentopography.org/                        |
| NOAA Digital Coast              | 海岸点云、DEM、影像            | https://coast.noaa.gov/dataviewer/                 |
| Cesium 3D Tiles Samples         | B3DM、PNTS、3D Tiles           | https://github.com/CesiumGS/3d-tiles-samples       |
| Stanford 3D Scanning Repository | PLY、OBJ 三维模型              | https://graphics.stanford.edu/data/3Dscanrep/      |
| QGIS Sample Data                | GIS 教学样例                   | https://qgis.org/downloads/data/                   |
| GDAL 测试数据                   | 多种矢量/栅格样例              | https://github.com/OSGeo/gdal/tree/master/autotest |

---

# 二、56 种格式对应的数据源与创建方式

## 1. 矢量、GIS、CAD、空间数据库类

| 序号 | 格式           | 类型                                 | 样例数据源/链接                                                                         | 创建或转换方式                                            |
| ---: | -------------- | ------------------------------------ | --------------------------------------------------------------------------------------- | --------------------------------------------------------- |
|    1 | `shp`        | Esri Shapefile                       | Natural Earth Shapefile：https://www.naturalearthdata.com/downloads/                    | `ogr2ogr -f "ESRI Shapefile" out.shp in.geojson`        |
|    2 | `mdb`        | Esri Personal Geodatabase / Access   | ArcGIS 示例数据、旧版 Esri 数据                                                         | 通常需 ArcGIS；开源读取可用 GDAL/OGR MDB 驱动，写入有限   |
|    3 | `gdb`        | Esri File Geodatabase                | Esri Open Data、ArcGIS Hub：https://hub.arcgis.com/                                     | `ogr2ogr -f OpenFileGDB out.gdb in.shp`，或 ArcGIS 创建 |
|    4 | `dwg`        | AutoCAD Drawing                      | Autodesk 示例：https://www.autodesk.com/                                                | 通常需 AutoCAD、ODA File Converter；开源写 DWG 支持有限   |
|    5 | `dxf`        | AutoCAD Exchange Format              | QGIS/GDAL 样例、CAD 公共样例                                                            | Python `ezdxf` 或 `ogr2ogr -f DXF out.dxf in.shp`     |
|    6 | `edb`        | 专有工程/空间数据库，含义依软件不同  | 需确认具体厂商，如 Bentley、SuperMap、MapGIS 等                                         | 多为专有格式，建议用原软件导出为 SHP/GDB/DXF              |
|    7 | `accdb`      | Microsoft Access 新版数据库          | Microsoft Access 示例库                                                                 | Python `pyodbc`、Access、ArcGIS；空间字段通常需自定义   |
|    8 | `e00`        | ArcInfo Interchange                  | GDAL Arc/Info E00 测试数据：https://github.com/OSGeo/gdal/tree/master/autotest/ogr/data | `ogr2ogr out.shp in.e00`；创建通常需 ArcInfo/ArcGIS     |
|    9 | `vot`        | 可能为专有矢量/行业格式              | 需确认厂商                                                                              | 建议从原系统导出 SHP/DXF/GDB；通用开源支持不稳定          |
|   10 | `udd`        | 常见于 SuperMap 旧数据源相关文件     | SuperMap 示例数据：https://www.supermap.com/                                            | 用 SuperMap iDesktop 创建或转换为 UDB/UDBX/SHP            |
|   11 | `udbx`       | SuperMap 数据源                      | SuperMap 示例数据                                                                       | SuperMap iDesktop/iObjects 创建；可导入 SHP、GDB、CAD     |
|   12 | `sdd`        | 可能为 SuperMap/行业空间数据文件     | SuperMap 或厂商样例                                                                     | 需用对应软件导出 SHP/GDB/GeoJSON                          |
|   13 | `wp`         | 常见于 MapGIS 区文件                 | MapGIS 示例数据                                                                         | MapGIS 创建；可转 SHP、DXF                                |
|   14 | `wt`         | 常见于 MapGIS 点文件                 | MapGIS 示例数据                                                                         | MapGIS 创建；可转 SHP、DXF                                |
|   15 | `w` / `wl` | 常见于 MapGIS 线文件，通常为 `.wl` | MapGIS 示例数据                                                                         | MapGIS 创建；建议确认是否为 `.wl`                       |
|   16 | `dgn`        | MicroStation Design                  | Bentley 示例数据：https://www.bentley.com/                                              | `ogr2ogr -f DGN out.dgn in.shp`，或 MicroStation 创建   |
|   17 | `tab`        | MapInfo TAB                          | GDAL/MapInfo 样例                                                                       | `ogr2ogr -f "MapInfo File" out.tab in.shp`              |
|   18 | `mif`        | MapInfo Interchange Format           | GDAL/MapInfo 样例                                                                       | `ogr2ogr -f "MapInfo File" out.mif in.shp`              |
|   19 | `kml`        | Google Earth KML                     | Google Earth 示例：https://developers.google.com/kml                                    | `ogr2ogr -f KML out.kml in.shp`                         |
|   20 | `kmz`        | 压缩 KML                             | Google Earth 示例                                                                       | 先生成 KML，再 zip 为 KMZ                                 |

---

## 2. Esri 栅格、Coverage、影像、遥感格式

| 序号 | 格式               | 类型                                         | 样例数据源/链接                                                 | 创建或转换方式                                                |
| ---: | ------------------ | -------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------- |
|   21 | `adf`            | Esri ArcInfo Binary Grid / Coverage 组成文件 | ArcGIS 示例数据、GDAL 测试数据                                  | ArcGIS 创建；GDAL 可读：`gdalinfo hdr.adf`                  |
|   22 | `pg`             | 可能为 ArcInfo Coverage/MapGIS/行业文件      | 需确认具体软件                                                  | 建议用原软件转 SHP/GDB                                        |
|   23 | `jpeg` / `jpg` | 普通影像                                     | Natural Earth Raster、OpenAerialMap：https://openaerialmap.org/ | Python Pillow：`Image.save("out.jpg")`                      |
|   24 | `bmp`            | 位图影像                                     | 任意遥感/图片转出                                               | Python Pillow：`Image.save("out.bmp")`                      |
|   25 | `png`            | 无损影像/瓦片                                | OSM/XYZ Tiles、Natural Earth                                    | Python Pillow：`Image.save("out.png")`                      |
|   26 | `tif`            | TIFF / GeoTIFF                               | USGS、Sentinel、WorldClim                                       | `gdal_translate in.jp2 out.tif` 或 Rasterio 创建            |
|   27 | `tiff`           | TIFF / GeoTIFF                               | 同上                                                            | 同 `tif`                                                    |
|   28 | `bil`            | Band Interleaved by Line                     | USGS DEM、遥感数据                                              | `gdal_translate -of EHdr in.tif out.bil`                    |
|   29 | `bip`            | Band Interleaved by Pixel                    | 遥感多波段数据                                                  | `gdal_translate -of ENVI -co INTERLEAVE=BIP in.tif out.bip` |
|   30 | `bsq`            | Band Sequential                              | 遥感多波段数据                                                  | `gdal_translate -of ENVI -co INTERLEAVE=BSQ in.tif out.bsq` |
|   31 | `iq`             | 可能为 ERDAS/遥感专有索引文件                | 需确认软件环境                                                  | 建议用 ERDAS、ENVI 或原系统导出 GeoTIFF                       |
|   32 | `ecw`            | Enhanced Compression Wavelet                 | 大幅遥感影像，示例需 Hexagon/ER Mapper SDK                      | GDAL 需 ECW SDK；创建常需商业授权                             |
|   33 | `sid`            | MrSID                                        | LizardTech/Extensis 示例：https://www.extensis.com/             | 读取需 SDK；创建通常需商业软件                                |
|   34 | `hdf`            | HDF4/HDF5 遥感科学数据                       | NASA MODIS：https://earthdata.nasa.gov/                         | Python `h5py` 或 NASA 数据下载                              |
|   35 | `dem`            | USGS DEM / 通用高程                          | USGS、OpenTopography                                            | `gdal_translate -of USGSDEM in.tif out.dem`                 |
|   36 | `img`            | ERDAS Imagine IMG                            | USGS、ERDAS 示例                                                | `gdal_translate -of HFA in.tif out.img`                     |
|   37 | `grd`            | Surfer/GMT/Grid                              | GMT 示例：https://www.generic-mapping-tools.org/                | `gdal_translate -of GSAG in.tif out.grd` 或 GMT 创建        |
|   38 | `dsm`            | Digital Surface Model，通常不是固定扩展名    | OpenTopography、无人机测绘 DSM                                  | 通常以 GeoTIFF 保存，如 `dsm.tif`                           |
|   39 | `asc`            | ASCII Grid                                   | DEM 栅格样例                                                    | `gdal_translate -of AAIGrid in.tif out.asc`                 |

---

## 3. 三维模型、点云、三维瓦片格式

| 序号 | 格式     | 类型                            | 样例数据源/链接                                                            | 创建或转换方式                              |
| ---: | -------- | ------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------- |
|   40 | `obj`  | Wavefront 3D Model              | Stanford 3D：https://graphics.stanford.edu/data/3Dscanrep/                 | Blender、MeshLab、CloudCompare 导出         |
|   41 | `las`  | LiDAR 点云                      | OpenTopography、USGS 3DEP                                                  | Python `laspy`、PDAL 创建                 |
|   42 | `ply`  | Polygon File Format             | Stanford 3D、ModelNet                                                      | MeshLab、CloudCompare、Open3D 创建          |
|   43 | `pod`  | Bentley Pointools POD 点云      | Bentley 示例                                                               | Bentley Pointools / ContextCapture 创建     |
|   44 | `osgb` | OpenSceneGraph Binary           | 倾斜摄影、OSG 示例                                                         | ContextCapture、OpenSceneGraph 工具         |
|   45 | `osg`  | OpenSceneGraph ASCII            | OpenSceneGraph 示例：https://github.com/openscenegraph/OpenSceneGraph-Data | OSG 工具或 Blender 插件                     |
|   46 | `osgt` | OpenSceneGraph Text             | OpenSceneGraph 示例                                                        | OSG 工具导出                                |
|   47 | `3mx`  | Bentley ContextCapture 三维模型 | Bentley ContextCapture 示例                                                | ContextCapture 生成                         |
|   48 | `b3dm` | 3D Tiles Batched Model          | Cesium Samples：https://github.com/CesiumGS/3d-tiles-samples               | `3d-tiles-tools`、Cesium ion、obj23dtiles |
|   49 | `pts`  | ASCII 点云                      | CloudCompare 示例、扫描仪导出                                              | 文本文件写入 x y z intensity                |
|   50 | `off`  | Object File Format              | Princeton ModelNet：http://modelnet.cs.princeton.edu/                      | MeshLab/Open3D 导出                         |
|   51 | `xyz`  | ASCII 点云坐标                  | OpenTopography、CloudCompare                                               | 纯文本写入 x y z                            |
|   52 | `stl`  | 3D 打印/网格                    | Thingiverse、Thingi10K：http://ten-thousand-models.appspot.com/            | Blender、trimesh 创建                       |
|   53 | `laz`  | 压缩 LAS 点云                   | OpenTopography、USGS 3DEP                                                  | `laszip` 或 PDAL：LAS 转 LAZ              |
|   54 | `neu`  | 可能为三维/工程/扫描专有格式    | 需确认软件来源                                                             | 建议用原软件导出 OBJ/STL/PLY/LAS            |
|   55 | `stp`  | STEP CAD                        | NIST STEP 示例：https://www.nist.gov/                                      | FreeCAD、OpenCascade 创建                   |
|   56 | `step` | STEP CAD，同 STP                | NIST STEP 示例                                                             | FreeCAD 导出 `.step`                      |

---

# 三、常用基础数据下载命令

## 1. 下载 Natural Earth 国家边界 SHP

```bash
wget https://naturalearth.s3.amazonaws.com/110m_cultural/ne_110m_admin_0_countries.zip
unzip ne_110m_admin_0_countries.zip -d natural_earth_countries
```

得到：

```text
ne_110m_admin_0_countries.shp
ne_110m_admin_0_countries.shx
ne_110m_admin_0_countries.dbf
ne_110m_admin_0_countries.prj
```

---

## 2. 下载 OSM PBF

```bash
wget https://download.geofabrik.de/asia/china-latest.osm.pbf
```

也可以下载某个国家或地区：

```text
https://download.geofabrik.de/
```

---

## 3. 下载 Cesium 3D Tiles 样例

```bash
git clone https://github.com/CesiumGS/3d-tiles-samples.git
```

其中包含：

```text
.b3dm
.pnts
.i3dm
.cmpt
tileset.json
```

---

## 4. 下载 OpenSceneGraph 样例

```bash
git clone https://github.com/openscenegraph/OpenSceneGraph-Data.git
```

可获得：

```text
.osg
.osgt
.osgb
.obj
.ive
```

---

# 四、常见格式创建与转换命令

下面假设你已经有一个基础矢量文件：

```text
input.shp
```

或基础栅格文件：

```text
input.tif
```

---

## 1. SHP 转 GeoDatabase / MapInfo / KML / DXF / DGN

```bash
# SHP 转 FileGDB
ogr2ogr -f OpenFileGDB output.gdb input.shp

# SHP 转 MapInfo TAB
ogr2ogr -f "MapInfo File" output.tab input.shp

# SHP 转 MIF
ogr2ogr -f "MapInfo File" output.mif input.shp

# SHP 转 KML
ogr2ogr -f KML output.kml input.shp

# SHP 转 DXF
ogr2ogr -f DXF output.dxf input.shp

# SHP 转 DGN
ogr2ogr -f DGN output.dgn input.shp
```

---

## 2. KML 压缩为 KMZ

```bash
zip output.kmz output.kml
```

或者：

```bash
mkdir kmz_tmp
cp output.kml kmz_tmp/doc.kml
cd kmz_tmp
zip -r ../output.kmz .
cd ..
```

---

## 3. GeoTIFF 转 JPEG、PNG、BMP

```bash
gdal_translate -of JPEG input.tif output.jpg
gdal_translate -of PNG input.tif output.png
gdal_translate -of BMP input.tif output.bmp
```

---

## 4. GeoTIFF 转 BIL、BIP、BSQ

```bash
# BIL
gdal_translate -of EHdr input.tif output.bil

# BIP
gdal_translate -of ENVI -co INTERLEAVE=BIP input.tif output_bip.dat

# BSQ
gdal_translate -of ENVI -co INTERLEAVE=BSQ input.tif output_bsq.dat
```

ENVI 格式通常会同时生成：

```text
output_bip.dat
output_bip.hdr
```

或：

```text
output_bsq.dat
output_bsq.hdr
```

---

## 5. GeoTIFF 转 IMG、ASC、DEM、GRD

```bash
# ERDAS IMG
gdal_translate -of HFA input.tif output.img

# ASCII Grid
gdal_translate -of AAIGrid input.tif output.asc

# USGS DEM
gdal_translate -of USGSDEM input.tif output.dem

# Surfer ASCII Grid
gdal_translate -of GSAG input.tif output.grd
```

---

## 6. LAS 与 LAZ 互转

使用 PDAL：

```bash
pdal translate input.las output.laz
pdal translate input.laz output.las
```

使用 laszip：

```bash
laszip -i input.las -o output.laz
laszip -i input.laz -o output.las
```

---

# 五、Python 创建部分常见格式样例

下面脚本可以创建：

```text
shp
kml
tif
jpg
png
bmp
asc
hdf
las
ply
obj
pts
xyz
stl
```

安装依赖：

```bash
pip install geopandas shapely rasterio numpy pandas pillow h5py laspy trimesh open3d
```

保存为：

```bash
create_common_samples.py
```

```python
import os
import numpy as np
import pandas as pd
import geopandas as gpd
from shapely.geometry import Point
from PIL import Image
import rasterio
from rasterio.transform import from_origin
import h5py
import laspy
import trimesh

OUT = "format_samples"
os.makedirs(OUT, exist_ok=True)

# -----------------------------
# 1. 创建 SHP
# -----------------------------
df = pd.DataFrame({
    "id": [1, 2, 3],
    "name": ["A点", "B点", "C点"],
    "lon": [116.40, 116.41, 116.42],
    "lat": [39.90, 39.91, 39.92]
})

gdf = gpd.GeoDataFrame(
    df,
    geometry=gpd.points_from_xy(df["lon"], df["lat"]),
    crs="EPSG:4326"
)

gdf.to_file(os.path.join(OUT, "sample.shp"), encoding="utf-8")

# -----------------------------
# 2. 创建 KML
# -----------------------------
kml = """<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2">
<Document>
"""

for _, row in df.iterrows():
    kml += f"""
<Placemark>
  <name>{row['name']}</name>
  <Point>
    <coordinates>{row['lon']},{row['lat']},0</coordinates>
  </Point>
</Placemark>
"""

kml += """
</Document>
</kml>
"""

with open(os.path.join(OUT, "sample.kml"), "w", encoding="utf-8") as f:
    f.write(kml)

# -----------------------------
# 3. 创建 GeoTIFF / TIF
# -----------------------------
arr = np.arange(100, dtype=np.float32).reshape(10, 10)

transform = from_origin(116.0, 40.0, 0.01, 0.01)

with rasterio.open(
    os.path.join(OUT, "sample.tif"),
    "w",
    driver="GTiff",
    height=arr.shape[0],
    width=arr.shape[1],
    count=1,
    dtype=arr.dtype,
    crs="EPSG:4326",
    transform=transform
) as dst:
    dst.write(arr, 1)

# -----------------------------
# 4. 创建 JPG / PNG / BMP
# -----------------------------
img_arr = np.uint8((arr / arr.max()) * 255)
img = Image.fromarray(img_arr)

img.save(os.path.join(OUT, "sample.jpg"))
img.save(os.path.join(OUT, "sample.png"))
img.save(os.path.join(OUT, "sample.bmp"))

# -----------------------------
# 5. 创建 ASC
# -----------------------------
with open(os.path.join(OUT, "sample.asc"), "w", encoding="utf-8") as f:
    f.write("ncols         10\n")
    f.write("nrows         10\n")
    f.write("xllcorner     116.0\n")
    f.write("yllcorner     39.9\n")
    f.write("cellsize      0.01\n")
    f.write("NODATA_value  -9999\n")
    for row in arr:
        f.write(" ".join(str(float(v)) for v in row) + "\n")

# -----------------------------
# 6. 创建 HDF5
# -----------------------------
with h5py.File(os.path.join(OUT, "sample.hdf"), "w") as h:
    h.create_dataset("dem", data=arr)
    h.attrs["crs"] = "EPSG:4326"

# -----------------------------
# 7. 创建 LAS
# -----------------------------
header = laspy.LasHeader(point_format=3, version="1.2")
header.offsets = np.array([116.0, 39.0, 0.0])
header.scales = np.array([0.000001, 0.000001, 0.01])

las = laspy.LasData(header)

n = 100
las.x = 116.3 + np.random.random(n) * 0.01
las.y = 39.9 + np.random.random(n) * 0.01
las.z = 50 + np.random.random(n) * 20
las.intensity = np.random.randint(0, 255, n)

las.write(os.path.join(OUT, "sample.las"))

# -----------------------------
# 8. 创建 PTS / XYZ
# -----------------------------
points = np.column_stack([
    116.3 + np.random.random(n) * 0.01,
    39.9 + np.random.random(n) * 0.01,
    50 + np.random.random(n) * 20
])

np.savetxt(os.path.join(OUT, "sample.xyz"), points, fmt="%.8f")
np.savetxt(os.path.join(OUT, "sample.pts"), points, fmt="%.8f")

# -----------------------------
# 9. 创建 OBJ / STL / PLY
# -----------------------------
mesh = trimesh.creation.box(extents=(1, 1, 1))

mesh.export(os.path.join(OUT, "sample.obj"))
mesh.export(os.path.join(OUT, "sample.stl"))
mesh.export(os.path.join(OUT, "sample.ply"))

print("样例文件已生成：", OUT)
```

运行：

```bash
python create_common_samples.py
```

---

# 六、DXF 创建脚本

安装：

```bash
pip install ezdxf
```

创建 `sample.dxf`：

```python
import ezdxf

doc = ezdxf.new()
msp = doc.modelspace()

msp.add_point((0, 0))
msp.add_line((0, 0), (10, 10))
msp.add_lwpolyline([(0, 0), (10, 0), (10, 10), (0, 10), (0, 0)])

doc.saveas("sample.dxf")
```

---

# 七、Access MDB / ACCDB 创建思路

`mdb` 和 `accdb` 本质是 Microsoft Access 数据库，不是天然空间格式。GIS 中常见的是 Esri Personal Geodatabase，通常后缀为 `.mdb`。

## 1. Windows 下用 Python 创建 ACCDB/MDB 表

需要安装：

- Microsoft Access Database Engine
- `pyodbc`

```bash
pip install pyodbc
```

示例连接已有 ACCDB：

```python
import pyodbc

conn_str = (
    r"DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};"
    r"DBQ=C:\data\sample.accdb;"
)

conn = pyodbc.connect(conn_str)
cur = conn.cursor()

cur.execute("""
CREATE TABLE poi (
    id INTEGER,
    name TEXT,
    lon DOUBLE,
    lat DOUBLE
)
""")

cur.execute("""
INSERT INTO poi(id, name, lon, lat)
VALUES (1, 'A点', 116.4, 39.9)
""")

conn.commit()
conn.close()
```

> 注意：Esri Personal Geodatabase 的空间字段结构复杂，建议用 ArcGIS 创建。

---

# 八、FileGDB 创建方式

如果安装了较新版本 GDAL，可尝试：

```bash
ogr2ogr -f OpenFileGDB sample.gdb input.shp
```

更稳妥方式是使用 ArcGIS Pro：

```python
import arcpy

arcpy.management.CreateFileGDB(
    out_folder_path=r"C:\data",
    out_name="sample.gdb"
)
```

导入 Shapefile：

```python
import arcpy

arcpy.conversion.FeatureClassToGeodatabase(
    Input_Features=[r"C:\data\sample.shp"],
    Output_Geodatabase=r"C:\data\sample.gdb"
)
```

---

# 九、B3DM / 3D Tiles 创建方式

推荐工具：

- Cesium ion：https://cesium.com/platform/cesium-ion/
- 3d-tiles-tools：https://github.com/CesiumGS/3d-tiles-tools
- obj23dtiles：https://github.com/PrincessGod/objTo3d-tiles

## OBJ 转 3D Tiles 示例

```bash
npm install -g obj23dtiles
```

```bash
obj23dtiles \
  -i sample.obj \
  -o tileset \
  --longitude 116.4 \
  --latitude 39.9
```

输出通常包括：

```text
tileset/
├── tileset.json
└── *.b3dm
```

---

# 十、OSGB / OSG / OSGT 创建方式

安装 OpenSceneGraph 后，可用 `osgconv` 转换：

```bash
osgconv sample.obj sample.osg
osgconv sample.obj sample.osgt
osgconv sample.obj sample.osgb
```

也可从 OSG 官方样例下载：

```bash
git clone https://github.com/openscenegraph/OpenSceneGraph-Data.git
```

---

# 十一、PLY / OFF / STL / OBJ 互转

使用 MeshLab 或 CloudCompare 最方便。

也可用 Python `trimesh`：

```python
import trimesh

mesh = trimesh.load("sample.obj")

mesh.export("sample.ply")
mesh.export("sample.off")
mesh.export("sample.stl")
```

---

# 十二、STEP / STP 创建方式

推荐使用 FreeCAD。

## FreeCAD Python 示例

```python
import FreeCAD
import Part

doc = FreeCAD.newDocument()

box = Part.makeBox(10, 10, 10)
Part.show(box)

doc.saveAs("sample.FCStd")

Part.export([doc.Objects[0]], "sample.step")
Part.export([doc.Objects[0]], "sample.stp")
```

也可以从 NIST STEP 示例获取：

```text
https://www.nist.gov/services-resources/software/step-file-analyzer-and-viewer
```

---

# 十三、POD / 3MX / ECW / SID 等商业格式建议

以下格式通常不建议作为通用交换格式，因为依赖商业软件或 SDK：

| 格式             | 推荐软件                          | 建议交换格式         |
| ---------------- | --------------------------------- | -------------------- |
| `pod`          | Bentley Pointools、ContextCapture | LAS、LAZ、E57、XYZ   |
| `3mx`          | Bentley ContextCapture            | OBJ、OSGB、3D Tiles  |
| `ecw`          | ER Mapper、Hexagon SDK            | GeoTIFF、JPEG2000    |
| `sid`          | MrSID SDK                         | GeoTIFF、JPEG2000    |
| `dwg`          | AutoCAD、ODA Converter            | DXF、SHP、GDB        |
| `mdb`          | ArcGIS Desktop                    | GDB、SHP、GeoPackage |
| `edb`          | 具体厂商软件                      | SHP、GDB、GeoJSON    |
| `vot`          | 具体厂商软件                      | SHP、DXF、GeoJSON    |
| `udd/sdd/udbx` | SuperMap                          | SHP、GeoJSON、GDB    |
| `wp/wt/wl`     | MapGIS                            | SHP、DXF、GDB        |

---

# 十四、推荐的统一格式转换工具

| 工具              | 适合格式                                                  |
| ----------------- | --------------------------------------------------------- |
| GDAL/OGR          | SHP、GDB、TAB、MIF、KML、DXF、DGN、GeoTIFF、IMG、ASC、DEM |
| QGIS              | 大部分 GIS 矢量和栅格                                     |
| ArcGIS Pro        | SHP、GDB、MDB、E00、ADF、DEM                              |
| FME               | 大量商业/专有格式转换                                     |
| AutoCAD / ODA     | DWG、DXF                                                  |
| SuperMap iDesktop | UDBX、UDD、SDD                                            |
| MapGIS            | WP、WT、WL                                                |
| CloudCompare      | LAS、LAZ、PLY、OBJ、PTS、XYZ、E57                         |
| MeshLab           | OBJ、PLY、STL、OFF                                        |
| Blender           | OBJ、STL、PLY、glTF                                       |
| PDAL              | LAS、LAZ、XYZ、PTS                                        |
| Cesium ion        | B3DM、3D Tiles                                            |
| FreeCAD           | STEP、STP、STL、OBJ                                       |

---

# 十五、最实用的数据获取与生成路线

如果你的目标是构建一个覆盖这些格式的测试数据集，我建议这样做：

1. 用 **Natural Earth** 下载 `shp`
2. 用 **GDAL/OGR** 批量转换为：

- `gdb`
- `tab`
- `mif`
- `kml`
- `dxf`
- `dgn`

3. 用 **USGS/OpenTopography** 下载 `tif`、`dem`、`las`
4. 用 **GDAL** 转为：

- `img`
- `asc`
- `bil`
- `bip`
- `bsq`
- `grd`
- `jpg`
- `png`
- `bmp`

5. 用 **PDAL/laszip** 生成：

- `las`
- `laz`
- `xyz`
- `pts`

6. 用 **MeshLab/CloudCompare/Trimesh** 生成：

- `obj`
- `ply`
- `off`
- `stl`

7. 用 **Cesium 3D Tiles Samples** 获取：

- `b3dm`
- `tileset.json`

8. 对 `dwg`、`pod`、`3mx`、`ecw`、`sid`、`mdb`、`wp/wt/wl`、`udbx` 等专有格式，使用对应商业软件生成或找厂商样例数据。
