# 画中诗句与岸点字体

仅供 `/ink` 右侧竖排诗句与三处岸点题签使用，不替换全站阅读字体。制作及更新日期：2026-11-16。

| 文件 | 内容 | 大小 |
| --- | --- | ---: |
| `ma-shan-zheng-ink.woff2` | Ma Shan Zheng Regular 400；三处摘句、作者、题名与岸点标签所需的 69 个码点、78 个字形 | 27,404 B |
| `noto-serif-sc-ink-punctuation.woff2` | Noto Serif SC Regular 400；11 个标点码点、28 个字形，包含竖排替代字形 | 3,004 B |

两份字体合计 30,408 B。`characters.txt` 记录当前正文、题名与岸点字符；`manifest.json` 记录实际覆盖、字形数、保留表及 SHA-256。汉字与岸点标签采用马善政楷书；诗句标点应单独使用 Noto Serif SC 子集，作者和题名仍可使用原有宋体。

当前完整覆盖 `LANDINGS.name` 的「启程渡口」「听雨竹汀」「临江石亭」共 12 个汉字；在原诗句子集上新增 9 个汉字，原字形覆盖保持不变，Noto 标点字体未改动。

## 官方来源与许可

- Ma Shan Zheng：设计者 Ma ShanZheng；[Google Fonts 官方字体项目](https://github.com/googlefonts/mashanzheng)，[已固定的字体文件](https://github.com/google/fonts/blob/406197b91ff39a93061c2c2eeaee67ddf2ae1f0d/ofl/mashanzheng/MaShanZheng-Regular.ttf)，Version 2.003。原文件 SHA-256：`6d2546bb189c732a8ca29af9e22457b152387d158aa459e4ac2ce1e51788b7fb`。
- Noto Serif SC：[Google Fonts 官方字体文件](https://github.com/google/fonts/blob/8b0a1d0f5983c89bc2b93f1b5fb55f9e252744b5/ofl/notoserifsc/NotoSerifSC%5Bwght%5D.ttf)，Version 2.003-H1。原文件 SHA-256：`050080d9255a86808f2945bffac582b31ef32bc36411ce29563b4961670c66f9`。从可变字体实例化为 400 字重后输出静态 WOFF2。
- 两者均采用 SIL Open Font License 1.1。完整原始版权与许可分别保存在 `OFL-Ma-Shan-Zheng.txt`、`OFL-Noto-Serif-SC.txt`，随字体一起分发；子集仍使用相同许可，不单独售卖字体。

## 竖排处理

Ma Shan Zheng 的官方源文件没有 `vert`、`vrt2`、`vhea` 或 `vmtx`，且缺少中点 `·`。因此不能用它的横排标点冒充竖排支持。

Noto 子集保留 `GSUB` 的 `vert` / `vrt2`、`vhea` / `vmtx`、`BASE` 以及相关定位特性。直接输入原文标点即可由竖排排版替换，不必改写正文字符。实际检查的映射包括：`，→︐`、`、→︑`、`。→︒`、`《→︽`、`》→︾`。子集还覆盖 `·`，两份字体联合覆盖 `characters.txt` 的全部可见字符。

## 更新子集

新增诗句、题名或 `src/lib/ink-world.ts` 的岸点标签时，先更新 `characters.txt`；如出现新标点，同时扩充标点集合。不要直接往正文写入竖排兼容字符。

本机已有 `fontTools 4.60.2` 与 Brotli，未添加项目依赖。按上面的固定版本下载原 TTF 后，可以用下列 Python 方法重建；将 `source` 指向下载文件、`target` 指向本目录对应 WOFF2。主字体传入 `characters.txt` 全文；标点字体传入 `，。、·《》︐︑︒︽︾`，并指定 `weight=400`。

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
