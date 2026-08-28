# LA-SRPE / LA-RPQ-DS implementation and evaluation audit

Audit date: 2026-08-28 (Asia/Kuala_Lumpur, UTC+08:00)  
Audited repository revision: `e7bc2f6915bff810b482c9ad877d5d62910c4642`  
Audited manuscript: `C:\Users\Poh Wen\Downloads\la_srpe_larpqds_lncs.tex`  
Manuscript SHA-256: `9D61190654902BA6A7C94134608A2BECA65EC88D35554522C11503ED43BAF5BD`

## Executive result

The audited revision contains no implementation. Before this audit added documentation under `evaluation/`, the complete tracked tree was one 705-byte `.gitignore`. There is no `Cargo.toml`, source directory, dependency lockfile, parameter file, test suite, benchmark target, result file, or simulation. Consequently:

- No LA-SRPE primitive operation is implemented.
- No Ling-style lattice operation is implemented.
- No LA-RPQ-DS workflow operation is implemented, even as a simulation.
- No auxiliary cryptographic component is wrapped or implemented.
- No correctness, performance, size, scaling, leakage-trace, security-level, or deployment claim can be supported empirically by this repository.
- The manuscript's current statements that no implementation results are claimed and that its evaluation section is prospective are consistent with the repository.

This audit supports conclusion **C**, not B: the repository does not yet validate even the higher-level workflow.

## 1. Scope and evidence boundary

The repository and manuscript were treated as different evidence sources. The manuscript contains mathematical definitions, algorithms, assumptions, proofs/sketches, leakage declarations, and an experimental plan. Those descriptions were not treated as executable code or instructions to fabricate an implementation. This audit assesses software presence and executable evidence; it does not independently validate the manuscript's cryptographic proofs.

Status meanings used below:

- **Fully implemented as real cryptography:** bespoke cryptographic implementation with concrete parameters and executable tests.
- **Standard-library wrapper:** executable integration of a named, version-pinned cryptographic library.
- **Simulated:** executable workflow/model that does not perform the claimed cryptography and is clearly labelled as such.
- **Placeholder/stub:** callable or scaffolded code with missing/no-op/mock behavior.
- **Not implemented:** no corresponding code or callable artifact exists.

Every requested component is **not implemented**. There are also no stubs or simulations.

## 2. Repository structure

### 2.1 Tree before audit artifacts

