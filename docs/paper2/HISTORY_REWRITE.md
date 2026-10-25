# Paper 2 history rewrite

> **Working documents.** Plans, drafts, status notes and number sheets named here were removed from the current
> tree on 6 October 2026. On 25 October 2026 the plans and design specifications, drafts, status and handover notes,
> figure-selection notes, manuscript outlines and the draft article for X were also removed from the git history
> (`docs/paper2/HISTORY_REWRITE.md`, section "Rewrite of 25 October 2026"); number sheets and check reports
> remain in the history, and the scripts write them again when they run.

> **Layout.** Since 25 October 2026 `paper/` is `papers/paper1/`, `paper2/` is `papers/paper2/` and `derive_surface/` is
> `src/derive_surface/`; paths below are those before the move.

Date: 27 September 2026. Branch `paper2-capital`, local and not pushed. It replaces the local branch
`paper2-kapital` (tip `469cc8e`), which was never pushed. That branch and its backup
`backup/paper2-kapital-vor-integration` still hold the unscrubbed history. Neither may be pushed (no
`git push --all`). They can be deleted once this branch is pushed and its hashes are anchored externally.

Raw account ids and unsalted hashes are deliberately not reproduced here (Addendum 1.4).

After this record was committed, the Paper 2 documents were given English file names. This file uses the new
names, and gives the old name first where it describes the content of old commits.

## Why

1. **Account identifiers and personal data are removed before publication** (audit A01, pre-registration
   Addendum 1.4). The old commits contained the following:
   - raw ids of dominant maker subaccounts and of PM2 override accounts;
   - unsalted `sha256(str(id))[:10]` hashes, which can be reversed by enumeration;
   - chain fixtures that identify single accounts: whole balances, and a day of events with transaction hashes.
2. **The history now builds on the corrected Paper 1 history.** `main` (`d2a2b43`) holds the corrected, cleaned
   and English Paper 1 history. The old base `0afff2c` corresponds to `01857a4` there. The Paper 2 commits now
   sit on `main`.

## What was done

1. **Replay.** The 20 commits `0afff2c..469cc8e` were replayed onto `main` `d2a2b43`, one new commit per
   original, under these rules:
   - **Paper 1 files keep the `main` version.** This covers `docs/paper1/`, `results/p1*`, `scripts/p1_*`,
     `tests/test_p1_*`, `paper/`, `derive_surface/*p1*`, `figdata` and `figures_social`. The Paper 1 finding
     files of the old history were dropped:
     - `docs/paper1/BEFUND_2026-09-25_GEBUEHRENEINHEIT.md`
     - `results/p1_befund/`
     - `scripts/p1_befund_gebuehreneinheit.py`
     - `tests/test_p1_befund_gebuehreneinheit.py`

     `main` holds them in final English form: `docs/paper1/FINDING_2026-09-25_FEE_UNITS.md`,
     `results/p1_finding/`, `scripts/p1_fee_units_finding.py` and `tests/test_p1_fee_units_finding.py`. The
     commit `955d205` (the Paper 1 finding) became empty and was dropped.
   - **Handover.** `docs/paper2/UEBERGABE.md` is `docs/paper2/HANDOVER.md` on `main`. The two German addenda of
     Paper 2 (24 and 25 September 2026) are appended unchanged to `HANDOVER.md`, to be translated later.
   - **`.gitignore`** is the union of both sides.
   - **OS artefacts.** The removal of `.DS_Store`, `paper/.DS_Store` and `paper/social/.Rhistory` (audit A69) is
     kept, although the last two lie under `paper/`. The guard in `tests/test_p2_ids.py` requires it.
   - No other path conflicted.
2. **Scrub.** `git filter-branch --index-filter` ran over `main..paper2-capital` with
   `scripts/p2_history_scrub.py index`. The script does the following:
   - it removes the six account fixtures from `tests/fixtures/p2`;
   - it replaces unsalted hashes by their p2ids labels;
   - it replaces long top-maker ids in `tests/` by 4242;
   - it replaces the ordered top list in `tests/test_p2_ids.py` by the synthetic list.

   Two extensions covered what the script leaves out:
   - **Override account ids in `tests/`.** Each raw PM2 override account id
     (`data/p2/params/*_pm2_overrides_raw.json`) became a synthetic five-digit placeholder, one per account.
     This also applies where the id was written as a hex literal or as a 32-byte log topic. The affected files
     are `tests/fixtures/p2/params_logs.json`, `tests/test_p2_params.py` and `tests/test_p2_history_scrub.py`.
   - **The checked override account in `docs/paper2/get_margin_semantik.md` (now `get_margin_semantics.md`).**
     From the first commit on, its raw id became its p2ids label. The file carried that label from Stage B on in
     any case.

   The scrubbed `results/p2/params/*_pm2_overrides.json` of Stage A to Addendum 4 are byte-identical to the
   labelled files of Stage B.

   Since 27 September 2026 both extensions are part of `scripts/p2_history_scrub.py` (option `--overrides`), and
   its `check` counts these forms too. Rerun on every file state of the old range, the script gives the same
   placeholders and the same results as the run described here.
3. **English messages.** The commit messages were translated into English with `git filter-branch --msg-filter`.
   Type prefixes and `Co-Authored-By` trailers are kept verbatim. Each message still describes the original
   commit, for example "463 tests green" in Stage A, and "history not rewritten" and "Paper 1 finding
   reproducible" in the audit fix.
4. **Dates are kept.** Author and committer (name, e-mail, and date with time zone) of every original commit are
   kept, checked pair by pair. As a result, the rewritten commits are dated 24 and 25 September 2026 before their
   new parent `d2a2b43` (25 September 2026, 12:37 +0200). The dates record when the work was first committed.
5. **New commits on top.**
   - `b637822`: Paper 2 on the revised Paper 1 code. `inference_p1.analysis_frame` now divides fee and rebate
     by the amount. `edge_frame` therefore computes the pre-registered sensitivity in Paper 1's former form
     itself, and the result is bit-identical on the 603,940 fills. A test now passes the amount, and
     `roodman2019` in `paper2/refs.bib` equals `paper/refs.bib` again.
   - The commit of this file.

## Pre-registration and addenda

`docs/paper2/PRAEREGISTRIERUNG.md` is byte-identical in every pair (same git blob, same SHA-256). The diff of
each of these five commits against its parent is identical to the original diff: same files, same changes.
For every commit, the author date equals the committer date, and both equal the date of the original.

| Document | Old commit | New commit | Author and committer date | Git blob of PRAEREGISTRIERUNG.md | SHA-256 of PRAEREGISTRIERUNG.md |
|---|---|---|---|---|---|
| Pre-registration | `1d13227b6f0698f29fcd95d18c5828ae6a0bbc5e` | `cc0a29f655b50c7415e01583b25952e9831bbecd` | 2026-09-24 22:33:50 +0200 | `f251a0c5493e81a9dca98b1aecab54d2bde4349b` | `d777206a923e8eca4747043613914b0ba790e6b6f4d54b14244fb54784e46217` |
| Addendum 1 | `eb534fe80dea66e651589e1ea01dfdeec6040c47` | `85bb0b7e17a3d32731a83de40ab88b8e012f3983` | 2026-09-25 00:26:54 +0200 | `83693818948078d9f308e1083541056cd2924ac4` | `7f5f701d014d8fd5f7c401ef93dc622beaad14bd08fbcc66995b29b6fc3781c3` |
| Addendum 2 | `94652106387bc1f6006e0dbe02ac0a7edb13ea67` | `6005d7c53ccf51d3df7d038025f546eb5d07a6ef` | 2026-09-25 00:29:29 +0200 | `461a9af11090c853e2bbf0495c8f1b42c760f434` | `c2e6d5334c85d7dea95035a7d340cc20e41976c5d27e3cc74b3b6b7ffc8c53b6` |
| Addendum 3 | `bfc34c81c507c51a38e83878422b89d072f9ed18` | `8778432b49f01532809517d592acda4727d7aaa8` | 2026-09-25 00:29:56 +0200 | `55ea5cc2868ae7244fa693af5718171aef78673f` | `536a39cbac9a762d97d38e57ce1ac41439f22f676b3432ff31d8d3b9556c7c09` |
| Addendum 4 | `c4fcb59d61b73549aa90cacbd3b997816f36d823` | `e492ba123c4c81ddc478c0c6ef2246867deeeaae` | 2026-09-25 01:29:58 +0200 | `96986c732c85408daa4e704c519601bb531fdc4b` | `eb5e269571df785c0bc65e0b9ebb230dab9380edb95a951b63656eae07bb05f9` |

