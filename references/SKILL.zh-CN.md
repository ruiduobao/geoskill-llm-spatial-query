# LLM空间查询 | LLM Spatial Query

用一句自然语言（如"筛选面积大于 50 的地块，取值最高的前 3 个"）对矢量图层做属性 + 空间查询，输出命中要素 GeoJSON 与结构化查询计划。

本 skill 是 LLM/NL2GeoSQL 空间问答系统的**离线 numpy 等价实现**：不依赖大模型，用正则规则把文本解析成查询计划（字段/比较符/数值、bbox 空间意图、排序与 Top-N），再用 geopandas/shapely 执行；查询计划落盘为 JSON，完全可审计、可单测。

## 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 示例 2：合成地块查询（离线）

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 117.0 40.0 --synthetic --query "面积大于50的前3个" --output-dir ./out
```

### 示例 3：真实矢量属性查询

```bash
python geoskill-llm-spatial-query.py --input parcels.geojson --query "人口小于3000" --output-dir ./out
```

### 示例 4：空间范围查询

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 116.5 39.5 --synthetic --query "在范围内的地块" --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `query_result.geojson` | GeoJSON | 命中要素集合 |
| `query_plan.json` | JSON | 解析出的结构化查询计划 |
| `output-manifest.json` | JSON | 运行清单（输入/输出/QA/退出码） |

## 数据源 / Source

本地矢量文件（GeoJSON/Shapefile），或 --synthetic 规则地块网格图层。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传任何用户数据。

## License

MIT
