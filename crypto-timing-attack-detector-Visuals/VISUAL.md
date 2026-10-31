# crypto-timing-attack-detector, drawn out

I made these five pictures because the whole program is one file,
main.py, and its shape is easier to see than to read out of argparse
help. Everything in them comes from main.py and the README beside it.

## The visuals

- [architecture.svg](architecture.svg): the system shape. One command
  line at the top, main() checking and dispatching in the middle, the
  four operation functions in a grid, and the one logged average at
  the bottom.
- [sequence.svg](sequence.svg): one timed run as a sequence across
  four lanes: main(), the timing function, the operation itself, and
  logging. perf_counter before, perf_counter after, the elapsed time
  appended, repeated until num_runs is reached.
- [flowchart.svg](flowchart.svg): a run followed as decisions. The
  num_runs check that exits 1, the dispatch, the timing loop, and the
  average at the end.
- [product-map.svg](product-map.svg): the product in one frame: who
  it serves, what goes in, the one line that comes out, and where it
  stops.
- [statistics.svg](statistics.svg): the numbers from the table below,
  drawn as bars on paper.

Each sheet also ships as a PNG beside its SVG: the same drawing,
finished as a photograph of the paper page.

Every sheet here is drawn to look like what it is: a page sketched by
hand on paper, and each one is signed GK somewhere in the drawing,
never twice in the same place, because that is what I do with a page
I made.

## Written product map

What it is: a small timing bench. It runs one of four cryptographic
operations over and over, times each run with time.perf_counter(),
and logs the average time over the requested runs. It is a
demonstration of timing measurement, and the README and I both say
so plainly.

Who it serves: me, and anyone learning how timing measurement is
done, run by run, on their own machine.

Capabilities, grouped:

- The four timed operations: compare_strings (an intentionally
  early-exit string comparison in compare_strings_vulnerable, which
  returns on the first mismatched character and at once when the
  lengths differ), hmac_verify (HMAC-SHA256 computed once, then
  computed and verified per run), aes_cbc_encrypt (AES-256-CBC with
  PKCS7 padding, the key string hashed with SHA-256 to a 32 byte key,
  one random IV per invocation), and pbkdf2_derive
  (PBKDF2-HMAC-SHA256, a fixed 100,000 iterations, a 32 byte output).
- The timing loop itself: each of the four *_timing functions times
  every run, collects the times in a list, logs the average, and
  returns the list.
- Input handling: argparse with defaults for every option, a
  num_runs check (at least 1), and type checks per operation.
  Failures are logged and the program exits 1.

Inputs: the operation name, --num_runs (default 100), --string1 and
--string2, --message, --key, --password, and --salt, each with the
defaults listed in the README. Outputs: one logged line, the average
time in seconds for the operation run. The list of individual times
is returned by the timing functions but never printed.

Boundaries, what it does NOT do: it does not compare timing
distributions, run statistical tests, or produce a vulnerability
verdict. No minimum, maximum, or variance is reported. Timings at
this scale are dominated by interpreter and library overhead, so an
average here is not evidence that an operation is or is not
exploitable.

## Statistics, with citations

Counted 2026-10-30, from the clone this folder ships in, at the
commit before this visuals commit (e45458e).

| Number | Value | How it was counted, and from what |
|---|---|---|
| Files in the repo | 6 | git ls-files in the clone, counted 2026-10-30 |
| Python lines | 201 | Line count over the 1 tracked .py file (main.py), by wc -l style counting over tracked files, binary and image files excluded, 2026-10-30 |
| Markdown lines | 71 | Line count over the 1 tracked .md file (README.md), same method, 2026-10-30 |
| Text lines | 1 | Line count over the 1 tracked .txt file (requirements.txt), same method, 2026-10-30 |
| No-extension lines | 21 | Line count over the 1 tracked file with no extension (LICENSE), same method, 2026-10-30 |
| Commits | 8 | git log in the clone, counted 2026-10-30 |
| First commit | 2025-04-13 | git log, oldest entry, 2026-10-30 |
| Last commit before this one | 2026-10-30 | git log, newest entry, 2026-10-30 |
| Tests | 0 bundled, none run | No test files exist in git ls-files, and the README states there are no bundled tests, checked 2026-10-30. A run attempt on 2026-10-30 (python3 main.py compare_strings --num_runs 5) stopped at import with ModuleNotFoundError for cryptography, which is not installed in this environment and was not installed for this work. No pass is claimed. |
| Languages (API) | Python 7,833 bytes | GitHub API, GET /repos/ShadowGuard-AI/crypto-timing-attack-detector/languages, read 2026-10-30 |
| Repo size (API) | 76 KB | GitHub API, GET /repos/ShadowGuard-AI/crypto-timing-attack-detector, size field, read 2026-10-30 |
| Stars / forks (API) | 1 / 0 | Same API response, read 2026-10-30 |

External statistics: none. No outside number says anything honest
about this repo, so the repo's own numbers are the whole table.

## Roadmap summary

Done: the four timed operations, the exit-1 failure handling, and
the README aligned with the verified behaviour. Working now:
nothing in flight. Next, if I return to it: a first test suite, and
reporting more than the average if I ever want distributions. The
full roadmap, with the Later and Not planned sections, is in
[ROADMAP.md](ROADMAP.md).