```text
LA-RPQ-DS/
`-- .gitignore
```

The initial commit and audited `HEAD` tracked exactly `.gitignore`. It contains generic Rust/Cargo ignore patterns (`debug`, `target`, Rust backup files, PDB files, and tooling output), but an ignore template is not evidence of a Rust project or cryptographic code.

### 2.2 Main files/modules

| Path at audited revision | Role | Evaluation evidence |
|---|---|---|
| `.gitignore` | Generic Rust/Cargo ignore rules | Administrative only; no implementation content. |
| `Cargo.toml` | Not present | There is no build manifest or dependency declaration. |
| `Cargo.lock` | Not present | No package versions are pinned. |
| `src/`, `tests/`, `benches/` | Not present | No code, tests, or benchmark modules exist. |
| Parameter/configuration files | Not present | No concrete LWE or suite parameters exist. |
| Result/data files | Not present | No empirical measurements exist. |

The files under `evaluation/` are audit outputs created after inspection. They are documentation and status data, not an implementation.

## 3. Implementation status

The machine-readable version of the following audit is in `component_status.csv`.

### 3.1 Primitive-level LA-SRPE

| Component | File/module path | Status | Evidence from code | Safe manuscript claim | Unsafe manuscript claim |
|---|---|---|---|---|---|
| LA-SRPE.Setup | None | Not implemented | Only `.gitignore` was tracked; no manifest/source. | Setup is mathematically specified. | The prototype implements or benchmarks Setup. |
| LA-SRPE.KeyGen | None | Not implemented | No key types, sampler, or source. | KeyGen is mathematically specified. | Working LA-SRPE keys are generated. |
| LA-SRPE.TokenGen | None | Not implemented | No token/tree state or source. | TokenGen is mathematically specified. | Server tokens are implemented or measured. |
| LA-SRPE.Encrypt | None | Not implemented | No ciphertext type, arithmetic, or source. | Encrypt is mathematically specified for a bit. | Concrete LA-SRPE encryption works or is benchmarked. |
| LA-SRPE.Update | None | Not implemented | No revocation state, cover, or update type. | Update is mathematically specified. | Update material is generated or measured. |
| LA-SRPE.Transform | None | Not implemented | No server transformation code. | Transform is mathematically specified. | Outsourced transformation is implemented or validated. |
| LA-SRPE.Decrypt | None | Not implemented | No decoding/rounding or rejection path. | Decrypt is mathematically specified. | Decryption correctness/failure rate was tested. |
| LA-SRPE.Revoke | None | Not implemented | No persistent/transactional revocation state. | Revoke is mathematically specified. | Revocation is enforced by the repository. |

### 3.2 Concrete Ling-style lattice operations

| Component | File/module path | Status | Evidence from code | Safe manuscript claim | Unsafe manuscript claim |
|---|---|---|---|---|---|
| TrapGen | None | Not implemented | No lattice dependency, matrix type, or trapdoor code. | A standard TrapGen routine is required by the description. | Trapdoor generation is implemented, secure, constant-time, or measured. |
| SampleLeft | None | Not implemented | No Gaussian sampler, norm check, or parameters. | SampleLeft appears in the mathematical construction. | Sampling is distribution-correct, constant-time, or benchmarked. |
| Gadget inversion | None | Not implemented | No gadget representation/inversion routine. | Deterministic gadget inversion is specified. | Gadget inversion has been implemented or validated. |
| LWE matrix generation | None | Not implemented | No modulus arithmetic, PRNG use, matrix sampler, or parameter set. | The manuscript specifies uniform/noisy matrix sampling. | The repository generates secure concrete LWE instances. |
| Identity/time embedding | None | Not implemented | No HFRD implementation or identity/epoch encoding. | HFRD-based embedding is specified abstractly. | Identity/time embedding is implemented and tested. |
| Zero-inner-product predicate encoding | None | Not implemented | No vector/policy code or modular inner product. | The described relation is zero inner product over `Z_q`. | The repository enforces that predicate relation. |
| Complete-subtree revocation | None | Not implemented | No tree, leaf assignment, KUNodes, cover, or tests. | Complete-subtree revocation is part of the paper construction. | Cover generation/scaling is implemented or measured. |
| One-bit encryption/decryption | None | Not implemented | No ciphertext/noise/decoder implementation. | The concrete paper construction has a one-bit message space. | One-bit correctness or failure probability is empirically established. |
| Bitwise AEAD-key protection | None | Not implemented | No BitProtect adapter or AEAD key type. | The paper proposes one LA-SRPE ciphertext per AEAD-key bit. | The adapter is implemented, practical, or benchmarked. |

### 3.3 LA-RPQ-DS operations

| Component | File/module path | Status | Evidence from code | Safe manuscript claim | Unsafe manuscript claim |
|---|---|---|---|---|---|
| LA-RPQ-DS.Setup | None | Not implemented | No API, suite configuration, registry, or keys. | The composition's Setup is specified on paper. | A working framework setup exists. |
| LA-RPQ-DS.KeyGen | None | Not implemented | No local-key/token composition or delivery code. | KeyGen is specified as calls into LA-SRPE. | Keys/tokens are issued by a prototype. |
| LA-RPQ-DS.Encrypt | None | Not implemented | No AEAD dependency or payload code. | Encrypt is specified to sample a session key and nonce. | Payload encryption is implemented or measured. |
| LA-RPQ-DS.Share | None | Not implemented | No container, serialiser, bit adapter, hash, or signing code. | Share is a proposed authenticated-container construction. | Share is implemented, interoperable, or benchmarked. |
| LA-RPQ-DS.Delegate | None | Not implemented | No verification, Transform loop, or Bind implementation. | Delegate is specified componentwise. | Delegation works or its soundness was experimentally validated. |
| LA-RPQ-DS.Decrypt | None | Not implemented | No parser, verifier, Bind, key reconstruction, or AEAD call. | Decrypt is specified as a composition. | Authorised/rejected cases pass in the prototype. |
| LA-RPQ-DS.Revoke | None | Not implemented | No revocation state or atomic ordering. | Revoke is specified as an LA-SRPE call. | The implementation enforces revocation. |
| LA-RPQ-DS.Update | None | Not implemented | No update envelope, authentication, freshness, or state. | Update is specified; authenticity/freshness are assumptions. | Authenticated updates or rollback resistance exist. |

### 3.4 Auxiliary components

| Component | File/module path | Status | Evidence from code | Safe manuscript claim | Unsafe manuscript claim |
|---|---|---|---|---|---|
| AEAD encryption/decryption | None | Not implemented | No named suite, dependency, nonce API, or calls. | AEAD is a required generic component. | A named AEAD is integrated, nonce-safe, or benchmarked. |
| Post-quantum signatures | None | Not implemented | No ML-DSA/library dependency, key, sign, or verify code. | ML-DSA is a candidate; the theorem remains generic. | ML-DSA is integrated or benchmarked. |
| Hash/domain separation | None | Not implemented | No named hash or executable domain-separated call. | Domain labels are specified and collision resistance assumed. | Hash/domain separation is implemented or tested. |
| Canonical encoding | None | Not implemented | No schema, encoder/decoder, canonicality checks, or vectors. | A versioned, injective, length-delimited encoding is required. | A stable/interoperable canonical format exists. |
| Pseudonymous handle generation | None | Not implemented | No handle generator; the paper exposes `u` and stable `cid`. | Baseline identities/object identifiers are declared linkable. | Users/objects are anonymous, pseudonymous, or unlinkable. |
| Update authentication/freshness | None | Not implemented | No authority signature, monotonic state, replay cache, or durable storage. | Authentic/fresh updates are assumptions; rollback resistance is out of scope. | Rollback/replay is detected by the implementation. |
| Benchmark scripts | None | Not implemented | No benchmark target, runner, schema, or results. | Section 9 is a measurement plan. | Timings, sizes, scalability, or practicality are repository-backed. |
| Test suite | None | Not implemented | No tests/fixtures/runner; Cargo cannot find a manifest. | Correctness/negative cases are prospective. | Correctness, malformed-input safety, revocation, rollback, or nonce handling was validated. |

## 4. Correctness-test audit

### 4.1 Existing tests

No tests exist. As a diagnostic probe, the following command was attempted:

```powershell
cargo test --all-targets
```

It exited nonzero before compilation:

```text
error: could not find `Cargo.toml` in `C:\Github\LA-RPQ-DS` or any parent directory
```

This is evidence that no Cargo test target exists; it is not a failed cryptographic test run. `cargo metadata --format-version 1` failed for the same reason.

No executable tests were added because there is no language manifest, API, data type, parameter set, or implementation to test. Adding passing mocks would create misleading evidence. The concrete proposed suite is in `CORRECTNESS_TEST_PLAN.md`.

### 4.2 Requested cases and present outcome

| Requested case | Present outcome | Required disposition before an implementation claim |
|---|---|---|
| Authorised non-revoked recipient decrypts | Not runnable | End-to-end exact-payload success test. |
| Non-authorised recipient fails | Not runnable | Nonzero-inner-product rejection with no partial output. |
| Revoked recipient fails after update | Not runnable | Atomic revoke/commit/update test at the effective epoch. |
| Wrong epoch fails | Not runnable | Reject older, newer, and mismatched signed updates. |
| Modified owner signature fails | Not runnable | Reject bit flips and cross-owner substitution before plaintext. |
| Modified payload fails | Not runnable | Reject through body binding/signature or AEAD; release no plaintext. |
| Modified associated data fails | Not runnable | Reject both encoding/binding and AEAD modifications. |
| Malformed ciphertext fails | Not runnable | Strict parser/dimension/canonicality tests; no panic or excessive allocation. |
| Update rollback | **Explicitly unsupported** by the paper model; no code | Either add authenticated monotonic durable state and tests, or retain an explicit unsupported statement. |
| Nonce reuse | **Unsupported**; no code/API | Either add atomic reuse prevention and tests, or state reliance on the suite's nonce-generation policy without reuse detection. |

The manuscript also correctly states that revocation does not retroactively revoke material/ciphertexts from earlier epochs. A future suite must test this separately so that a supported forward-revocation behavior is not confused with backward secrecy.

## 5. Benchmark and scaling audit

### 5.1 Measurements run

No cryptographic benchmark was run because no real, wrapped, simulated, or stubbed operation exists. No numeric result can be produced honestly, including object sizes: there are no serialized object types or parameters from which to obtain them. Accordingly, no `benchmark_results.csv` was generated. `component_status.csv` is an audit inventory, not a benchmark result.

The requested primitive timings and sizes, LA-RPQ-DS timings/overheads, and auxiliary timings are all **unavailable**. There is also no workflow simulation to label or measure.

### 5.2 Scaling experiments

None of the user/revocation/predicate/payload sweeps can run. The blocker is absence of an implementation, not merely excessive scale. Reducing the grids would not make an absent algorithm benchmarkable and would risk turning a mock into purported evidence.

The future protocol in `BENCHMARK_PLAN.md` preserves the requested settings:

- users: 10, 50, 100, 500, 1000;
- revoked users: 0, 1, 5, 10, 50, 100 where `r <= N`;
- predicate dimension: 2, 4, 8, 16, 32;
- payload: 1 KiB, 10 KiB, 100 KiB, 1 MiB, 10 MiB;
- both clustered and scattered revocation sets, with actual complete-subtree cover size recorded.

The plan requires raw per-trial CSV and median, IQR, and a declared high percentile. It also separates real auxiliary-library measurements from real lattice-core measurements and from any future workflow simulation.

## 6. Leakage-related trade-off analysis

Because there is no executable system, the repository currently emits no protocol transcript and cannot provide empirical leakage traces. The following is the **manuscript-declared baseline profile**, not an observation of software behavior.

| Requested leakage item | Repository observation | Manuscript-declared baseline if implemented | Trade-off / safe interpretation |
|---|---|---|---|
| Policy/predicate dimensions | No implementation | `ell`, `kappa`, token/ciphertext sizes are visible. | Dimension and size-hiding must not be claimed. |
| Predicate vector/policy label | No implementation | Server record exposes `u`, predicate vector `x`, token, and path. | Server-side predicate privacy is vacuous in the baseline. |
| Revocation-list size | No implementation | Concrete leakage explicitly exposes epoch, cover `Y_t`, `|Y_t|`, update size/repetition; revocation events are in the generic transcript. It does not separately promise that `|RL|` is hidden. | Cover/event evolution can reveal or permit inference about revocation; do not claim revocation-list privacy. |
| Complete-subtree cover size | No implementation | `Y_t` and `|Y_t|` are public. | Compactness may reduce traffic but exposes tree structure correlated with revocations. |
| Update material size | No implementation | Exposed, including repetition. | Size/equality-pattern privacy is not provided. |
| Transformation transcript size | No implementation | Request size, identity `u`, epoch `t`, `cid`, path-cover intersection outcome, timing, and repetition are visible. | Transformation anonymity/access-pattern hiding is not provided. |
| Visible identity/user handle | No implementation | User identity `u` and owner identity `O` are visible; no pseudonymous-handle mechanism is specified. | Do not call identities anonymous or pseudonymous. |
| Object/ciphertext identifier | No implementation | Stable content identifier `cid` is visible. | Object equality and reuse are linkable. |
| Timing/repetition | No implementation | Explicitly exposed for updates and transformation requests. | Frequency and temporal correlation remain observable. |
| Repeated requests linkable | No implementation | Yes, via declared repetition plus stable `u`, `cid`, epoch, routing, and timing fields. | Unlinkability is expressly unsupported. |

The data-sharing container additionally declares version, suite, owner, epoch, payload length, associated-data length and contents, nonce, `cid`, timing, and repetition as visible. Associated data must therefore not carry secrets. The intended hidden values (subject to the manuscript's selective/conditional qualifications) are the payload, session key, encrypted attribute, local decryption keys, trapdoors, and private randomness. This is a paper-level intended profile, not a software-verified property.

## 7. Reproducibility record

| Item | Recorded value |
|---|---|
| Repository | `https://github.com/TM-RnD-Project/LA-RPQ-DS.git` |
| Revision | `e7bc2f6915bff810b482c9ad877d5d62910c4642` (`main`) |
| Revision timestamp | 2026-08-28T16:00:32+08:00 |
| Initial worktree state | Clean; only tracked `.gitignore` present |
| Audit run time | 2026-08-28T16:05:15+08:00 (2026-08-28T08:05:15Z) for captured host metadata |
| Time zone | Windows `Singapore Standard Time` (UTC+08:00; same current offset as Asia/Kuala_Lumpur) |
| OS | Microsoft Windows 11 Home Single Language, version 10.0.26200, build 26200 |
| CPU | Intel Core i9-14900HX, 32 logical processors |
| Memory | 16,790,597,632 bytes (15.64 GiB) |
| Rust compiler | `rustc 1.85.1 (4eb161250 2025-03-15)` (host tool only; unused by project) |
| Cargo | `cargo 1.85.1 (d73d2caf9 2024-12-31)` (host tool only; no manifest) |
| Python | 3.12.10 (host tool only; unused by project) |
| PowerShell | Windows PowerShell Desktop 5.1.26100.9168 |
| Git | 2.55.0.windows.3 |
| LaTeX compiler | Not available on the audit host; `tables.tex` was structurally checked but could not be compile-tested. |
| Project package/library versions | None; no dependency manifest/lockfile |
| Build command | None exists. `cargo metadata --format-version 1` fails because `Cargo.toml` is absent. |
| Test command | No project command exists. Diagnostic `cargo test --all-targets` fails before compilation because `Cargo.toml` is absent. |
| Benchmark command | None exists; no benchmark target/script. |
| Random-seed policy | None exists. The manuscript requires cryptographic randomness and future recording of the seed policy. |
| Thread/optimization policy | None exists. |

