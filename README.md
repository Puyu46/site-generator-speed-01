# Faster static site generator

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Puyu46/site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Puyu46/site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in replacement for [makefun-site 1.0.0](https://make.fun/c/site-generator-speed-01-run-20261122), written for the [site-generator-speed-01-run-20261122](https://make.fun/c/site-generator-speed-01-run-20261122) competition on [make.fun](https://make.fun). It builds the same static site — same pages, feeds, sitemap and report — from the same `{site, templates, files}` input, byte for byte.

## How it is faster

- **Templates are compiled once per case**, not walked per page: each parsed template becomes straight-line code (with the node walker kept as a fallback if code generation is unavailable), so a render is direct `out.push` calls instead of a switch per node.
- **Templates are parsed once per case** and shared across its hundreds of pages; nothing is cached across cases.
- **One fused Markdown pass**: lexing and rendering happen in a single walk over the lines, with headings, lists, fences and blockquotes handled inline.
- **Bulk inline escaping**: the inline renderer jumps between special characters and copies plain runs verbatim instead of escaping character by character.
- **Cached slugify** with a fast ASCII path, a fast word counter, and array-joined string building throughout.

## Results

On the competition's three practice cases (250–480 Markdown sources each), the output is byte-identical to the reference, at a median **~2.8x** the speed of the house baseline (about 190 ms vs about 540 ms of CPU for the three cases together, measured locally with Node 22). The final heat decides the prize; the entry must beat the baseline at 95% one-sided.

## Files

- `solve.js` — the generator (`export run(input)`), ES module, Node 22, no dependencies, no WASM, no native code.
- `main.ts` — the kit driver: reads `/input/cases.json`, answers each case alone with `run()`, writes `/output/answers.json`.
- `README.md` — this file.
- `LICENSE` — MIT.

## License

MIT — see `LICENSE`.
