# 🔐 For Auditors: Verifying Artifact Integrity, Authenticity, and Release Date
**(SHA‑256 → GPG Signature → OTS Anchor — a Cryptographic Combo)**

Public key in the **results** repo: [`keys/emm_pub_key.asc`](https://github.com/euro-macromechanica-backtest/results/tree/main/keys/emm_pub_key.asc) (ASCII‑armored)  
**GPG key fingerprint (rleydev — thelaziestcat):** `4E53 DA03 D1A1 644E 6479  3C47 EEEC 0AFA 4D8A F0A7`

---

A neutral, detailed, and clear description of **what exactly is verified**, **why three protection layers are used**, and **how to verify on macOS, Linux, and Windows**.

---

## 1) 🧱 The Basics: What SHA‑256 Is and Why It’s Trustworthy

**SHA‑256** is a cryptographic “fingerprint” of a file: a **64‑character** hexadecimal string (256 bits).  
The same file on any machine yields the same SHA‑256; changing **even one byte** completely changes the hash.

**What it provides in an audit:**
- ✅ **Integrity:** a matching hash means every bit of the checked file is identical to the original.  
- 🔁 **Repeatability:** the result does not depend on platform or software.  
- 🧾 **Manifests:** it’s convenient to keep lines like `HASH␣␣relative/path/to/file` and verify the **entire set** at once.

**Cryptographic properties (no math needed):**
- 🌩️ **Avalanche effect:** the tiniest input change fully alters the output.  
- 🧩 **Collision resistance:** it’s practically impossible to find **two different files with the same SHA‑256**.  
- 🎯 **Preimage resistance:** it’s practically impossible to **craft a file that matches a given SHA‑256**.

**Why “practically impossible”?**  
The value space is about **2^256** (≈ 1.16×10^77). Exhaustive search is physically unrealistic. Even with **quantum speedup (Grover’s algorithm)**, the security margin for integrity verification remains enormous.

> ⚠️ Limitation: SHA‑256 confirms **integrity**, but by itself does not answer **who** published the artifact and **when** it existed. Those aspects are covered by the **GPG signature** (authenticity) and the **OTS anchor** (time attestation).

---

## 2) 🧾 Recommended SHA‑256 Manifest Format

One line per file:
```
<64-char-hash><TWO SPACES><relative/path/to/file>
```

Example:
```
d2a5…9b0f  dist/app-1.0.0.tar.gz
f1c3…7a21  docs/README.md
```

Why so:
- ␠␠ **Two spaces** — the standard for `sha256sum`/`shasum -c`.  
- 📦 **Relative paths** — easier to move the repository as a whole.

---

## 3) 🗝️ What the GPG Signature Adds (Authenticity: “who”)

A **GPG signature** is a cryptographic “autograph” on a file (typically you sign the **manifest**).  
The signature is created with the publisher’s **private key** and verified with the corresponding **public key**.

**A successful signature check means:**
- the file has **not changed** since it was signed; and  
- it was signed by the **owner of the corresponding private key** (the one whose public key is used for verification).

**Critical:** you must be confident that the **public key is truly the author’s**.  
Compare the key’s **fingerprint** with the one announced beforehand via independent channels (official site/repo/release notes). If the fingerprint matches, the key is considered the author’s.

### 🧭 In short: if you downloaded the public key from the repo
1) **Show the fingerprint without importing:**  
   ```bash
   gpg -n --import-options show-only --import PUBLISHING_KEY.asc
   # or
   gpg --show-keys --with-fingerprint PUBLISHING_KEY.asc
   ```
2) **Cross-check the fingerprint via an independent channel.** Any mismatch → stop the process.  
3) **Import and verify the signature:**  
   ```bash
   gpg --import PUBLISHING_KEY.asc
   gpg --verify manifest.sha256.asc manifest.sha256
   ```
   It only passes if you see `Good signature …` **and** the fingerprint matches exactly.

(Optional for key rotation: `gpg --check-sigs <NEW_FPR>` — the new key should be signed by the old one.)

---