The rewrite did not change `PRAEREGISTRIERUNG.md`: the tip of the rewritten history carries the Addendum 4 version.
Afterwards, on 27 September 2026, the file was only extended: Addendum 5 appends the mapping above and changes no
earlier rule. `docs/paper2/PREREGISTRATION.md` (formerly `PREREGISTRATION_EN.md`) translates it and names the git
blob of the extended file.

## All rewritten commits

| Old | New | Author and committer date | Message |
|---|---|---|---|
| `6559b21` | `b2dd70e` | 2026-09-24 22:29:04 +0200 | docs(p2): semantics of get_margin clarified, contradiction 11 vs 1.2 resolved |
| `1d13227` | `cc0a29f` | 2026-09-24 22:33:50 +0200 | prereg(p2): specification and pre-registration before the first capital figure |
| `cccfc01` | `0968031` | 2026-09-24 22:37:01 +0200 | plan(p2): overall plan in seven stages |
| `9b1394f` | `fca1aea` | 2026-09-24 22:38:06 +0200 | feat(p2): throttled chain client with block clock |
| `bd11559` | `d1bdd69` | 2026-09-24 22:59:46 +0200 | docs(p2): literature checked and manuscript skeleton |
| `e7361d2` | `78566f1` | 2026-09-25 00:26:36 +0200 | feat(p2): Stage A - feed history, parameter timelines, SM/PM/PM2 replication, maker holdings |
| `eb534fe` | `85bb0b7` | 2026-09-25 00:26:54 +0200 | prereg(p2): Addendum 1 before the first capital figure (exact on-chain book for H2) |
| `aaec2d3` | `a59f2b9` | 2026-09-25 00:27:22 +0200 | chore(p2): machine-wide lock for memory-heavy jobs |
| `9465210` | `6005d7c` | 2026-09-25 00:29:29 +0200 | prereg(p2): Addendum 2 - fee and rebate are totals per fill |
| `bfc34c8` | `8778432` | 2026-09-25 00:29:56 +0200 | prereg(p2): Addendum 3 - bootstrap and placebo made precise before the inference |
| `c4fcb59` | `e492ba1` | 2026-09-25 01:29:58 +0200 | prereg(p2): Addendum 4 - readings of H2 to H4 before the first test statistic |
| `591d2d5` | `58551d6` | 2026-09-25 01:30:22 +0200 | feat(p2): Stage B - validation, capital per fill, marginal cost, netting, doses, surface |
| `93b42bc` | `f7ba923` | 2026-09-25 02:16:35 +0200 | feat(p2): Stage C - inference H1 to H4 with independent recomputation |
| `955d205` | dropped | 2026-09-25 02:16:35 +0200 | docs(p1): finding on the fee unit (on `main` in final form) |
| `03ee4ca` | `ae90485` | 2026-09-25 02:49:10 +0200 | feat(p2): MM sensitivity H2 with one lib per ratio, API snapshot, CLI |
| `48e4032` | `ae87357` | 2026-09-25 03:49:47 +0200 | feat(p2): Stage D - nine figures, GIF of the capital surface, social cards |
| `3ec74f4` | `1894bad` | 2026-09-25 04:05:41 +0200 | feat(p2): Stage E - manuscript, check chain, referee and numbers round |
| `d51ede0` | `1c29ba8` | 2026-09-25 05:02:45 +0200 | docs(p2): audit of the whole paper (69 findings) |
| `c510913` | `f94bc53` | 2026-09-25 07:40:56 +0200 | fix(p2): audit findings fixed (57 fixed, 9 with a remainder, 3 for the author) |
| `469cc8e` | `fe331b0` | 2026-09-25 07:41:34 +0200 | docs(p2): morning report and handover addendum |

Some commits differ from the original in their own diff, not only in their base:
- `b2dd70e`: `HANDOVER.md` instead of `UEBERGABE.md`, and the label in `get_margin_semantik.md` (now
  `get_margin_semantics.md`);
- `78566f1`, `58551d6`, `f7ba923` and `ae90485`: the scrub;
- `f94bc53`: the scrub, and the Paper 1 files dropped;
- `fe331b0`: `HANDOVER.md`.

The other commits have the same diff as their originals.

## Checks

- **A01, the script's check.** `scripts/p2_history_scrub.py check main..paper2-capital` reports 0 files. It
  reported 12 files over the old range; with the extensions built in, it reports 16 there.
- **A01, a wider scan.** A second scan covered every file of every tree in `main..paper2-capital`, not only the
  Paper 2 roots. It looked for:
  - unsalted hashes of all ids below 250,000;
  - sha256, sha1 and md5 prefixes of the ids of the top ten and of the override accounts;
  - five-digit ids of these accounts, whether written as a decimal token, as a hex literal or as a 32-byte word
    (also in inflated PDF streams);
  - the ordered top list;
  - the six fixture paths.

  None of these categories has a match. Over the old range, the same scan flagged 17 files. Top-ten ids with
  one to four digits cannot be told apart from ordinary numbers. The scan therefore also looks for such a number
  right after an account-like key. It finds three test files, `tests/test_p2_books.py`,
  `tests/test_p2_inference.py` and `tests/test_p2_params.py`, and the matches are synthetic test values (for
  example accounts 5 and 7 in `tests/test_p2_params.py`).
- **End state.** `git diff paper2-kapital paper2-capital` shows differences only in these places:
  - Paper 1 files and other files in their `main` version (`derive_surface/animate.py`,
    `derive_surface/markouts.py`, `docs/derive_api_notes.md`, `docs/superpowers/*p1*`, `tests/conftest.py`);
  - `HANDOVER.md` and `UEBERGABE.md`;
  - the three scrubbed test files;
  - `b637822`;
  - this file.

  The private fixtures were already absent from the tip of `paper2-kapital`.
- **Tests and build.** `python3 -m pytest -q` gives 1004 passed, with the private fixtures under
  `data/p2/fixtures_private`. `python3 scripts/p2_build.py` gives "build clean" (12 pages, 352 numbers checked)
  in a checkout whose repository still holds the old commits.

## Consequences and open points

- **Old hashes are still cited.** The manuscript and several files name the old hashes:
  - `paper2/main.tex` names `1d13227`, `eb534fe`, `9465210`, `bfc34c8` and `c4fcb59`, and also `103c676` of
    Paper 1, which is `81d89dc` on `main`;
  - `derive_surface/social_p2.py` and the social cards name `1d13227` in the footer;
  - so do `docs/paper2/MANUSCRIPT.md`, `NUMBER_CHECK.md`, `FIGURE_SELECTION.md`, `VALIDATION.md`,
    `PREREGISTRATION.md` and `tests/test_p2_number_check.py`.

  In a clone that holds only this branch, the old commits do not exist. There `scripts/p2_build.py` fails with
  nine numbers without source, all of them commit references.

  Still to do:
  1. Write a dated Addendum 5 to `PRAEREGISTRIERUNG.md` with the mapping above and its translation in
     `PREREGISTRATION.md`, including the new blob hash.
  2. Then put the new hashes into `main.tex`, the social cards and the files listed above.
  3. Regenerate `NUMBER_CHECK.md`.

  Status later on 27 September 2026, with the English file names: `paper2/main.tex`, `derive_surface/social_p2.py`
  with the social cards, `NUMBER_CHECK.md` and `tests/test_p2_number_check.py` cite the new hashes, and every
  commit that `main.tex` cites is an ancestor of this branch. In a clone that holds only this branch,
  `scripts/p2_build.py` now reports "build clean".

  Addendum 5 is written. `PRAEREGISTRIERUNG.md` ends with a dated "Nachtrag 5 (27.09.2026)". It gives the reason
  and the old and new commits of the pre-registration and Addenda 1 to 4 with their author and committer times. It
  states that the registered texts are byte-identical, points to the SHA-256 values above and says that the
  pre-registration becomes public only with the push. It changes no earlier rule. `PREREGISTRATION.md` translates
  it as Addendum 5 and names the new blob, and `tests/test_p2_prereg_en.py` expects five addenda. `MANUSCRIPT.md`,
  `FIGURE_SELECTION.md`, `VALIDATION.md`, `HANDOVER.md`, `REPORT_2026-09-25.md` and the master plan now name the
  new hashes; where they describe the old history, they give the old hash with the new one ("old (now new)").
  `AUDIT.md` and `HISTORY_CLEANUP.md` keep the old hashes in their body, each followed by its new one; in the
  command block of `HISTORY_CLEANUP.md` a comment gives the mapping.
  `paper2/main.tex` names Addendum 5 in one sentence of the section "Data, code and pre-registration" (its date is
  the constant `addendum5_day` of `scripts/p2_number_check.py`) and refers to this file for the mapping. Still
  open: the external anchoring, which waits for the push.

  `AUDIT.md`, `HISTORY_CLEANUP.md` and `REPORT_2026-09-25.md` are dated reports about the old history. They keep
  their content and are now in English, under English file names.
