# 画中诗句与岸点字体

仅供 `/ink` 及杭州、密州、黄州各篇章的右侧竖排诗句与岸点题签使用，不替换全站阅读字体。制作及更新日期：2026-11-16。

| 文件 | 内容 | 大小 |
| --- | --- | ---: |
| `ma-shan-zheng-ink.woff2` | Ma Shan Zheng Regular 400；九首诗词全文、作者、题名与九处岸点标签所需的 390 个码点、400 个字形 | 160,228 B |
| `noto-serif-sc-ink-punctuation.woff2` | Noto Serif SC Regular 400；20 个码点、36 个字形，包含竖排标点及主字体缺少的“濛” | 3,892 B |

两份字体合计 164,120 B（约 160.3 KiB）。`characters.txt` 记录当前正文、题名与岸点字符；`manifest.json` 记录实际覆盖、字形数、保留表及 SHA-256。汉字与岸点标签采用马善政楷书；诗句标点应单独使用 Noto Serif SC 子集，作者和题名仍可使用原有宋体。“濛”由 Noto 子集提供单字回退，正文不改为“蒙”。

当前完整覆盖黄州三首正文、杭州《饮湖上初晴后雨·其二》《六月二十七日望湖楼醉书·其一》《夜泛西湖五绝·其四》、密州《江城子·密州出猎》《望江南·超然台作》《水调歌头·明月几时有》，以及全部九处岸点标签和篇章题名。原黄州字形覆盖全部保留；标点同时覆盖正文使用的逗号、顿号、句号、问号，以及渲染器支持的叹号、冒号、分号。

## 官方来源与许可

- Ma Shan Zheng：设计者 Ma ShanZheng；[Google Fonts 官方字体项目](https://github.com/googlefonts/mashanzheng)，[已固定的字体文件](https://github.com/google/fonts/blob/406197b91ff39a93061c2c2eeaee67ddf2ae1f0d/ofl/mashanzheng/MaShanZheng-Regular.ttf)，Version 2.003。原文件 SHA-256：`6d2546bb189c732a8ca29af9e22457b152387d158aa459e4ac2ce1e51788b7fb`。
- Noto Serif SC：[Google Fonts 官方字体文件](https://github.com/google/fonts/blob/8b0a1d0f5983c89bc2b93f1b5fb55f9e252744b5/ofl/notoserifsc/NotoSerifSC%5Bwght%5D.ttf)，Version 2.003-H1。原文件 SHA-256：`050080d9255a86808f2945bffac582b31ef32bc36411ce29563b4961670c66f9`。从可变字体实例化为 400 字重后输出静态 WOFF2。
- 两者均采用 SIL Open Font License 1.1。完整原始版权与许可分别保存在 `OFL-Ma-Shan-Zheng.txt`、`OFL-Noto-Serif-SC.txt`，随字体一起分发；子集仍使用相同许可，不单独售卖字体。

## 竖排处理

Ma Shan Zheng 的官方源文件没有 `vert`、`vrt2`、`vhea` 或 `vmtx`，且缺少中点 `·` 和汉字 `濛`。因此不能用它的横排标点冒充竖排支持。正文的字体栈须将 `var(--font-ink-punctuation)` 放在马善政之后、系统回退之前，以便“濛”使用随项目分发的确定字形。

Noto 子集保留 `GSUB` 的 `vert` / `vrt2`、`vhea` / `vmtx`、`BASE` 以及相关定位特性。直接输入原文标点即可由竖排排版替换，不必改写正文字符。实际检查的映射包括：`，→︐`、`、→︑`、`。→︒`、`？→︖`、`！→︕`、`：→︓`、`；→︔`、`《→︽`、`》→︾`。子集还覆盖 `·` 与 `濛`，两份字体联合覆盖 `characters.txt` 的全部可见字符。

## 更新子集

新增诗句、题名或 `src/lib/ink-world.ts` / `src/lib/ink-chapters.ts` 的岸点标签时，先更新 `characters.txt`；如出现新标点，同时扩充标点集合。如果马善政源文件缺字，将缺字纳入 Noto 回退子集并记录。不要直接往正文写入竖排兼容字符。

使用 `fontTools 4.60.2` 与 Brotli，未添加项目依赖。按上面的固定版本下载原 TTF 后，可以用下列 Python 方法重建；将 `source` 指向下载文件、`target` 指向本目录对应 WOFF2。主字体传入 `characters.txt` 全文；标点字体传入 `濛，。、·《》？！：；︐︑︒︽︾︖︕︓︔`，并指定 `weight=400`。

```python
from fontTools.ttLib import TTFont
from fontTools import subset
from fontTools.varLib.instancer import instantiateVariableFont

def build(source, target, text, weight=None):
    font = TTFont(source, recalcTimestamp=False)
    available = font.getBestCmap()
    options = subset.Options()
    options.layout_features = ["*"]
    options.layout_scripts = ["*"]
    options.name_IDs = ["*"]
    options.name_languages = ["*"]
    options.name_legacy = True
    options.glyph_names = True
    options.notdef_outline = True
    options.recommended_glyphs = True
    options.recalc_timestamp = False
    builder = subset.Subsetter(options=options)
    builder.populate(unicodes={ord(c) for c in text if ord(c) in available})
    builder.subset(font)
    if weight is not None:
        instantiateVariableFont(font, {"wght": weight}, inplace=True,
                                updateFontNames=True)
    font.flavor = "woff2"
    font.save(target)
```

每次重建后重新打开 WOFF2，检查联合字符覆盖、标点的 `vert` / `vrt2` 映射与垂直度量仍存在，并刷新 `manifest.json`。这项字体文件检查不代表浏览器实际竖排视觉已经验收。
