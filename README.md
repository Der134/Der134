# Der134 — AI for Science & ML Systems Benchmarks

> **Research Focus:** Leakage-free scientific evaluation, out-of-distribution (OOD) uncertainty quantification, and reliable agent toolchains across **proteins**, **small-molecule drug discovery**, and **bioinformatics agents**.

Standard random splits and point-accuracy leaderboards routinely mask severe deployment failures in biological ML. Across three open-source tracks, my work measures *where* foundation-model scaling, fine-tuning, uncertainty calibration, and autonomous scientific tool use break under real domain shift—and builds reproducible fixes verified on full public benchmarks.

---

## Featured Open-Science Tracks (A + D + F)

| Track | Domain & Scale | Core Empirical Discovery | Repository & Technical Report |
|---|---|---|---|
| **Track A — Proteins (`FLIP2`)** | **317,976** unique protein sequences · **6** datasets · **13** official OOD splits · **ESM-2 (8M → 3B)** probing + 3B LoRA | **Fine-Tuning Inversion & Scaling Recovery:** `ESM-2 3B` LoRA boosts in-distribution accuracy (+0.029) yet degrades every OOD split (−0.18 to −0.30); switching `zscore_fit` to row-wise `l2_norm` reverses inverse scaling on `trpb/two_to_many` (`0.336 → 0.443`), while 5× Deep Ensembles cut RMSE by −59% to −66%. | [`flip2-plm-probing-benchmark`](https://github.com/Der134/flip2-plm-probing-benchmark) · [6-Page PDF](https://github.com/Der134/flip2-plm-probing-benchmark/releases/latest) |
| **Track D — Small Molecules (`ADMET + ChEMBL`)** | **51,827** unique SMILES · **7** model families (`RF`, `XGB`, `ChemBERTa-2`, `MoLFormer-XL`, `GIN-VN`, `GPS-Lite`) × **25** tasks × **3** scaffold seeds (`4,344` conformal runs) | **Universal IID Collapse & Bilateral Mondrian Rescue:** Naive IID split conformal under-covers across every backbone (`0.8854` vs. `90%`, `0.8532` in Far OOD); scaffold + Mondrian calibration restores validity (`0.9009–0.9183` reg; `0.8923/0.8950` bilateral cls), and conformal potency-hERG gating lifts EGFR hit precision from `41.25%` to **`95.40%`** (`97.14%` dual-certified). | [`tdc-admet-ood-conformal-benchmark`](https://github.com/Der134/tdc-admet-ood-conformal-benchmark) · [5-Page PDF](https://github.com/Der134/tdc-admet-ood-conformal-benchmark/releases/latest) |
| **Track F — Scientific Agents (`ScienceBench-Bio50`)** | **40** skills / **66** Python scripts in [`google-deepmind/science-skills`](https://github.com/google-deepmind/science-skills) v1.4.0 × **50** live cross-DB tasks (`ClinVar`, `gnomAD`, `Ensembl VEP`, `UniProt`, `AFDB`) | **Tool Reliability Audit & Upstream Fixes:** Uncovered silent allele/coordinate corruption in `clinvar_api.py` (**[Issue #15](https://github.com/google-deepmind/science-skills/issues/15)**) and relative-path crashes in `gnomad`/`alphagenome` (**[Issue #16](https://github.com/google-deepmind/science-skills/issues/16)**), lifting end-to-end multi-DB pass rate from **`12.0%` (6/50) → `90.0%` (45/50)**. | [`direction_f_science_skills/`](https://github.com/Der134/flip2-plm-probing-benchmark/tree/main/direction_f_science_skills) · [Audit Report](https://github.com/Der134/flip2-plm-probing-benchmark/blob/main/direction_f_science_skills/DIRECTION_F_AUDIT_REPORT.md) |

---

## Cross-Modal Synthesis: Three Takeaways for Deployment

1. **Pretrained Geometry Is Fragile Under Biological OOD:** Unmonitored fine-tuning (`ESM-2 3B` LoRA) and naïve feature standardization (`zscore_fit`) degrade extrapolation when validation splits mirror the training distribution; frozen L2-normalized representations (`ESM-2` for proteins, `MoLFormer-XL` for broad-scaffold ADMET MAE) preserve cross-series structure.
2. **Point Uncertainty Must Be Calibrated & Class-Conditioned:** Neither tree variance nor neural deep ensembles alone prevent OOD coverage collapse (`0.8532` Far-OOD regression coverage) or minority-class neglect (`0.5432`–`0.7971` marginal ICP); combining scaffold-aligned, CQR/adaptive, and Mondrian calibration restores finite-sample guarantees—and threshold-gating outperforms raw LCB reranking under heteroscedastic variance (`EGFR × hERG` MPO: `83/87` hits).
3. **Agent Reliability Bottlenecks on Bioinformatics I/O Contracts:** Scientific LLM agents frequently fail not from reasoning errors, but from silent XML/API parsing bugs and path edge cases in downstream bioinformatics wrappers—making deterministic end-to-end tool benchmarks essential before scaling autonomous variant interpretation (`ClinVar VUS + AlphaGenome`).
