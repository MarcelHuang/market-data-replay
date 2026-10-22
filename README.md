# market-data-replay

Ingest a multi-GB public trade dataset into partitioned Parquet, process it through a bounded-memory streaming path, replay it from the command line, and benchmark pandas, polars and a pure-Python generator pipeline on the same job.

> **Status:** scaffold only. Nothing below is implemented yet.

## Setup

```bash
uv sync          # creates .venv with Python 3.15 and the dev tools
uv run pytest
uv run ruff check && uv run ruff format --check && uv run mypy src
```

## Design

TODO

## Data and schema

TODO: the dataset and its licence, how `scripts/download.py` fetches it (data is never committed), column types, timestamp unit and timezone, partitioning.

## Streaming and peak RSS

TODO: how the streaming path bounds memory, and how peak RSS was measured.

## Benchmark method

- **Hardware:** Beelink SER8, AMD Ryzen 7 8745HS (8C/16T), 27 GB RAM, Crucial P3 1 TB NVMe
- **Filesystem:** btrfs with `compress=zstd:3`, on dm-crypt
- **Core pinning:** `taskset`
- **Page cache:** cold (after `drop_caches`) and warm runs
- **Runs:** TODO; report the median and spread (`hyperfine`)

## Results

TODO

## Free-threaded CPython (3.15t)

As of 23 Oct 2026, pandas 3.1.0 and pyarrow 26.0.0 publish free-threaded 3.15t wheels; polars 2.0.0 does not. While that's true, `uv sync --python 3.15t` fails on polars, so a 3.15t variant needs polars moved into an optional extra (or a polars release with 3.15t wheels).

## Domain questions

- Realistic data handling: timestamps, trades vs quotes, exchange quirks.
- What a researcher needs from a replay tool.

## Must ship (by 31 Jan 2027)

- [ ] Multi-GB public trade dataset ingested into partitioned Parquet with explicit types
- [ ] Streaming path with bounded memory, proven by peak-RSS measurements
- [ ] Replay CLI: time window and speed; VWAP and rolling volume per window
- [ ] Benchmark: pandas vs polars vs pure-Python generator pipeline on the same job
- [ ] Written explanation of the differences, with the method above

**Stretch:** free-threaded 3.15t variant · `mmap` variant · cold vs warm page-cache comparison in the write-up.