## 4) ⛓️ What the OTS Anchor Adds (Time: “when”)

**OpenTimestamps (OTS)** anchors a hash (usually the **manifest’s hash**) to **time in the public Bitcoin blockchain**.  
The file `manifest.sha256.ots` is a compact cryptographic proof that the given hash existed **no later than** a specific block. The proof does not reveal artifact contents (only the hash is anchored on-chain).

**Role of the layers together:**

| Layer | What it proves | What it protects against |
|---|---|---|
| **SHA‑256** | Bit‑for‑bit **integrity** of files | Silent modification/corruption |
| **GPG signature** | **Authenticity/authorship** of the manifest | Third‑party substitution of files and hashes |
| **OTS anchor** | **Date/time** of hash existence (no later than the block time) | Falsifying the publication timeline |

The combination makes forgery practically impossible: an attacker would need to find a SHA‑256 collision, compromise the private GPG key, **and** rewrite blockchain history — simultaneously.

---

## 5) 📦 What Is Typically Provided to Auditors

- **Artifacts**: release files (`…tar.gz`, `…zip`, etc.).  
- **Hash manifest**: `manifest.sha256`.  
- **GPG signature of the manifest**: `manifest.sha256.asc` (detached, ASCII‑armored).  
- **OTS anchor for the manifest**: `manifest.sha256.ots`.  
- **Public GPG key**: `PUBLISHING_KEY.asc` **and/or** its **fingerprint**, published in advance via independent channels.

---

## 6) 🧪 Verifying SHA‑256 (Commands for All OS)

**macOS:**
```bash
# single file
shasum -a 256 path/to/file
# entire manifest
shasum -a 256 -c manifest.sha256
```
(Alternative: `gsha256sum -c manifest.sha256` from GNU coreutils.)

**Linux:**
```bash
sha256sum -c manifest.sha256
# or
sha256sum path/to/file
```

**Windows — PowerShell:**
```powershell
# single file
Get-FileHash -Algorithm SHA256 -Path "C:\path	oile"

# via manifest (double space required)
Get-Content .\manifest.sha256 | ForEach-Object {
  if ($_ -match '^\s*([0-9a-fA-F]{64})\s\s(.+)$') {
    $hash = $matches[1].ToLower()
    $path = $matches[2]
    $got  = (Get-FileHash -Algorithm SHA256 -Path $path).Hash.ToLower()
    if ($got -eq $hash) { "OK     $path" } else { "FAILED $path" }
  }
}
```

**Windows — Git Bash / WSL:** same as Linux (`sha256sum -c manifest.sha256`).

Expected: `OK` for all lines; `FAILED` means byte‑level differences.

---

## 7) 🗝️ Verifying the GPG Signature (Authenticity)

**macOS / Linux / Windows (GnuPG/Gpg4win):**
```bash
# import the public key
gpg --import PUBLISHING_KEY.asc

# check the fingerprint against the one announced beforehand
gpg --fingerprint

# verify the manifest’s signature
gpg --verify manifest.sha256.asc manifest.sha256
```
It passes if you see `Good signature …` and the key’s fingerprint **exactly matches** the announced one.

---

## 8) ⏱️ Verifying the OTS Anchor (Time) — Including Multiple Blocks

**Automatic** (if the ots client is installed):
```bash
ots verify manifest.sha256.ots
```

**If `ots verify` fails** (no network/client, or the proof has not yet fully matured):
```bash
ots info manifest.sha256.ots
```
Notes for `ots info`:
- The output can contain **multiple anchors/attestations**, i.e., **multiple blocks** (different calendars may anchor at different times).  
- **Verify all listed blocks/txids** via an independent explorer (e.g., **mempool.space**): find the `block height/hash` or `txid`, confirm that the block exists on mainnet and has a valid timestamp.  
- For conservative dating, take the **earliest valid block** in the list — this is the minimal upper bound of “existed no later than.”

If you have network access, you can “upgrade” the proof and re‑verify:
```bash
ots upgrade manifest.sha256.ots
ots verify manifest.sha256.ots
```