These host versions do not establish a build environment because there is no project definition and no dependency selection.

## 8. Safe manuscript claims

The following statements are supported by the repository/manuscript distinction:

1. "LA-SRPE and its Ling-style lattice instantiation are specified mathematically; no software implementation is supplied in the audited repository."
2. "LA-RPQ-DS is a proposed composition of LA-SRPE, AEAD, post-quantum signatures, hashing/domain separation, and canonical encoding."
3. "The concrete paper description uses one-bit LA-SRPE messages and proposes bitwise protection of an AEAD session key."
4. "The baseline leakage profile deliberately exposes the server predicate vector, dimensions, update cover and size, identity/epoch/object routing metadata, timing, and repetition."
5. "The current repository contains no correctness tests, benchmark programs, parameter sets, measurements, or deployment study."
6. "The Implementation and Evaluation section is a prospective experimental plan; no numerical efficiency, scalability, or practicality result is claimed."
7. "Rollback resistance and nonce-reuse detection are not implemented; rollback is outside the stated concrete model."
8. "Any security statement about the concrete construction remains a theoretical, selective, CPA-style, and conditionally QPT-qualified manuscript statement; it is not empirically validated by this repository."

The example safe statement "the prototype validates the LA-RPQ-DS workflow and leakage-trace instrumentation" is **not** safe here, because no prototype or instrumentation exists.

