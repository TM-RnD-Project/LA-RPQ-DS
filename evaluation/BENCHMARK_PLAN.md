# LA-SRPE / LA-RPQ-DS benchmark protocol

Status as audited at commit `e7bc2f6915bff810b482c9ad877d5d62910c4642`: **protocol only**. No benchmarkable component or simulation exists, so no benchmark was run and no benchmark-results CSV was generated.

## Preconditions for measurement

Do not time manuscript equations, no-op functions, mock policy checks, simulated sleeps, or byte-count estimates and label them as concrete LA-SRPE measurements. Primitive measurements require working `TrapGen`, `SampleLeft`, gadget inversion, modular matrix arithmetic, error sampling, encoding/decoding, complete-subtree state, strict serialisation, and a documented parameter set. End-to-end measurements additionally require a fixed AEAD suite, fixed post-quantum signature suite, fixed hash, canonical wire format, authenticated updates, and all correctness gates.

Every measured record should include at least:

```text
revision, timestamp_utc, host_id, os, cpu, logical_threads, memory_bytes,
compiler, compiler_flags, dependency_lock_hash, suite, parameter_set,
seed_policy, threads, warmup_count, trial_count, workload, operation,
users, revoked_users, predicate_dimension, payload_bytes, outcome,
sample_index, elapsed_ns, serialized_bytes, cover_nodes, notes
```

Raw per-trial observations should be preserved. Derived tables should report sample count, median, interquartile range, and a declared high percentile (for example p95), rather than only a mean.

## Required measurements

### Primitive-level LA-SRPE

Measure Setup, KeyGen, TokenGen, Encrypt, Update, Transform, Decrypt, and Revoke separately. For each applicable workload, record serialized public-parameter size, master-secret size if safe to inspect locally, user secret-key size, server-token size, ciphertext size, update-material size, transformed-ciphertext size, cover cardinality, and success/rejection outcome. Separate successful and rejected Transform/Decrypt paths.

### LA-RPQ-DS

Measure payload encryption, Share, Delegate, recipient Decrypt, and the combined Revoke/Update workflow. Record final container size, persistent storage by role, upload/download communication, update distribution traffic, and temporary owner state. Report access-control/key-protection cost separately from payload AEAD cost.

### Auxiliary components

Measure AEAD encryption/decryption by payload length; post-quantum signature key generation/sign/verify and signature/public-key sizes; hashing and canonical encode/decode separately and together. These may be reported before LA-SRPE exists only if the exact suites and real library-backed implementations are committed. Such results must be labelled auxiliary-component results, not LA-SRPE or LA-RPQ-DS results.

## Scaling matrix

Once an implementation exists, use the requested grids where valid:

- Maximum/registered users `N`: 10, 50, 100, 500, 1000.
- Revoked users `r`: 0, 1, 5, 10, 50, 100, filtered so `r <= N`.
- Predicate dimension `ell`: 2, 4, 8, 16, 32.
- Payload bytes: 1 KiB, 10 KiB, 100 KiB, 1 MiB, 10 MiB.

For revocation, include both uniformly scattered and clustered revoked leaves because equal `r` can produce different complete-subtree covers. Record the actual cover cardinality. For bitwise key protection, also record the AEAD key length and exact number of one-bit ciphertexts.

Use a staged design if the full Cartesian product is too costly:

1. A one-factor sweep around one documented baseline parameter set.
2. A revocation sweep over every valid `(N,r)` pair for both leaf distributions.
3. A policy sweep over `ell` with fixed `N,r`.
4. A payload sweep with fixed access-control inputs to isolate symmetric cost.
5. Selected interaction points at the smallest, baseline, and largest feasible settings.

Any omitted requested point must be listed with the concrete reason (memory bound, runtime bound, parameter invalidity, or test failure). A smaller setting must not be presented as the requested one.

## Randomness and reproducibility

Production cryptographic paths must use OS cryptographic randomness. Benchmark reproducibility must not weaken production randomness: expose deterministic entropy only behind a test/benchmark feature, record its seed, and prevent that feature in release builds. If deterministic sampling changes the distribution or implementation path, use non-deterministic production entropy and record only the seed policy, not a claimed repeatable seed.

Archive the dependency lockfile, parameter file, raw CSV, aggregation script, exact command line, revision, compiler flags, and machine state. Results from simulated workflow plumbing, if later added, must use an explicit `implementation_kind=workflow_simulation` field and must never be merged into concrete-primitive plots.
