# biome_rowan (fork)

A fork of [`biome_rowan`](https://crates.io/crates/biome_rowan) **0.5.8** — Biome's
lossless syntax tree library — with a larger limit on the number of syntax kinds.

## Why

`SyntaxKindSet` is a fixed-size bit set: upstream it holds `[u128; 4]`, i.e. at most
**512** syntax kinds per language. A full SQL grammar (Postgres DDL and PL/pgSQL in one
tree) needs more than that, and generation fails as soon as a language crosses the limit.

## What changed

The only change is in `src/ast/mod.rs`: the bit set is widened to `[u128; 8]`
(`KIND_SET_WORDS = 8`), allowing **1024** syntax kinds. Everything else — the API, the
version number (`0.5.8`), the behaviour — is identical to the crates.io release, so the
fork is a drop-in replacement.

See the commit history: the first commit is the unmodified crates.io source, the second
one is the change.

## Usage

Because `biome_parser`, `biome_diagnostics` and other Biome crates depend on
`biome_rowan` from crates.io themselves, the fork has to replace it everywhere at once.
Use a `[patch.crates-io]` entry rather than a plain dependency:

```toml
[patch.crates-io]
biome_rowan = { git = "https://github.com/Stonum/biome_rowan.git" }
```

It is used together with the [biome_parser fork](https://github.com/Stonum/biome_parser),
which lifts the matching limit on token kinds.

## License

Same as upstream Biome: dual-licensed under [MIT](LICENSE-MIT) or
[Apache-2.0](LICENSE-APACHE), at your option. Original work © Biome Developers and
Contributors.
