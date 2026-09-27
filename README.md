# The fmIDE community library

Templates, recipes and functions that people share for [fmIDE](https://github.com/taroyamakatrmttm-star/fmide), the visual builder for financial models. Each one is a **library pack**: a single file that fmIDE writes (**File → Save as Library Pack…**) and reads (**File → Open Library Pack…**), with a preview of what it holds before anything is added.

> **The catalogue:** browse the approved packs at **https://fmide.pages.dev/library/**. Each pack has a page listing what it holds, a download, and the credit its licence asks for.

Everything here is formulas and layout, never code: fmIDE reads a pack with its own parser and never runs anything in it. Macros are not shared.

## Using a pack

1. Find the pack in the [catalogue](https://fmide.pages.dev/library/) and download it from its page (or download the file from `packs/` here; each is named `<pack id>.fmide-pack.json`).
2. In fmIDE: **File → Open Library Pack…**, choose the file, look at the preview, untick anything you don't want, and click **Add to My Library**.

fmIDE shows where each item came from, including its author and pack, in the Templates window and the Functions manager. That line is the credit CC BY 4.0 asks for.

## Sharing a pack

1. In fmIDE, **File → Save as Library Pack…**: give it a title, your author name (always the same one), a description and tags, and tick what to share. A recipe takes its parts along, and a function takes the functions it calls.
2. Read the [submission terms](SUBMITTING.md). By submitting, you agree to them and license your items under CC BY 4.0.
3. Open a pull request that adds your file to `packs/`, **named after its pack id**: `packs/<pack id>.fmide-pack.json`. Tick the box in the pull-request template.
4. The automatic check posts a report on the pull request. If it lists **records to add**, add those entries to `families.json`, `authors.json` and `packs.json` exactly as shown (or leave them for the maintainer to add).
5. The maintainer reviews every pack and merges it. Once merged, a pack is **never edited**: to share a new version, save a new pack.

## The rules

The check enforces them. It is fmIDE's own pack checker, `tools/check-pack.js --library`, at the fmIDE commit named in `checker.json`.

- **A pack is at most 5 MB**, saved by fmIDE and unchanged. Anything fmIDE would quietly leave out or tidy is an error, and so are characters that hide or reverse text.
- **Only CC BY 4.0.** Anyone may use, change and share the items, commercially too, with credit to the author.
- **A family belongs to its first author.** A template or function family belongs to the GitHub account that first shared it (recorded in `families.json`), and only that account adds new versions to it. This stops a stranger publishing a "version 4" of your template that every canvas made from it would then offer as an update.
- **Sharing someone else's item again:** only an exact copy of a version approved in their pack, carrying the record of where it came from (fmIDE keeps that record when you share it again). Your own changes to someone else's item go in as a new template or function of your own.
- **One author name per account.** The author name in your packs must be the one recorded for your GitHub account in `authors.json`, and no two accounts use the same name.
- **Ids are never used twice.** A pack id or version id stays taken, even after a takedown. fmIDE gives new ids every time you save.
- **Records are only ever added.** `families.json`, `authors.json` and `packs.json` are never changed or cut down by a submission. A change of a family's owner is a separate pull request by the maintainer.

## The records

| File | What it holds |
|---|---|
| `families.json` | Each template or function family: its type, the GitHub account that owns it (by numeric id, with the login beside it), its first pack and date |
| `authors.json` | Each GitHub account's author name |
| `packs.json` | Each approved pack: its account, date, SHA-256 hash and the version ids it holds. The record stays when a pack is taken down. |
| `checker.json` | The fmIDE commit whose checker checks this library, and the maintainers |

Their exact format is in fmIDE's [`docs/file-formats.md`](https://github.com/taroyamakatrmttm-star/fmide/blob/main/docs/file-formats.md) ("The community library's records").

## Reporting an item, and takedowns

If a pack holds something that isn't its author's to share, credits the wrong person, or is harmful or broken, open an issue with the **Report an item** form. Each pack's page in the catalogue has a **Report this pack** link that opens the form with the pack id filled in.

A takedown removes the pack file from the library and from the catalogue. Its records stay, so its ids are never used again, and family ownership doesn't change. Copies people have already downloaded stay theirs, under CC BY 4.0.

## For the maintainer

- **Approving a submission:** read the check's comment. Its summary lists the account, the new pack and the families it claims. Look at the pack's content, then merge.
- **Adding records yourself:** in a checkout of fmIDE at the commit in `checker.json`, run `node tools/check-pack.js --library PATH/TO/fmide-library --write-records --account LOGIN --account-id ID` with the submitter's login and numeric id (shown in the check's comment), then commit the three record files to the pull request.
- **A takedown:** a pull request of its own that only deletes the pack file. It leaves the catalogue when fmIDE's pointer is moved past it (below).
- **Publishing to the catalogue:** the catalogue is built by fmIDE from this repository, which fmIDE holds as a git submodule (`library/`) pinned to one commit. After merging here, in fmIDE run `git submodule update --remote library` and open a pull request with that one change. fmIDE's build checks every pack again (nothing is published if one fails); the pull request's preview shows the new catalogue, and merging it updates the live site.
- **Moving to a newer checker:** a pull request that changes `fmide.commit` in `checker.json` to a newer fmIDE commit. It takes effect once merged, because checks always read `checker.json` from `main`.
- **Access to fmIDE:** while fmIDE is private, the check reads it with the deploy key stored in this repository's secret `FMIDE_DEPLOY_KEY`.

## Licences

The packs are licensed by their authors under [CC BY 4.0](LICENSE-CC-BY-4.0.txt). This repository's own files (this README, the templates, the workflow) are under the [Apache License 2.0](LICENSE-APACHE-2.0.txt). See [LICENSING.md](LICENSING.md).
