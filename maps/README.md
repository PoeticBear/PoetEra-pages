# 苏轼主要足迹底图

`su-shi-region.svg` 来自 Natural Earth 1:110m 的 land 与 coastline 数据，裁取东经 101°—124°、北纬 17.5°—40°，以区域等距矩形投影绘制。只保留自然陆地与海岸，不绘制行政边界。

- 上游：[Natural Earth vector](https://github.com/nvkelso/natural-earth-vector)
- 图层：[land](https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_110m_land.geojson)、[coastline](https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_110m_coastline.geojson)
- 权利：[Natural Earth public domain](https://www.naturalearthdata.com/about/terms-of-use/)
- 下载日期：2026-11-13。
- 生成脚本：`scripts/build-life-map.mjs`。输入文件放在 `tmp/life-research/{land,coastline}.geojson`，运行脚本即可重建。

人物节点位于 `content/su-shi-journey.json`。城市坐标依据 [GeoNames cities15000](https://download.geonames.org/export/dump/cities15000.zip)，采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)，数据提供方为 GeoNames；对应记录 ID 保存在每个地点的 `geoNameId` 中。凤翔为今城关附近的约略点，参考《凤翔县志》地理范围。儋州以现代那大附近示意，未将其当作苏轼居所精确坐标。

这些点与连接线是主要节点的阅读导航，不是完整行程、道路导航、宋代地图或对历史行政区的复原。多个同城节点保留不同时期的经历；连线仅表达选取节点的先后次序。