- **Missing fixtures in rewritten commits.** The rewritten intermediate commits lack the six account fixtures.
  Their tests that need them do not run in those commits.
- **Limits of the pseudonyms.** Two limits of the pseudonyms still hold:
  - the ranks M1 to M10 can be recomputed from the public tape;
  - the X labels of override accounts can be matched through the public `LibOverrideUpdated` events.
    `tests/fixtures/p2/params_logs.json` keeps the transaction hash of the event whose account topic was
    replaced.

  The rewrite removes the raw ids and the reversible hashes, not these routes.
- **Before the push.** Push only `paper2-capital`. Integrate it with a merge commit (A02), and anchor the new
  hashes externally (OpenTimestamps or OSF).

## Second rewrite (5 October 2026)

Before the first push of Paper 2 the whole history of the repository was rewritten once more, also the history of
Paper 1 on `main`, which had been public since 25 September 2026. Rules:

1. **Authorship.** Every commit names the author, gregor_284. Some early commits (3 September 2026) had named the AI
   coding tool that was used as their author, and most commits named it as co-author in a "Co-Authored-By" line;
   those lines are removed, as are the tool's session links and its branch name in four merge messages.
2. **Language.** The 20 remaining German commit messages are translated into English.
3. **Files.** Example commit lines of this kind are removed from the planning documents under
   `docs/superpowers/plans/`, and two mentions of the tool's branch name in `docs/paper1/REVISION_2026-09-25.md`
   and `docs/paper2/AUDIT.md` are reworded. No other file changes. Both pre-registrations are byte-identical in every
   commit (same git blob, table below).
4. **Times.** Author and committer, date, time of day and time zone of every commit are kept.
5. **Refs.** `main`, `paper1-revision` and `paper2-capital` are rewritten; the tool's side branch is not
   carried over. The pull requests #1 to #5 on GitHub keep their original commits on their pages.

Done with git-filter-repo in a fresh clone; the state before is kept as a bundle outside the repository. Addendum 7
(`7c01e85`) of the Paper 2 pre-registration and Addendum 4 (`9d37c19`) of the Paper 1 pre-registration record the
rewrite.

### Pre-registration commits (hash before 5 October 2026, new hash, blob of the pre-registration file)

| Commit | Before | New | Blob (identical before and after) |
|---|---|---|---|
| p1_prereg | `3fd9caa` | `cf1f432` | `0376d559bba2fb626377cffc4b85b01ef34a0bc0` |
| p1_add1 | `7f67eaf` | `143f695` | `5103b6edb1109cac08e863bdaf624df7cf595c99` |
| p1_add2 | `837595c` | `634c276` | `fc02ab9959756dec32bcfff326881861c795f2f3` |
| p1_add3 | `faf6cea` | `e4a3873` | `6da6185be44a127180d6a9173b2e5fc921aed783` |
| p2_prereg | `cc0a29f` | `c9e9162` | `f251a0c5493e81a9dca98b1aecab54d2bde4349b` |
| p2_add1 | `85bb0b7` | `b5c905c` | `83693818948078d9f308e1083541056cd2924ac4` |
| p2_add2 | `6005d7c` | `60896fe` | `461a9af11090c853e2bbf0495c8f1b42c760f434` |
| p2_add3 | `8778432` | `2416902` | `55ea5cc2868ae7244fa693af5718171aef78673f` |
| p2_add4 | `e492ba1` | `a02058a` | `96986c732c85408daa4e704c519601bb531fdc4b` |
| p2_add5 | `a3282d6` | `8c066a7` | `c2d7378bfa0d51ac63d819ec9ded8571535ab4a6` |
| p2_add6 | `1b46e80` | `6f2436e` | `06a0004cda02936baf42db3101f2bd24e50f54a8` |

### Every commit (author date, hash before 5 October 2026, new hash, message)