## 9. Unsafe manuscript claims

The repository does not support any of the following:

1. The concrete LA-SRPE lattice primitive, any one of its eight algorithms, or any underlying trapdoor/gadget/LWE sampler is implemented.
2. LA-RPQ-DS workflow correctness has been validated, even using simulation.
3. A standard AEAD, ML-DSA/post-quantum signature, or hash/canonical encoding library is integrated.
4. The code achieves a stated classical or post-quantum security level or uses estimator-validated parameters.
5. Any timing, serialized size, storage, communication, scaling, decryption-failure, noise-margin, or cover-size result exists.
6. The system is efficient, practical, scalable, production-ready, constant-time, side-channel resistant, interoperable, or deployable.
7. Revoked/wrong-policy/wrong-epoch recipients are rejected in code, or malformed/tampered inputs fail safely.
8. Update authentication, update freshness, rollback/replay detection, atomic revocation ordering, or nonce-reuse rejection is implemented.
9. Policies, identities, covers, objects, access patterns, or repeated requests are anonymous, hidden, or unlinkable.
10. Results from another ABE/RLWE system or a future workflow mock measure this matrix-LWE LA-SRPE construction.

## 10. Recommended manuscript edits

The manuscript is already substantially aligned with this audit: its abstract says no implementation results are claimed; Section 9 calls itself a prospective design; and the limitations section says no implementation, parameter set, benchmark, side-channel analysis, or deployment study is supplied. Recommended changes are therefore precision edits, not invented results:

