# crypto-timing-attack-detector roadmap

Written 2026-10-30 from the repo as it stands. No dates on the
future items, because I do not know when I will be back in this
code, and a date I invent here would be a small lie in a public
file.

## Done

- The four timed operations in main.py: early-exit string
  comparison, HMAC-SHA256 verify, AES-256-CBC encrypt, and
  PBKDF2-HMAC-SHA256 derive, each timed with perf_counter over the
  requested runs and logged as one average.
- AES key handling fixed so a key string of any length works: the
  string is hashed with SHA-256 to the 32 byte AES-256 key
  (commit c07ff50).
- num_runs validation: values under 1 are an input error.
- Failure honesty (commit ded5ce3): input and runtime failures
  exit 1 instead of reporting success, and a failed PBKDF2
  derivation propagates to main() instead of being swallowed.
- Documentation and the README aligned with the verified
  behaviour, requirements.txt, and these visuals.

## Working now

- Nothing in flight. No TODO or FIXME notes exist anywhere in the
  code, checked 2026-10-30.

## Next

- A first test suite. The repo has none today, and the timing
  functions return their times lists, so they are testable as they
  stand.
- Decide whether the returned per-run times should ever be printed
  or summarised. Today only the average leaves the function, and
  that limit is stated in the README.

## Later

- Nothing honest to put here yet. More operations would only be
  worth timing if I had a question about them, and I do not right
  now.

## Not planned

- Statistical timing analysis or a vulnerability verdict. The
  README states the averages demonstrate measurement only, and
  turning this bench into a detector that pronounces on
  exploitability would claim more than these timings can support.
