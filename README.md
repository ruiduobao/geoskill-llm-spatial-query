# LLM Spatial Query (geoskill-llm-spatial-query)

> Natural-language rule parsing + geopandas spatial query, outputting query-result GeoJSON (offline numpy-equivalent implementation)

---

## 1. Overview

Perform attribute + spatial queries on a vector layer with a single natural-language sentence (e.g., "select parcels with area > 50 and take the top 3 by value"), outputting the matched features as GeoJSON along with a structured query plan. This skill is an **offline numpy-equivalent implementation** of an LLM/NL2GeoSQL spatial question-answering system: it does not depend on a large model, parsing the text into a query plan (fields/operators/values, bbox spatial intent, sorting and Top-N) with regex rules, then executing it with geopandas/shapely; the query plan is persisted as JSON and is fully auditable and unit-testable.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-llm-spatial-query.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `query_result.geojson` | GeoJSON | Matched feature set |
| `query_plan.json` | JSON | Parsed structured query plan |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/exit code) |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
