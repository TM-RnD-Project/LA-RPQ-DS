# LA-RPQ-DS correctness and negative-test plan

Status as audited at commit `e7bc2f6915bff810b482c9ad877d5d62910c4642`: **proposed only**. The repository has no implementation, test harness, fixtures, or parameter set. None of the cases below has been executed. These are acceptance criteria for a future implementation, not test results.

## Required fixtures and invariants

A future harness needs deterministic test-only entropy with a recorded seed, while production paths must use operating-system cryptographic randomness. Each fixture must record the full parameter set, suite identifiers, user-to-leaf assignments, predicates and attributes, epoch ordering, revocation events, and byte-exact canonical encodings.

Negative tests must assert a typed rejection (`Err`/`bot`) and must never accept a panic, truncation, implicit default, partial plaintext, or unauthenticated fallback as success. Tests that mutate bytes should use every structural field at least once, not only a single sample offset.

## Required cases

| ID | Case | Minimal arrangement | Required result | Current support |
|---|---|---|---|---|
| T01 | Authorised, non-revoked recipient | Generate `(u,x)` with zero inner product against `y`; issue the current authenticated update; encrypt, share, delegate, and decrypt. | The exact payload and associated data context are recovered; every signature, binding, epoch, and AEAD check succeeds. | Not runnable. |
| T02 | Non-authorised recipient | Use an active `(u,x_bad)` with nonzero inner product against the ciphertext attribute. | Transform or decrypt returns a typed rejection; no session-key bit or payload is returned. | Not runnable. |
| T03 | Revoked recipient after update | Revoke `u` effective at epoch `t`, commit the state, issue the authenticated update for `t`, and protect a key for `t`. | Token path and honest cover do not yield an accepting transformation; decryption rejects. | Not runnable. |
| T04 | Wrong epoch | Combine a ciphertext for `t` with update material for `t-1` and `t+1`, including correctly signed but mismatched material. | Epoch mismatch is rejected before key recovery. | Not runnable. |
| T05 | Modified owner signature | Flip each signature region in turn and substitute another owner's valid signature. | Delegate and recipient Decrypt reject before using transformed material or releasing plaintext. | Not runnable. |
| T06 | Modified payload ciphertext | Flip, delete, append, and reorder payload-ciphertext bytes while holding other fields fixed. | Canonical-body identifier/signature verification or AEAD authentication rejects; no plaintext is released. | Not runnable. |
| T07 | Modified associated data | Change the value, length, encoding, and field position of associated data. | Container binding/signature and AEAD verification reject; no plaintext is released. | Not runnable. |
| T08 | Malformed ciphertext | Exercise truncated matrices/vectors, surplus dimensions, noncanonical integers, invalid node labels, duplicate fields, unknown suite/version, invalid bit count, and overlong lengths. | Strict parsing rejects before allocation or arithmetic where possible; there is no panic or excessive allocation. | Not runnable. |
| T09 | Update rollback | Present an authentic update older than the recipient/server's highest committed epoch; repeat across restart using durable state. | Must reject if rollback resistance is claimed. Otherwise the API and manuscript must explicitly state that rollback is unsupported. | Explicitly unsupported by the current manuscript model; no mechanism exists. |
| T10 | Nonce reuse | Attempt a second encryption under the same AEAD key and nonce, including concurrent requests and state restart. | Must reject atomically if nonce-reuse prevention is claimed. Otherwise the API and manuscript must explicitly state that it relies solely on probabilistic nonce generation and does not detect reuse. | Unsupported; no nonce API or persistent registry exists. |

## Additional high-value cases

- Revoke after a ciphertext epoch: document and test the stated lack of retroactive revocation/backward secrecy.
- Repeat update generation from identical committed state: define whether byte equality is expected; probabilistic material must still have equivalent semantics.
- Mix components from two signed containers: `Bind` must reject source-component substitution.
- Reorder, duplicate, or omit bit-protection components: reject before reconstructing the AEAD key.
- Corrupt one transformed key-bit component at every position: the whole operation must fail without revealing which bit failed externally unless that outcome leakage is declared.
- Use an unknown owner verification key or stale registry binding: reject.
- Exhaust boundary values for `N`, tree depth, `ell`, modulus representatives, epoch counter, and payload length.
- Verify zeroisation only with an implementation-appropriate inspection method; a source-level `drop` is not by itself evidence of physical erasure.

## Release gate

No correctness, enforcement, malformed-input safety, rollback-detection, or nonce-reuse claim should appear as an implementation result until these tests are executable against a fixed revision and parameter file, with pass/fail output archived.