---

## 9) 🧰 Why Archive Hashes Sometimes Differ — and How We Handle It

SHA‑256 depends on **every bit**. Archives include **metadata** (ordering, owners/groups, timestamps, xattrs, compression parameters).  
To make identical archives hash identically across machines, artifacts are packaged **deterministically** (fixed `mtime`, owners/groups, ordering; superfluous metadata removed; gzip without timestamp). In this mode, hash discrepancies are not expected.

---

## 10) ✅ Summary Verification Order (TL;DR)

1) **SHA‑256:**  
   `shasum -a 256 -c manifest.sha256` (macOS) / `sha256sum -c manifest.sha256` (Linux/WSL/Git Bash) / PowerShell script (Windows).  
   Expect `OK` for all lines.

2) **GPG signature:**  
   `gpg -n --import-options show-only --import PUBLISHING_KEY.asc` → compare the fingerprint via an independent channel → `gpg --import PUBLISHING_KEY.asc` → `gpg --verify manifest.sha256.asc manifest.sha256`.  
   Expect `Good signature …` and an exact fingerprint match.

3) **OTS anchor:**  
   `ots verify manifest.sha256.ots`; if issues arise, use `ots info manifest.sha256.ots` and **verify all listed blocks** in an explorer (e.g., **mempool.space**).  
   For the “earliest” timestamp, use the **earliest valid block**.

If all three steps pass, you have a cryptographically strong confirmation that:  
- the files are byte‑for‑byte identical to the originals (integrity, SHA‑256);  
- the manifest is signed by the author’s key (authenticity, GPG);  
- the manifest existed no later than the anchored time (dating, OTS), and when multiple anchors exist, the **earliest** valid one is considered.


---


# Quick SHA‑256 Check Without “Rebasing” Paths

This section is for those who **don’t want to fuss** with converting absolute paths to relative and other nuances.  
Just compute the hash locally and compare the **64‑character** string with what’s published in the repo.  
(`A–F`/`a–f` case doesn’t matter — normalize to lowercase if you like.)

---

## ✅ One File → One Hash

**macOS**
```bash
# hash + filename
shasum -a 256 /path/to/file
# 64‑char hash only
shasum -a 256 /path/to/file | awk '{print $1}'
```

**Linux**
```bash
sha256sum /path/to/file
sha256sum /path/to/file | awk '{print $1}'
```

**Windows (PowerShell)**
```powershell
# hash + path
Get-FileHash -Algorithm SHA256 -Path "C:\path	oile"
# 64‑char hash only
(Get-FileHash -Algorithm SHA256 -Path "C:\path	oile").Hash
```

Then open the corresponding published hash (or hash file) in the repo and **visually compare** your 64‑character string to the published one.

---

## 🔁 Compare “file A vs file B” (should be identical)

**macOS / Linux**
```bash
# Step-by-step
A=$(shasum -a 256 /path/to/A | awk '{print tolower($1)}')
B=$(shasum -a 256 /path/to/B | awk '{print tolower($1)}')
[ "$A" = "$B" ] && echo "MATCH" || echo "DIFFERENT"

# One-liner
[ "$(shasum -a 256 /path/to/A | awk '{print tolower($1)}')" = "$(shasum -a 256 /path/to/B | awk '{print tolower($1)}')" ] && echo "MATCH" || echo "DIFFERENT"

# Note: on Linux you can use sha256sum instead of shasum:
# A=$(sha256sum /path/to/A | awk '{print tolower($1)}')
# B=$(sha256sum /path/to/B | awk '{print tolower($1)}')
```

**Windows (PowerShell)**
```powershell
$A = (Get-FileHash -Algorithm SHA256 -Path "C:\path	o\A").Hash.ToLower()
$B = (Get-FileHash -Algorithm SHA256 -Path "C:\path	o\B").Hash.ToLower()
if ($A -eq $B) { "MATCH" } else { "DIFFERENT" }
```

---

## 📜 Manual Manifest Check (“as is”)