1. Retain those disclaimers verbatim in the submitted version and do not rename Section 9 simply "Evaluation". "Implementation and Evaluation Plan" or "Prospective Evaluation Methodology" is accurate.
2. Add a repository-availability/status sentence with the audited revision: the repository currently contains no source code, build manifest, tests, parameters, or benchmarks. If this repository will be public, avoid calling it an "artifact" or "prototype" until that changes.
3. When saying "we instantiate," distinguish a **mathematical construction/instantiation in the manuscript** from a **software implementation**. Do not use "implemented instantiation" for the current artifact.
4. Keep every performance table schema-only. Do not include zeroes, estimated byte counts, copied parameters, synthetic sleeps, or results from an unrelated ABE implementation as measurements.
5. In the correctness-test plan, distinguish revocation effective for current/future epochs from retroactive revocation/backward secrecy, which the manuscript disclaims.
6. Clarify the future nonce contract: either require atomic nonce-reuse detection/persistence, or explicitly state that the chosen suite uses probabilistic nonce generation and that the prototype does not detect reuse. A test requirement alone does not define the mechanism.
7. Turn update authenticity/freshness from a composition assumption into a concrete signed, versioned, monotonic update-envelope design before claiming implemented revocation under an active storage adversary. Persist highest-seen epochs across restart if rollback detection is claimed.
8. Require a named AEAD, named signature parameter set, named hash, byte-level canonical schema, dependency versions, lattice parameter file, estimator output, and test vectors before any implementation evaluation.
9. If a workflow simulation is added first, label every output "workflow simulation" and state that it does not benchmark LA-SRPE. Conclusion B becomes appropriate only when the higher-level workflow and real auxiliary cryptography actually exist.
10. Use conclusion A only after real lattice routines, full algorithms, negative tests, fixed parameters, serialization, and reproducible benchmarks are present. Passing workflow tests around a mock primitive is insufficient.

## 11. Produced audit artifacts

- `IMPLEMENTATION_EVALUATION_REPORT.md`: this report.
- `component_status.csv`: machine-readable exhaustive 33-component classification.
- `CORRECTNESS_TEST_PLAN.md`: concrete future positive/negative acceptance suite, including explicit unsupported cases.
- `BENCHMARK_PLAN.md`: measurement schema, requested metrics, scaling grid, and simulation-labelling rules.
- `tables.tex`: LaTeX-ready audit summary, test-status, leakage, and claim-boundary tables.

No benchmark-results CSV was produced because no benchmark was run. Creating an empty or synthetic results file would risk being mistaken for empirical evidence.

## Final conclusion

**C. “The repository is currently insufficient for manuscript evaluation claims; only an experimental plan should be included.”**
