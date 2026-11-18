---
description: Read the newest file on the Desktop (fresh listing, not from memory)
---

Read and analyze the newest file on the Desktop. The listing below was taken the moment this command ran — it is the only truth; never reuse a Desktop path from earlier in the session (numbered copies like "something 8.png" and glob failures have made stale paths grab older files):

!`ls -lt ~/Desktop | head -5`

Read the FIRST entry (most recently modified). If it is an image, view and analyze it; if $ARGUMENTS adds a question or focus, answer it about that file.

If the read fails, the file is a 0-byte stub (visible in the listing above), or the image comes back blank/gray/wrong-looking, it may be an iCloud placeholder that needs a forced download: run `brctl download <path>` and read the file again.