1. Open `manifest.sha256` (in an editor or directly in the repo).  
   Line format:
   ```
   <64-char-hash><TWO SPACES><relative/path/to/file>
   ```
2. Find the desired file and **copy its 64‑character hash** from the manifest.  
3. Locally compute the hash of your file (see commands above) and **compare all 64 characters**.

### (Optional) Verify a Single Manifest Line via Command

**macOS / Linux**
```bash
EXPECTED="paste_64_hex_here"
ACTUAL=$(shasum -a 256 /path/to/file | awk '{print $1}')
[ "${ACTUAL,,}" = "${EXPECTED,,}" ] && echo "OK" || echo "FAILED"
```

**Windows (PowerShell)**
```powershell
$Expected = "PASTE_64_HEX_HERE"
$Actual   = (Get-FileHash -Algorithm SHA256 -Path "C:\path	oile").Hash
if ($Actual.ToLower() -eq $Expected.ToLower()) {"OK"} else {"FAILED"}
```

---

## ℹ️ Helpful Tips

- **Exactly 64 characters.** Make sure there are no extra spaces/newlines when copying.  
- **Case-insensitive.** Compare in a single case (lowercase), as shown in the examples.  
- **Mismatch = a different file.** Even 1 byte difference yields a completely different hash.  
- **No “rebasing.”** For this method you **don’t need** to convert paths to relative — just take the file, compute the hash, and compare against the published value.

---

# Checking SHA‑256 Manifests with Absolute Paths

Sometimes the published `artifacts.sha256` is created with **absolute paths** (e.g., `/Users/name/.../file.svg`).  
**The hashes are still correct** — the path in the line is just a “label” that `sha256sum -c` uses to find the file.  
Below is how to verify signatures and hashes on **any machine**.

### TL;DR
1) Verify the **GPG signature** of the original `artifacts.sha256`.  
2) Create a **local copy** of the manifest with **relative names** and run `sha256sum -c` on that copy.

### 1) Signature Verification (original manifest)
```bash
gpg --verify artifacts.sha256.asc artifacts.sha256
# (optional) OpenTimestamps
ots verify artifacts.sha256   # or: ots verify artifacts.sha256.ots
```

### 2) “Rebasing” Paths Locally and Verifying Hashes

**Linux / macOS (bash)**
```bash
sed -E 's#^([0-9a-f]{64})  .*/([^/]+)$#  #' artifacts.sha256 > artifacts.local.sha256
sha256sum -c artifacts.local.sha256
```
> The `sed` command preserves the 64‑char hex hash and replaces the path with just the filename.

**Windows (PowerShell)**
```powershell
Get-Content artifacts.sha256 | % {
  if ($_ -match '^([0-9a-f]{64})\s\s(.+)$') {
    "$($matches[1])  $(Split-Path $matches[2] -Leaf)"
  }
} | Set-Content artifacts.local.sha256

# In Git Bash / WSL:
# sha256sum -c artifacts.local.sha256
```

### Notes & FAQ
- **Why does `-c` fail automatically?**  
  `sha256sum -c` tries to open the file **by the path from the manifest**. If the path is absolute and doesn’t exist on your machine — you get `No such file or directory`. **The hashes themselves are valid.**
- **Does the path affect the hash?**  
  No. SHA‑256 is computed **only from the file content**.
- **Can I verify a single file manually?**  
  Yes:
  ```bash
  sha256sum somefile.svg
  # Compare the 64‑char hash with the first column in artifacts.sha256
  ```
- **GPG/OTS practice**  
  Verify the **GPG signature** on the original `artifacts.sha256`. For convenience, keep both files side by side:
  - `artifacts.sha256` (original, signed; may contain absolute paths)  
  - `artifacts.local.sha256` (your local copy for `-c` checks)

### (Optional) macOS Without GNU Utilities
```bash
# Create a simple sorted manifest with relative names
ls -1 | grep -v '^artifacts\.sha256$' | LC_ALL=C sort | xargs shasum -a 256 > artifacts.local.sha256

# Verify a single file:
shasum -a 256 somefile.svg
```