| Date | Before | New | Message |
|---|---|---|---|
| 2026-09-03 08:04 | `fd01b5c` | `9d9dbe6` | Derive option surface: data pipeline, Black-76/SVI surface engine, animations |
| 2026-09-03 09:18 | `7a845f1` | `6beafe7` | Initial commit |
| 2026-09-03 10:19 | `823e834` | `f21e900` | Merge pull request #1 from gelatotrade/option-surface-orderbook |
| 2026-09-03 12:04 | `00cc50e` | `e4741ca` | Review fixes, panel-driven surface upgrades, live recording and depth snapshot |
| 2026-09-03 12:20 | `0dcd0cb` | `2ee365f` | README: measured figures (basis, greek noise, skew-stickiness), frame cosmetics, even MP4 frame size |
| 2026-09-03 12:21 | `d20277c` | `35d19d6` | README: decimal commas in the greek-noise table |
| 2026-09-03 12:40 | `b05e9d1` | `8e356d4` | README in English; publish the finished animations; English frame labels |
| 2026-09-03 13:07 | `0b0e3b1` | `229d5a6` | Animations with English frame labels: HYPE tape and shock (same data, same parameters) |
| 2026-09-03 13:07 | `68f05cc` | `b594256` | Animations with English frame labels: BTC, ETH, HYPE live (same data, same parameters) |
| 2026-09-03 13:08 | `946b63d` | `fd365c2` | Remove stale German-labelled still (unreferenced) |
| 2026-09-03 13:12 | `6373cc3` | `db9407a` | Rename animations to *_en so GitHub's image cache serves the English frames |
| 2026-09-03 13:12 | `d67de4c` | `e54fbe6` | Reproduce commands write the *_en media names |
| 2026-09-03 14:34 | `0c07b52` | `6759512` | Merge pull request #2 from gelatotrade/option-surface-orderbook |
| 2026-09-03 15:02 | `8370284` | `ab70741` | Merge pull request #3 from gelatotrade/option-surface-orderbook |
| 2026-09-03 15:09 | `7308914` | `4699368` | Merge pull request #4 from gelatotrade/option-surface-orderbook |
| 2026-09-17 15:47 | `02e2c25` | `0829308` | paper1: full-field option tape in single-page windows |
| 2026-09-17 15:47 | `3fd9caa` | `cf1f432` | paper1: spec, plan, pre-registration (before any markout is computed) |
| 2026-09-17 15:47 | `81259a2` | `ae9ba56` | paper1: vol-feed (SVI) history from Derive Chain |
| 2026-09-17 15:48 | `13ccad7` | `8da0424` | paper1: reference data (settlements, liquidations, maker programmes, vaults, fees, funding) |
| 2026-09-17 15:48 | `7a69919` | `4ee1cab` | paper1: pair maker/taker rows and classify takers |
| 2026-09-17 15:48 | `d155d48` | `e7a6868` | paper1: p1 CLI and vol-feed check script |
| 2026-09-17 15:53 | `c890b83` | `fe5c541` | api: retry unparseable responses; paper1: walk liquidation history in 7-day windows |
| 2026-09-17 15:58 | `5763306` | `aa543d3` | paper1: liquidation history in daily windows with page-size fallback, halving and explicit gaps |
| 2026-09-17 16:07 | `52bb5a6` | `5b67761` | paper1: fixes from adversarial review |
| 2026-09-17 16:11 | `6ce8155` | `ea99f98` | paper1: record probe findings and execution deviations in spec and plan |
| 2026-09-17 16:23 | `00090e3` | `9d85cbc` | paper1: curated vault wallet list with sources (8 of 10 seen on the option tape; the two basis-trade vaults trade no options) |
| 2026-09-17 16:24 | `2499721` | `efe2481` | paper1: settle window 60 min (RFQ maker rows stamped up to 28 min before the fill) |
| 2026-09-17 16:26 | `2c7cf5d` | `be6c037` | paper1: script that writes the data status document from data/p1 (counts only, no markouts) |
| 2026-09-17 16:28 | `90663d2` | `c7666bd` | paper1: mark path (b) — attach latest on-chain SVI by push or signing time and price with Black-76 |
| 2026-09-17 16:43 | `3cc2ca6` | `e0fea45` | paper1: p1 markouts command |
| 2026-09-17 16:43 | `7f67eaf` | `143f695` | paper1: pre-registration addendum 1 (before any markout is computed); plan 2 |
| 2026-09-17 16:43 | `a4a24ca` | `b36e511` | paper1: markouts (paths a/b/c, three units, cells, sweeps, size) |
| 2026-09-17 16:51 | `8f8c70e` | `3663ad2` | paper1: feed check verifies svi_vol against the freshest signed curve (exact to 1e-5) and measures the on-chain push lag |
| 2026-09-17 17:13 | `df2a3ec` | `ab59856` | paper1: data status compares the rebuilt mark with the tape by clock, forward and quarter |
| 2026-09-17 18:19 | `fea9488` | `114d6d9` | paper1: fetch liquidation windows in parallel with progress logging |
| 2026-09-17 18:43 | `fad0dc4` | `234fe8d` | paper1: liquidations use the largest working page size (same auctions for every size, more bids for larger pages) |
| 2026-09-17 22:07 | `2690cdf` | `b903a0b` | paper1: ref can reuse existing liquidation files; maker scores survive network give-ups |
| 2026-09-17 22:13 | `b9c7b12` | `61e04ce` | paper1: pilot data status and vol-feed check (no markouts) |
| 2026-09-18 00:02 | `837595c` | `634c276` | paper1: pre-registration addendum 2 (H3 event date, net-edge decomposition, H1 metric); plan 3 |
| 2026-09-18 00:03 | `09b7ad4` | `f945ec7` | paper1: inference (wild cluster bootstrap, DiD with placebos, net edge per cell) |
| 2026-09-18 00:06 | `b7ed5bc` | `fb108c6` | paper1: p1 inference command, pilot results and figure sheet |
| 2026-09-18 00:09 | `b560a1e` | `04b5221` | paper1: decompose the markout into half spread and adverse selection per counterparty class |
| 2026-09-18 08:34 | `a473b25` | `c6bdb11` | paper1: figure style (CAS column widths, Okabe-Ito palette, PDF+PNG, robust limits) |
| 2026-09-18 08:35 | `de7be49` | `34d13d5` | paper1: CAS manuscript skeleton that builds with tectonic and embeds a styled figure |
| 2026-09-18 08:37 | `fef7fe9` | `c1d1129` | paper1: robustness of the two mark paths (agreement rises to 0.90 correlation when path a is measured near the target time) |
| 2026-09-18 08:38 | `c0931cf` | `0a889c8` | paper1: figure aggregations (horizon curves, delta-tenor matrix, weekly series, waterfall, example fill) |
| 2026-09-18 08:39 | `69aea60` | `c7bf089` | paper1: exact, fast cluster bootstrap of the median for figures (weighted median over a single sort) |
| 2026-09-18 09:07 | `818b56d` | `e17065a` | chore(p1): Path comparison in the results, plan 4 created, numpy warning contained |
| 2026-09-18 09:09 | `b72ef19` | `a7286e2` | feat(p1): Curve reconstruction for the mechanism figure, example fill corrected |
| 2026-09-18 09:10 | `16ce42a` | `ac0df45` | feat(p1): Result loader and cell matrix for the figures |
| 2026-09-18 09:11 | `c655a12` | `df8f628` | docs(p1): Findings from building the figures recorded |
| 2026-09-18 09:15 | `9c98663` | `cb02f29` | fix(p1): One bootstrap size per run, otherwise the sensitivity contradicts the headline number |
| 2026-09-18 09:17 | `6319b51` | `a408bfe` | data(p1): Inference recomputed with one bootstrap size |
| 2026-09-18 09:20 | `24f9a23` | `1d59cbd` | docs(p1): Figure selection and jury reasoning recorded |
| 2026-09-18 09:23 | `9772ee9` | `76ab684` | plan(p1): Nine figures specified |
| 2026-09-18 09:34 | `3cf54a8` | `8b0af87` | feat(p1): Nine figures, CLI subcommand and placebo estimators |
| 2026-09-18 09:44 | `4ce1bec` | `3dffd82` | fix(p1): Figures revised after review |
| 2026-09-18 09:48 | `494d952` | `223dc94` | feat(p1): Figures in the manuscript, check script and figure sheet |
| 2026-09-18 09:48 | `83d2e1f` | `7437272` | docs(p1): Numbers updated after the rerun |
| 2026-09-18 09:49 | `1680a4b` | `c4c633b` | plan(p1): Plan 4 abgeschlossen |
| 2026-09-18 11:30 | `714ecbe` | `3d62bea` | feat(p1): Manuscript written |
| 2026-09-18 12:21 | `fcdfde3` | `48b4377` | feat(p1): Cards and article for X |
| 2026-09-18 12:22 | `30ce2fe` | `071383c` | fix(p1): Pin the card style completely, otherwise the print style changes the image size |
| 2026-09-19 15:45 | `81d89dc` | `2139e62` | feat(p1): Statements for SSRN and a guard for the build |
| 2026-09-24 12:42 | `01857a4` | `de3291e` | docs(p2): Handover for the next session |
| 2026-09-24 22:29 | `b2dd70e` | `6642ff8` | docs(p2): semantics of get_margin clarified, contradiction 11 vs 1.2 resolved |
| 2026-09-24 22:33 | `cc0a29f` | `c9e9162` | prereg(p2): specification and pre-registration before the first capital figure |
| 2026-09-24 22:37 | `0968031` | `cc714b8` | plan(p2): overall plan in seven stages |
| 2026-09-24 22:38 | `fca1aea` | `2748b13` | feat(p2): throttled chain client with block clock |
| 2026-09-24 22:59 | `d1bdd69` | `55629ff` | docs(p2): literature checked and manuscript skeleton |
| 2026-09-25 00:26 | `78566f1` | `c8ab517` | feat(p2): Stage A - feed history, parameter timelines, SM/PM/PM2 replication, maker holdings |
| 2026-09-25 00:26 | `85bb0b7` | `b5c905c` | prereg(p2): Addendum 1 before the first capital figure (exact on-chain book for H2) |
| 2026-09-25 00:27 | `a59f2b9` | `b8c676a` | chore(p2): machine-wide lock for memory-heavy jobs |
| 2026-09-25 00:29 | `6005d7c` | `60896fe` | prereg(p2): Addendum 2 - fee and rebate are totals per fill |
| 2026-09-25 00:29 | `8778432` | `2416902` | prereg(p2): Addendum 3 - bootstrap and placebo made precise before the inference |
| 2026-09-25 01:29 | `e492ba1` | `a02058a` | prereg(p2): Addendum 4 - readings of H2 to H4 before the first test statistic |
| 2026-09-25 01:30 | `58551d6` | `f97a117` | feat(p2): Stage B - validation, capital per fill, marginal cost, netting, doses, surface |
| 2026-09-25 02:16 | `f7ba923` | `c55d47b` | feat(p2): Stage C - inference H1 to H4 with independent recomputation |
| 2026-09-25 02:49 | `ae90485` | `743d403` | feat(p2): MM sensitivity H2 with one lib per ratio, API snapshot, CLI |
| 2026-09-25 03:49 | `ae87357` | `74a89c5` | feat(p2): Stage D - nine figures, GIF of the capital surface, social cards |
| 2026-09-25 04:05 | `1894bad` | `5e256d8` | feat(p2): Stage E - manuscript, check chain, referee and numbers round |
| 2026-09-25 05:02 | `1c29ba8` | `89ab2d4` | docs(p2): audit of the whole paper (69 findings) |
| 2026-09-25 07:40 | `f94bc53` | `afb3006` | fix(p2): audit findings fixed (57 fixed, 9 with a remainder, 3 for the author) |
| 2026-09-25 07:41 | `fe331b0` | `e8b5b2c` | docs(p2): morning report and handover addendum |
| 2026-09-25 08:04 | `33993e7` | `fd5abd6` | plan(p1): Correction of the fee unit for the SSRN revision |
| 2026-09-25 09:10 | `faf6cea` | `e4a3873` | fix(p1): Fee and rebate per contract, map per notional consistent (revision of 25 September 2026) |
| 2026-09-25 10:49 | `d2e0181` | `4b4b8a4` | fix(p1): Lorenz file with salted wallet pseudonyms only |
| 2026-09-25 12:03 | `22191be` | `356a1f9` | Merge pull request #5 from gelatotrade/paper 1 correction |
| 2026-09-25 12:37 | `1fa723d` | `9c3828c` | docs(p1): English documentation, file names and generators |
| 2026-09-25 12:37 | `d2a2b43` | `42b529f` | Merge branch 'paper1-revision': English documentation for Paper 1 |
| 2026-09-27 12:31 | `b637822` | `c5c36ba` | fix(p2): keep Paper 2 working on the revised Paper 1 code |
| 2026-09-27 12:34 | `13795ac` | `b36daec` | docs(p2): record of the history rewrite (new base main, account ids removed) |
| 2026-09-27 13:38 | `19cfab8` | `2ce6d18` | docs(p2): English documentation, generators and manuscript update |
| 2026-09-27 13:38 | `8edae28` | `61bf971` | docs(p2): rename Paper 2 documents to English file names |
| 2026-09-27 14:23 | `a3282d6` | `8c066a7` | docs(p2): Addendum 5 on the history rewrite, English data labels, guards |
| 2026-09-27 14:23 | `ce7c843` | `124f0e9` | chore(p2): English names for the semantics result files |
| 2026-09-29 18:33 | `c5bad21` | `f36477d` | docs(p2): layout and readability pass, readable Figure 7 panel b |
| 2026-09-30 12:42 | `1b46e80` | `6f2436e` | prereg(p2): Addendum 6, fee of multi-leg RFQ packages (audit A67) |
| 2026-09-30 12:51 | `01700df` | `0ee0ac0` | fix(p2): number check binds direction words; manuscript and audit status of 30 September (audit B2) |
| 2026-09-30 12:51 | `4659457` | `dab0ac9` | docs: DOI for burlig2018 in both bibliographies (audit A61) |
| 2026-09-30 12:51 | `4f132c1` | `5474f8e` | feat(p2): H2 by maker side, H1 without RFQ fills, RFQ package sensitivity (audit A10, A12, A67) |
| 2026-09-30 12:51 | `9660659` | `f067389` | fix(p2): placebo cache knows panel rules and code, run has --out, --parts-dir, --fresh (audit A31, A32) |
| 2026-09-30 12:51 | `ef23ab0` | `fc74275` | fix(p2): H4 card hatches up to the placebo P95; figure docs updated (audit B3, B7) |
| 2026-10-05 09:03 | `4e36d9e` | `6e011fb` | results(p2): Addendum 6 sensitivity on the pilot cut (H1 with the RFQ package fee spread over its legs) |
| 2026-10-05 12:24 | `340502d` | `f9ed321` | docs(p2): reader audit of the PDF, round 1 (65 of 71 findings fixed, 6 in part) |
| 2026-10-05 13:02 | `1e23993` | `ee4a3bb` | docs(p2): reader audit of the PDF, round 2 |
| 2026-10-05 13:28 | `9052fae` | `32d2ba5` | docs(p2): reader audit of the PDF, round 3 (final review) |
| 2026-10-05 13:34 | `18ecdbb` | `ad897e4` | docs(p2): SSRN number and DOI of the companion paper (reader audit B5) |

