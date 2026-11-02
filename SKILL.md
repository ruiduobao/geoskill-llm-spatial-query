---
name: geoskill-llm-spatial-query
description: '自然语言规则解析+geopandas空间查询，输出查询结果GeoJSON（离线numpy等价实现）'
---

# LLM空间查询 | LLM Spatial Query

Runs attribute + spatial queries on a vector layer from a single natural-language sentence (e.g., "select parcels with area > 50, keep the top 3 by value"), and outputs the matched features as GeoJSON plus a structured query plan.

This skill is an **offline NumPy-equivalent implementation** of an LLM/NL2GeoSQL spatial Q&A system: it does not depend on large models, parses text into a query plan with regex rules (field/comparator/value, bbox spatial intent, ordering, and Top-N), and executes the plan with geopandas/shapely; the query plan is persisted as JSON and is fully auditable and unit-testable.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2: query on synthetic parcels (offline)

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 117.0 40.0 --synthetic --query "top 3 areas larger than 50" --output-dir ./out
```

### Example 3: attribute query on real vector data

```bash
python geoskill-llm-spatial-query.py --input parcels.geojson --query "population under 3000" --output-dir ./out
```

### Example 4: spatial extent query

```bash
python geoskill-llm-spatial-query.py --bbox 116.0 39.0 116.5 39.5 --synthetic --query "parcels in range" --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `query_result.geojson` | GeoJSON | Set of matched features |
| `query_plan.json` | JSON | Parsed structured query plan |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |

## Data Source / 数据源 / Source

Local vector files (GeoJSON/Shapefile), or a `--synthetic` regular parcel grid layer.

## Privacy / 隐私声明 / Privacy

- Offline by default; `--synthetic` mode requires no network at all.
- All processing is done locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
