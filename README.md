# onix-radicle-cob-git-store

Product-neutral local Git storage for signed Radicle collaborative objects (COBs). Version 0.2.0 implements the `radicle-cob` 0.21.1 `Store`, `change::Storage`, and `object::Storage` contracts over a **borrowed** git2 0.21.0 repository. It validates namespaced COB ref names, signed change authors, manifest types and signatures, supports exact caller-supplied expected-head compare-and-swap, and streams discovery with a bound on distinct object identities.

## Boundary and consumers

Kiln is the 0.21-cohort consumer: its CI status shell and built-in patch mutation transport own their opened Git repositories and transient commit configurations for the lifetime of borrowed `GitCobStore` values. Kiln retains CI state, status reducers, patch type/evaluator, signer resolution, actor admission, diagnostics, and high-level orchestration. This crate does not create repositories, discover or load signing keys, authorize actors, provide transport, protect filesystem ancestor paths, establish a filesystem snapshot, or prove arbitrary concurrency or release readiness.

The older 0.20-cohort source remains immutable for Cairn and Valence. Version 0.1.1 includes Valence's `RefLayout::RadLegacy` (`refs/rad/<actor>/cobs/...`) and namespaced refs; **version 0.2.0 intentionally supports only** `refs/namespaces/<actor>/refs/cobs/...`. The two dependency cohorts and APIs are not interchangeable. Existing Cairn source revision `af38dae7e0454b1206512794420a3076ea5a8d1f` has Git-archive BLAKE3 `ff1392cab83cb76123c56b0a70ab50fa0893788f5a876e4daf614003905a38e0`. The immutable 0.1.1 baseline is source revision `f78168ea4560ce3a3167b5850bc0c1568e1a1abe` with Git-archive BLAKE3 `bee9d0616208aba318212f624e5b85a14894c2e9f3f43fee5b7312e21acaa8b0`.

The verified 0.21-cohort v0.2.0 source snapshot for Kiln is commit `a87ca01412fb62e69171925d79d2d6b0a23a7e72`, with BLAKE3 `7c2902aee85ebcc923b1e7f9004d6235b550ddf18053194dd6c9286ef05859e6` over `git archive --format=tar a87ca01412fb62e69171925d79d2d6b0a23a7e72`. This points to the tested source snapshot; a subsequent documentation commit recording it does not change that pinned source.

Consumers adopt from an exact Git source commit and verify `git archive --format=tar <commit>` with BLAKE3 before a vendor import. A workspace-relative path is not a durable dependency. No remote, Radicle RID, GitHub endpoint, or network transport is declared here. A separately supplied remote and independent operator publication authorization remain prerequisites; this source preparation does not publish a new revision or alter the retained consumer snapshots.

## Verification

```sh
cargo fmt --all -- --check
cargo test
cargo clippy --all-targets -- -D warnings
nix flake check -L
```

The tests exercise signed COB creation, exact updates on a borrowed repository, stale-head drift even when ancestry would permit an update, bounded discovery across multiple valid namespaces, malformed or symbolic refs, signed-author mismatches, and invalid signatures. Those observations establish only local test behavior, not distributed consistency, arbitrary Git concurrency, actor trust, or release readiness.

## License

Current and new owned code and material are licensed under **AGPL-3.0-or-later**. See `LICENSE` for the complete license and `NOTICE` for the current grant, retained Cairn-contributor attribution, third-party boundary, and historical-grant scope.

`LICENSE-MIT` and `LICENSE-APACHE` retain the original license texts for earlier grants and immutable source snapshots; they do not offer permissive terms for new owned contributions. Prior grants remain effective for content they already cover, including byte-identical content retained here. The source identities above and Kiln's immutable `a87ca01412fb62e69171925d79d2d6b0a23a7e72` vendored snapshot remain unchanged.