## Rewrite of 25 October 2026

Date: 25 October 2026. The third rewrite removes from every commit the working documents that were produced while
the tools named under "Use of generative tools" in both papers and under "Use of AI tools" in the README were used,
together with tool logs and operating-system files, and gives every commit an English message in the Conventional
Commits format. It changes no file that it keeps, no rule of either pre-registration, no test statistic and no result.

1. **Files.** 71 paths are removed from every commit:
   - the plans and design specifications of the planning tool: `docs/superpowers/plans/` (13 files) and
     `docs/superpowers/specs/` (3 files);
   - session handovers and status reports: `docs/paper2/UEBERGABE.md`, `docs/paper2/HANDOVER.md`,
     `docs/paper2/stand/` and `docs/paper2/status/` (7 files each), `docs/paper2/BERICHT_2026-09-25.md`,
     `docs/paper2/REPORT_2026-09-25.md`, `docs/paper2/HISTORIE_BEREINIGEN.md`, `docs/paper2/HISTORY_CLEANUP.md`;
   - drafts, jury and referee texts: `docs/paper2/drafts/` (6 files) and `docs/paper2/entwuerfe/` (5 files); the
     figure-selection notes `docs/paper1/ABBILDUNGSWAHL.md`, `docs/paper1/FIGURE_SELECTION.md`,
     `docs/paper2/ABBILDUNGSWAHL.md`, `docs/paper2/FIGURE_SELECTION.md`; the manuscript outlines
     `docs/paper1/MANUSKRIPT.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/MANUSKRIPT.md`,
     `docs/paper2/MANUSCRIPT.md`; the draft article for X `paper/social/x_article.md`;
   - tool logs and operating-system files: `logs/` (12 files), `.DS_Store`, `paper/.DS_Store`,
     `paper/social/.Rhistory`.

   None of them is named by file in a pre-registration. Every file that stays keeps its git blob in every commit,
   so both pre-registrations are byte-identical in every commit (table below); historical versions of the files
   that stay are unchanged as well and may still name removed working documents. Registration commits that lost
   files with this rewrite:
   - Paper 1 pre-registration (`cf1f432`, now `5bddae9`): `docs/superpowers/plans/2026-09-17-p1-datenbasis.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`
   - Paper 1 Addendum 1 (`143f695`, now `3c5449e`): `docs/superpowers/plans/2026-09-17-p1-datenbasis.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`
   - Paper 1 Addendum 2 (`634c276`, now `54450f3`): `docs/superpowers/plans/2026-09-17-p1-datenbasis.md`, `docs/superpowers/plans/2026-09-17-p1-inferenz.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`
   - Paper 1 Addendum 3 (`e4a3873`, now `4845c98`): `.DS_Store`, `docs/paper1/ABBILDUNGSWAHL.md`, `docs/paper1/MANUSKRIPT.md`, `docs/paper2/UEBERGABE.md`, `docs/superpowers/plans/2026-09-17-p1-datenbasis.md`, `docs/superpowers/plans/2026-09-17-p1-inferenz.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-abbildungen.md`, `docs/superpowers/plans/2026-09-18-p1-manuskript.md`, `docs/superpowers/plans/2026-09-25-p1-korrektur.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 1 Addendum 4 (`9d37c19`, now `2e569f2`): `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/FIGURE_SELECTION.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/HISTORY_CLEANUP.md`, `docs/paper2/MANUSCRIPT.md`, `docs/paper2/REPORT_2026-09-25.md`, `docs/paper2/drafts/empirics.md`, `docs/paper2/drafts/jury_design.md`, `docs/paper2/drafts/jury_referee.md`, `docs/paper2/drafts/mechanism.md`, `docs/paper2/drafts/practitioner.md`, `docs/paper2/status/B0.md`, `docs/paper2/status/B1.md`, `docs/paper2/status/B2.md`, `docs/paper2/status/B3.md`, `docs/paper2/status/B4.md`, `docs/paper2/status/B5.md`, `docs/paper2/status/C5b.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-master-plan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-capital-design.md`, `paper/social/x_article.md`
   - Paper 2 pre-registration (`c9e9162`, now `4600d5d`): `.DS_Store`, `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/HANDOVER.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-kapital-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 2 Addendum 1 (`b5c905c`, now `241422f`): `.DS_Store`, `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/MANUSKRIPT.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-gesamtplan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-kapital-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 2 Addendum 2 (`60896fe`, now `b6dbde9`): `.DS_Store`, `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/MANUSKRIPT.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-gesamtplan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-kapital-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 2 Addendum 3 (`2416902`, now `4a33d66`): `.DS_Store`, `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/MANUSKRIPT.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-gesamtplan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-kapital-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 2 Addendum 4 (`a02058a`, now `33108ad`): `.DS_Store`, `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/MANUSKRIPT.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-gesamtplan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-kapital-design.md`, `paper/.DS_Store`, `paper/social/.Rhistory`, `paper/social/x_article.md`
   - Paper 2 Addendum 5 (`8c066a7`, now `2a6a0c2`): `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/FIGURE_SELECTION.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/HISTORY_CLEANUP.md`, `docs/paper2/MANUSCRIPT.md`, `docs/paper2/REPORT_2026-09-25.md`, `docs/paper2/drafts/empirics.md`, `docs/paper2/drafts/jury_design.md`, `docs/paper2/drafts/jury_referee.md`, `docs/paper2/drafts/mechanism.md`, `docs/paper2/drafts/practitioner.md`, `docs/paper2/status/B0.md`, `docs/paper2/status/B1.md`, `docs/paper2/status/B2.md`, `docs/paper2/status/B3.md`, `docs/paper2/status/B4.md`, `docs/paper2/status/B5.md`, `docs/paper2/status/C5b.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-master-plan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-capital-design.md`, `paper/social/x_article.md`
   - Paper 2 Addendum 6 (`6f2436e`, now `1d1abb0`): `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/FIGURE_SELECTION.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/HISTORY_CLEANUP.md`, `docs/paper2/MANUSCRIPT.md`, `docs/paper2/REPORT_2026-09-25.md`, `docs/paper2/drafts/empirics.md`, `docs/paper2/drafts/jury_design.md`, `docs/paper2/drafts/jury_referee.md`, `docs/paper2/drafts/mechanism.md`, `docs/paper2/drafts/practitioner.md`, `docs/paper2/status/B0.md`, `docs/paper2/status/B1.md`, `docs/paper2/status/B2.md`, `docs/paper2/status/B3.md`, `docs/paper2/status/B4.md`, `docs/paper2/status/B5.md`, `docs/paper2/status/C5b.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-master-plan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-capital-design.md`, `paper/social/x_article.md`
   - Paper 2 Addendum 7 (`7c01e85`, now `8419479`): `docs/paper1/FIGURE_SELECTION.md`, `docs/paper1/MANUSCRIPT.md`, `docs/paper2/FIGURE_SELECTION.md`, `docs/paper2/HANDOVER.md`, `docs/paper2/HISTORY_CLEANUP.md`, `docs/paper2/MANUSCRIPT.md`, `docs/paper2/REPORT_2026-09-25.md`, `docs/paper2/drafts/empirics.md`, `docs/paper2/drafts/jury_design.md`, `docs/paper2/drafts/jury_referee.md`, `docs/paper2/drafts/mechanism.md`, `docs/paper2/drafts/practitioner.md`, `docs/paper2/status/B0.md`, `docs/paper2/status/B1.md`, `docs/paper2/status/B2.md`, `docs/paper2/status/B3.md`, `docs/paper2/status/B4.md`, `docs/paper2/status/B5.md`, `docs/paper2/status/C5b.md`, `docs/superpowers/plans/2026-09-17-p1-data.md`, `docs/superpowers/plans/2026-09-17-p1-inference.md`, `docs/superpowers/plans/2026-09-17-p1-markouts.md`, `docs/superpowers/plans/2026-09-18-p1-figures.md`, `docs/superpowers/plans/2026-09-18-p1-manuscript.md`, `docs/superpowers/plans/2026-09-24-p2-master-plan.md`, `docs/superpowers/plans/2026-09-25-p1-correction.md`, `docs/superpowers/specs/2026-09-17-adverse-selection-design.md`, `docs/superpowers/specs/2026-09-24-p2-capital-design.md`, `paper/social/x_article.md`

   The tree of `bd326b9`, the last rewritten commit, is identical to that of `b93362b`, the last commit before this
   rewrite; the restructuring of 25 October 2026 (`src/`, `papers/`) follows in separate commits.
2. **Commits.** The seven merge commits are dropped; each had the tree of its second parent, so the history is now
   one line of commits with the same snapshots. Seven commits changed only removed files and are dropped. Six commits
   are folded into the commit that follows them (two parts of one README update, two parts of one re-render of the
   animations, a figure commit and its revision, the social cards and their fix, and three successive passes over the
   Paper 2 PDF); no registration commit is folded, and none is folded into another. 102 commits remain.
3. **Messages.** Every message is rewritten in English in the Conventional Commits format (`type(scope): subject`).
4. **Times.** Author, committer, date, time of day and time zone of every remaining commit are kept. A folded commit
   takes the time of the commit it is folded into, which is never earlier than its own.
5. **Refs.** `main` is rewritten, and no other branch carries the old history. The pull requests #1 to #5 on GitHub
   keep their original commits on their pages; only GitHub can remove those.

Done with a script that rebuilds every commit from the snapshot of the original, without the removed paths. The
state before is kept as a git bundle outside the repository (SHA-256 `571f6302c1d4606f13de51230097b0b7c02ff0af28f4d904449aa732e1f39fb6`). Checks run on
the result: one root, no merge commit; no removed path in any commit; for every remaining commit the tree equals the
original tree without the removed paths, and author, committer and their dates are equal; the blob of the
pre-registration file in each registration commit equals the original; the test suite gives the same result before
and after, also in a fresh clone without the old objects.

The manuscripts and PDFs in `papers/` (versions of 6 October 2026), the social cards and the dated records under
`docs/` cite the hashes before this rewrite; the tables map them. A rebuild of a manuscript resolves its `% src git:`
declarations against the current history, so the hashes in the manuscripts have to be updated before the next build.

### Pre-registration commits (hash before 25 October 2026, new hash, blob and SHA-256 of the pre-registration file)

| Version | Author time | Before | New | Blob (identical before and after) | SHA-256 |
|---|---|---|---|---|---|
| Paper 1 pre-registration | 2026-09-17 15:47 +0200 | `cf1f432` | `5bddae9` | `0376d559bba2fb626377cffc4b85b01ef34a0bc0` | `d233186e8a376998…` |
| Paper 1 Addendum 1 | 2026-09-17 16:43 +0200 | `143f695` | `3c5449e` | `5103b6edb1109cac08e863bdaf624df7cf595c99` | `4503883512ab264b…` |
| Paper 1 Addendum 2 | 2026-09-18 00:02 +0200 | `634c276` | `54450f3` | `fc02ab9959756dec32bcfff326881861c795f2f3` | `5e3a3af72340e154…` |
| Paper 1 Addendum 3 | 2026-09-25 09:10 +0200 | `e4a3873` | `4845c98` | `6da6185be44a127180d6a9173b2e5fc921aed783` | `c3d17de9716ae910…` |
| Paper 1 Addendum 4 | 2026-10-05 23:51 +0200 | `9d37c19` | `2e569f2` | `b6d5a86bd06c74dd5ee4867a43294a44dabec254` | `f218c174a383554d…` |
| Paper 2 pre-registration | 2026-09-24 22:33 +0200 | `c9e9162` | `4600d5d` | `f251a0c5493e81a9dca98b1aecab54d2bde4349b` | `d777206a923e8eca…` |
| Paper 2 Addendum 1 | 2026-09-25 00:26 +0200 | `b5c905c` | `241422f` | `83693818948078d9f308e1083541056cd2924ac4` | `7f5f701d014d8fd5…` |
| Paper 2 Addendum 2 | 2026-09-25 00:29 +0200 | `60896fe` | `b6dbde9` | `461a9af11090c853e2bbf0495c8f1b42c760f434` | `c2e6d5334c85d7de…` |
| Paper 2 Addendum 3 | 2026-09-25 00:29 +0200 | `2416902` | `4a33d66` | `55ea5cc2868ae7244fa693af5718171aef78673f` | `536a39cbac9a762d…` |
| Paper 2 Addendum 4 | 2026-09-25 01:29 +0200 | `a02058a` | `33108ad` | `96986c732c85408daa4e704c519601bb531fdc4b` | `eb5e269571df785c…` |
| Paper 2 Addendum 5 | 2026-09-27 14:23 +0200 | `8c066a7` | `2a6a0c2` | `c2d7378bfa0d51ac63d819ec9ded8571535ab4a6` | `01203c1814a9f20f…` |
| Paper 2 Addendum 6 | 2026-09-30 12:42 +0200 | `6f2436e` | `1d1abb0` | `06a0004cda02936baf42db3101f2bd24e50f54a8` | `0febb8e4d1a20ed6…` |
| Paper 2 Addendum 7 | 2026-10-05 23:51 +0200 | `7c01e85` | `8419479` | `f492585cf4a3308154ba764ef4616e09d3d68621` | `0931cfaa3995f7eb…` |

### Every commit (author date, hash before 25 October 2026, new hash, new message)

| Date | Before | New | Message |
|---|---|---|---|
| 2026-09-03 09:18 | `6beafe7` | `ca139f9` | chore: initial commit with a README title |
| 2026-09-03 08:04 | `9d9dbe6` | `d1510dd` | feat(surface): add data pipeline, Black-76/SVI surface and animations |
| 2026-09-03 10:19 | `f21e900` | – | *merge dropped; same tree as `9d9dbe6`, now `d1510dd`* |
| 2026-09-03 12:04 | `e4741ca` | `0e6bac0` | feat(surface): add live recording and depth snapshot, harden SVI fits |
| 2026-09-03 12:20 | `2ee365f` | `35c0e57` | *folded into the next commit* |
| 2026-09-03 12:21 | `35d19d6` | `35c0e57` | docs(surface): fill the README with measured basis, greek noise and SSR |
| 2026-09-03 14:34 | `6759512` | – | *merge dropped; same tree as `35d19d6`, now `35c0e57`* |
| 2026-09-03 12:40 | `8e356d4` | `49a648c` | docs(surface): translate README to English, publish BTC/ETH animations |
| 2026-09-03 15:02 | `ab70741` | – | *merge dropped; same tree as `8e356d4`, now `49a648c`* |
| 2026-09-03 13:07 | `b594256` | `e69c392` | *folded into the next commit* |
| 2026-09-03 13:07 | `229d5a6` | `e69c392` | chore(surface): re-render all animations with English frame labels |
| 2026-09-03 13:08 | `fd365c2` | `6432173` | chore(surface): remove the unreferenced German-labelled vanna still |
| 2026-09-03 15:09 | `4699368` | – | *merge dropped; same tree as `fd365c2`, now `6432173`* |
| 2026-09-17 15:47 | `cf1f432` | `5bddae9` | docs(p1): pre-register Paper 1 hypotheses before any markout is computed |
| 2026-09-17 15:47 | `0829308` | `5950026` | feat(p1): fetch the full-field option tape in single-page windows |
| 2026-09-17 15:47 | `ae9ba56` | `7db6419` | feat(p1): fetch the vol-feed (SVI) history from Derive Chain |
| 2026-09-17 15:48 | `8da0424` | `48423a0` | feat(p1): download settlements, liquidations, programmes and fees |
| 2026-09-17 15:48 | `4ee1cab` | `b31c7c1` | feat(p1): pair maker and taker rows into fills and classify takers |
| 2026-09-17 15:48 | `e7a6868` | `06bb47e` | feat(p1): add the p1 CLI and a vol-feed check script |
| 2026-09-17 15:53 | `fe5c541` | `df32262` | fix: retry unparseable API replies, walk liquidations in 7-day windows |
| 2026-09-17 15:58 | `aa543d3` | `d4e85d4` | fix(p1): walk liquidations daily with page-size fallback and halving |
| 2026-09-17 16:07 | `5b67761` | `e644c97` | fix(p1): harden tape caching, MM-programme lookup and feed timestamps |
| 2026-09-17 16:11 | `ea99f98` | – | *removed: it changed only removed files* |
| 2026-09-17 16:23 | `9d85cbc` | `3ca5fd4` | feat(p1): add a curated vault wallet list with sources |
| 2026-09-17 16:24 | `efe2481` | `d32e257` | fix(p1): raise the tape settle window to 60 min |
| 2026-09-17 16:26 | `be6c037` | `2cef120` | feat(p1): add a script that writes the data status from data/p1 |
| 2026-09-17 16:28 | `c7666bd` | `0b67fdc` | feat(p1): add mark path (b), Black-76 on the latest on-chain SVI curve |
| 2026-09-17 16:43 | `143f695` | `3c5449e` | docs(p1): add pre-registration addendum 1 before any markout is computed |
| 2026-09-17 16:43 | `b36e511` | `0d1e487` | feat(p1): compute markouts on mark paths a, b and c in three units |
| 2026-09-17 16:43 | `e0fea45` | `86684fc` | feat(p1): add the p1 markouts command |
| 2026-09-17 16:51 | `3663ad2` | `fa4c350` | feat(p1): verify svi_vol on the freshest signed curve, measure push lag |
| 2026-09-17 17:13 | `ab59856` | `d5079e5` | feat(p1): compare rebuilt mark and tape by clock, forward and quarter |
| 2026-09-17 18:19 | `114d6d9` | `3b6341a` | perf(p1): fetch liquidation windows in parallel with progress logging |
| 2026-09-17 18:43 | `234fe8d` | `138ce20` | fix(p1): read liquidations at the largest page size that works |
| 2026-09-17 22:07 | `b903a0b` | `327869a` | feat(p1): reuse liquidation files in ref, survive maker score give-ups |
| 2026-09-17 22:13 | `61e04ce` | `7715256` | feat(p1): report liquidation coverage in the pilot data status |
| 2026-09-18 00:02 | `634c276` | `54450f3` | docs(p1): add pre-registration addendum 2 before the first inference |
| 2026-09-18 00:03 | `f945ec7` | `f2626cd` | feat(p1): add the inference for H1 to H4 and the net edge per cell |
| 2026-09-18 00:06 | `fb108c6` | `120b896` | feat(p1): add the p1 inference command and the pilot results |
| 2026-09-18 00:09 | `04b5221` | `c209ee2` | feat(p1): split class markouts into half spread and adverse selection |
| 2026-09-18 08:34 | `c6bdb11` | `cca099c` | feat(p1): add figure style with CAS widths and the Okabe-Ito palette |
| 2026-09-18 08:35 | `34d13d5` | `5acaf98` | docs(p1): add a CAS manuscript skeleton that builds with tectonic |
| 2026-09-18 08:37 | `c1d1129` | `36ff78d` | feat(p1): compare the two mark paths as a robustness check |
| 2026-09-18 08:38 | `0a889c8` | `1fa4cd8` | feat(p1): add the data aggregations behind the figures |
| 2026-09-18 08:39 | `c7bf089` | `ae5a8a4` | perf(p1): bootstrap the median as a weighted median over one sort |
| 2026-09-18 09:07 | `e17065a` | `29ad81f` | chore(p1): add the mark-path comparison to the pilot results |
| 2026-09-18 09:09 | `a7286e2` | `8ab9f57` | feat(p1): reconstruct the quoted SVI curve for the mechanism figure |
| 2026-09-18 09:10 | `ac0df45` | `93f21eb` | feat(p1): add a result loader and a cell matrix for the figures |
| 2026-09-18 09:11 | `df8f628` | `f5ea428` | docs(p1): record findings from building the figures |
| 2026-09-18 09:15 | `cb02f29` | `ceacb86` | fix(p1): use one bootstrap size for every number of an inference run |
| 2026-09-18 09:17 | `a408bfe` | `fed8ebf` | chore(p1): recompute the pilot inference with one bootstrap size |
| 2026-09-18 09:20 | `1d59cbd` | – | *removed: it changed only removed files* |
| 2026-09-18 09:23 | `76ab684` | – | *removed: it changed only removed files* |
| 2026-09-18 09:34 | `8b0af87` | `ebcae00` | *folded into the next commit* |
| 2026-09-18 09:44 | `3dffd82` | `ebcae00` | feat(p1): draw the nine figures and add the p1 figures command |
| 2026-09-18 09:48 | `223dc94` | `45b48d8` | feat(p1): place the figures in the manuscript and check their numbers |
| 2026-09-18 09:48 | `7437272` | – | *removed: it changed only removed files* |
| 2026-09-18 09:49 | `c4c633b` | – | *removed: it changed only removed files* |
| 2026-09-18 11:30 | `3d62bea` | `d5dc716` | docs(p1): write the Paper 1 manuscript under a word budget |
| 2026-09-18 12:21 | `48b4377` | `1bcd831` | *folded into the next commit* |
| 2026-09-18 12:22 | `071383c` | `1bcd831` | feat(p1): add social media cards for Paper 1 |
| 2026-09-19 15:45 | `2139e62` | `568efab` | docs(p1): add SSRN statements and a check for the manuscript build |
| 2026-09-24 12:42 | `de3291e` | `909a547` | docs(p1): add the PDF of the first Paper 1 version of 19 September |
| 2026-09-25 08:04 | `fd5abd6` | `871c1ee` | docs(p1): record the fee unit error in the Paper 1 net edge |
| 2026-09-25 09:10 | `e4a3873` | `4845c98` | fix(p1): add pre-registration addendum 3 and correct the fee unit |
| 2026-09-25 10:49 | `4b4b8a4` | `fed6f81` | fix(p1): store wallets in the Lorenz file only as salted pseudonyms |
| 2026-09-25 12:03 | `356a1f9` | – | *merge dropped; same tree as `4b4b8a4`, now `fed6f81`* |
| 2026-09-25 12:37 | `9c3828c` | `8b853e5` | docs(p1): translate the Paper 1 documentation and generators to English |
| 2026-09-25 12:37 | `42b529f` | – | *merge dropped; same tree as `9c3828c`, now `8b853e5`* |
| 2026-09-24 22:29 | `6642ff8` | `dba6c27` | docs(p2): document the get_margin semantics and the capital measure |
| 2026-09-24 22:33 | `c9e9162` | `4600d5d` | docs(p2): pre-register H1 to H4 before the first capital figure |
| 2026-09-24 22:37 | `cc714b8` | – | *removed: it changed only removed files* |
| 2026-09-24 22:38 | `2748b13` | `da1cc87` | feat(p2): add a throttled chain client with a block clock |
| 2026-09-24 22:59 | `55629ff` | `85f3ba1` | docs(p2): add the checked literature and the manuscript skeleton |
| 2026-09-25 00:26 | `c8ab517` | `e737d8e` | feat(p2): add feed history, parameter timelines and margin replicas |
| 2026-09-25 00:26 | `b5c905c` | `241422f` | docs(p2): add pre-registration addendum 1 before any capital figure |
| 2026-09-25 00:27 | `b8c676a` | `9382925` | chore(p2): add a machine-wide lock for memory-heavy jobs |
| 2026-09-25 00:29 | `60896fe` | `b6dbde9` | docs(p2): add pre-registration addendum 2 on fee and rebate per fill |
| 2026-09-25 00:29 | `2416902` | `4a33d66` | docs(p2): add pre-registration addendum 3 before the inference |
| 2026-09-25 01:29 | `a02058a` | `33108ad` | docs(p2): add pre-registration addendum 4 before any test statistic |
| 2026-09-25 01:30 | `f97a117` | `7fb900d` | feat(p2): validate the replica and compute capital per fill |
| 2026-09-25 02:16 | `c55d47b` | `376499c` | feat(p2): add the H1 to H4 inference with an independent recomputation |
| 2026-09-25 02:49 | `743d403` | `28ef407` | feat(p2): add the H2 maintenance margin sensitivity and an API snapshot |
| 2026-09-25 03:49 | `74a89c5` | `06ed5d6` | feat(p2): add nine figures, the capital surface GIF and social cards |
| 2026-09-25 04:05 | `5e256d8` | `93e932e` | feat(p2): write the manuscript with build, number and figure checks |
| 2026-09-25 05:02 | `89ab2d4` | `60a75ab` | docs(p2): add an internal audit of the whole paper (69 findings) |
| 2026-09-25 07:40 | `afb3006` | `ee7d609` | fix(p2): correct the audit findings in code, inference, figures and text |
| 2026-09-25 07:41 | `e8b5b2c` | – | *removed: it changed only removed files* |
| 2026-09-27 12:31 | `c5c36ba` | `f6a3dcd` | fix(p2): keep Paper 2 working on the revised Paper 1 code |
| 2026-09-27 12:34 | `b36daec` | `67f4d06` | docs(p2): record the rewrite of the Paper 2 history |
| 2026-09-27 13:38 | `61bf971` | `dfa6d63` | docs(p2): rename the Paper 2 documents and generators to English names |
| 2026-09-27 13:38 | `2ce6d18` | `2268ef5` | docs(p2): translate the Paper 2 documentation and generators to English |
| 2026-09-27 14:23 | `124f0e9` | `246ea6e` | chore(p2): rename the get_margin semantics result files to English |
| 2026-09-27 14:23 | `8c066a7` | `2a6a0c2` | docs(p2): add pre-registration addendum 5 recording the history rewrite |
| 2026-09-29 18:33 | `f36477d` | `0edd777` | docs(p2): simplify the manuscript text and declutter Figure 7 panel b |
| 2026-09-30 12:42 | `6f2436e` | `1d1abb0` | docs(p2): add pre-registration addendum 6 on multi-leg RFQ package fees |
| 2026-09-30 12:51 | `5474f8e` | `be16567` | feat(p2): report H2 by maker side and H1 without RFQ fills |
| 2026-09-30 12:51 | `f067389` | `5a95fed` | fix(p2): tie the H4 placebo cache to panel rules and code |
| 2026-09-30 12:51 | `fc74275` | `a1e34a1` | fix(p2): hatch the H4 social card up to the placebo P95 |
| 2026-09-30 12:51 | `dab0ac9` | `1e1ee18` | docs(p1,p2): add the DOI of burlig2018 to both bibliographies |
| 2026-09-30 12:51 | `0ee0ac0` | `ec12edd` | fix(p2): make the number check bind direction words |
| 2026-10-05 09:03 | `6e011fb` | `a17e522` | feat(p2): compute the Addendum 6 sensitivity of H1 on the pilot cut |
| 2026-10-05 12:24 | `f9ed321` | `6abae37` | *folded into the next commit* |
| 2026-10-05 13:02 | `ee4a3bb` | `6abae37` | *folded into the next commit* |
| 2026-10-05 13:28 | `32d2ba5` | `6abae37` | docs(p2): revise the Paper 2 text, figures and layout page by page |
| 2026-10-05 13:34 | `ad897e4` | `1b90d64` | docs(p2): cite the companion paper by its SSRN number and DOI |
| 2026-10-05 23:51 | `7c01e85` | `8419479` | docs(p2): add pre-registration addendum 7 on the second history rewrite |
| 2026-10-05 23:51 | `9d37c19` | `2e569f2` | docs(p1): add pre-registration addendum 4 on the history rewrite |
| 2026-10-06 00:00 | `d616f95` | `bbe5ba3` | docs(p1,p2): cite the rewritten commit hashes in both papers |
| 2026-10-06 00:00 | `9ad4bbf` | – | *merge dropped; same tree as `d616f95`, now `bbe5ba3`* |
| 2026-10-06 08:42 | `c90dfb8` | `c2eba43` | docs: show both papers in the README after the surface animations |
| 2026-10-06 09:24 | `878e4d1` | `093f9ee` | docs(p1,p2): drop the "Use of generative tools" section from both papers |
| 2026-10-06 09:26 | `22902e7` | `9410a68` | docs(p1,p2): cite the repository as gelatotrade/Derive-Research |
| 2026-10-06 10:05 | `9d0baa6` | `901aee5` | docs: date the Paper 1 revision 6 October 2026 in the README |
| 2026-10-06 10:21 | `968fb88` | `e5da6f6` | chore: keep only code, papers, data and pre-registrations in the tree |
| 2026-10-06 12:10 | `a8e119e` | `feb122a` | feat(p1): run Paper 1 on the registered sample to 30 September 2026 |
| 2026-10-06 12:27 | `70e7c0b` | `34c3f11` | feat(p2): run Paper 2 on the registered sample to 30 September 2026 |
| 2026-10-06 13:07 | `f089ff7` | `a58d7d8` | fix(p1,p2): follow the registered rules in the final data run |
| 2026-10-06 13:54 | `bfcc386` | `8a0d8d1` | docs(p1,p2): polish layout, wording and figure labels of both papers |
| 2026-10-06 15:14 | `5af1341` | `34da1cc` | fix(p1,p2): correct how both papers describe the registered analysis |
| 2026-10-06 15:33 | `be775df` | `11f5283` | fix(p1): state the coefficient rule as registered |
| 2026-10-08 16:00 | `b93362b` | `bd326b9` | docs: add a statement on the use of AI tools to the papers and README |
