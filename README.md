# clone-lin-morpho-mathlib

Experimental **LIN** clone of Morpho Blue `MathLib` — WAD mul/div and the
Taylor compound used by `_accrueInterest`:

`interest = totalBorrowAssets.wMulDown(borrowRate.wTaylorCompounded(elapsed))`

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [morpho-org/morpho-blue](https://github.com/morpho-org/morpho-blue) `339a885801972740eb27f51da110a4675b839d42`
  - `src/libraries/MathLib.sol` sha256 `61c0e20da6b026a356ed86d47da921ef2d69f65e40d9c8b97e7ae18ad497aa22` blob `3c0afc991c3bfa9314498b43047a0675be4a9410`
  - File SPDX MIT. Repository LICENSE is GPL-2.0. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_morpho_mathlib.lin`](src/lin_morpho_mathlib.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/morpho_mathlib_c11.c`](test/oracles/morpho_mathlib_c11.c)
- **Proofs (lin-open only):** `examples/morpho_mathlib/` and `make morpho-mathlib-proof`
- **Class:** EXPERIMENTAL i64-positive. Not mainnet uint256. Not a production Morpho replacement.

Named goldens (`WAD=1e18`):

| id | call | value |
|---|---|---|
| FLOOR | mulDivDown(7, 9, 2) | **31** |
| CEIL | mulDivUp(7, 9, 2) | **32** |
| TAYLOR | wTaylorCompounded(3170979198, 86400) | **274010136628159** |
| ACCRUE | accrueInterest(1e18, 3170979198, 86400) | **274010136628159** |

`3170979198` is ~10% APY as a WAD per-second rate (`0.10e18 / 31536000`).
Ceil is remainder-based (`floor+1` iff `r!=0`), not overflow-prone `(prod+d-1)/d`.
`1e18*1e18/1` fail-closes while naive i64 wraps.
`elapsed` is an argument (Solidity `Morpho.sol` uses `block.timestamp`).

## Reproduce proofs (in lin-open)

```
make morpho-mathlib-proof
```

## Not claimed

Uint256 / mainnet Morpho Blue market parity.
LIN replacing Morpho, Uniswap, or the EVM. TVL or dollar savings. zk soundness.
