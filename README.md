# KAY9 record

Every scan batch document and scan report the KAY9 watchdog has committed on Robinhood Chain
(chain id 4663), byte for byte. The files are served at <https://record.kay9.io>.

Nothing here needs to be trusted. Each file is checked against a value on chain:

- `batches/<root>.json` is a scan batch document. `root` is the Merkle root that
  `KAY9ScanRegistry` (`0x79778723c021386F3C7727289A30716edaa635A1`) recorded in
  `commitScanBatch`. Rebuild the tree from the document's entries and compare it with the root
  on chain.
- `reports/<reportHash>.json` is a full scan report. Its keccak256 is `reportHash`, the value the
  batch entry commits to.

A file that fails either check is not part of the record, wherever it came from.

## Where the files came from

From 2026-09-25 to 2026-09-27 the documents were pinned on IPFS through Pinata, and the batch
entries of that period point at `ipfs://` addresses. Those pins stay in place. This repository
holds the same bytes, and `ipfs-mirror.json` maps each of those IPFS addresses to its file here.

From 2026-09-27 new documents are published here, at `https://record.kay9.io/...`, because the
free pinning plan allows 500 files in total and the watchdog writes about 200 a day.

## Known gaps

The documents of batches 27 and 28 (committed on 2026-09-27 at blocks 73,722,772 and 73,792,926)
were lost when the pinning service stopped accepting files: the scanner fell back to its own disk,
which does not outlive a run. Their roots are on chain; their contents cannot be opened.

## Copying the record

`git clone https://github.com/kay9T/kay9-record` gives you all of it. The procedure for rebuilding
the watchdog's record from the chain and these files is in
[`docs/REBUILD_THE_RECORD.md`](https://github.com/kay9T/kay9-protocol/blob/main/docs/REBUILD_THE_RECORD.md).
