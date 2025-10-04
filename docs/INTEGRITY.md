# 🔐 For Audit: Verifying Integrity, Authenticity, and Release Date of Artifacts
**(SHA-256 → GPG signature → OTS anchor — a cryptographic combo)**

Public key in the **results** repo: [`keys/emm_pub_key.asc`](https://github.com/euro-macromechanica-backtest/results/blob/main/keys/emm_pub_key.asc) (ASCII-armored)  
**GPG key fingerprint (rleydev — thelaziestcat):** `4E53 DA03 D1A1 644E 6479  3C47 EEEC 0AFA 4D8A F0A7`

---

Below is a neutral, detailed, plain-English guide explaining **what** is being verified, **why** we use three protection layers, and **how** to verify on macOS, Linux, and Windows.

---

## 1) 🧱 Foundation: what SHA-256 is and why it matters

**SHA-256** is a file “fingerprint”: a **64-character** hexadecimal string (256 bits).  
The same file on any machine yields the same SHA-256; changing **even one byte** completely changes the hash (avalanche effect).

**What this provides for audit:**  
- ✅ **Integrity:** a matching hash guarantees the file hasn’t changed by a single bit.  
- 🔁 **Repeatability:** the result is OS/tooling-independent.  
- 🧾 **Manifests:** convenient for verifying whole sets of artifacts at once.

> ⚠️ Limitation: SHA-256 confirms **that** a file is unchanged, but not **who** released it or **when** it existed. Those are covered by the **GPG signature** (authenticity) and the **OTS anchor** (time).

### 🔒 Why “faking a hash” is practically impossible (simple explanation)

When people say “fake SHA-256,” they typically mean one of two attacks:

1) **Forge a file to match a given, fixed hash.**  
2) **Find another file with the same hash as an already published one.**

Both reduce to **blindly guessing** the one winning combination among an **astronomical** number of possibilities.

#### How astronomical?

- For SHA-256 there are roughly **2^256** possibilities — a 77-digit number.  
  Imagine a 77-character random code you must guess on the first try.
- Even the “easier” attack — find **any** two distinct strings with the same hash (a collision) — needs about **2^128** attempts.

These numbers are far beyond what could be brute-forced **in humanity’s lifetime** on any reasonable hardware.

#### Why can’t you tweak a file slightly and keep the same hash?  
Because SHA-256 has a strong **avalanche effect**: change one byte and the hash changes completely and unpredictably.

---

### 🧪 What if we had a “quantum accelerator” (Grover’s algorithm)?

The quantum **Grover** algorithm speeds up brute force: instead of `N` tries you need about `√N`. For SHA-256:  
- instead of **2^256** you get **about 2^128**. That’s **still absurdly large**.

**Concrete back-of-the-envelope:**  
- Suppose a fantastically powerful quantum computer does **1 trillion** (10^12) Grover iterations per second.  
- To match **one fixed SHA-256**, it needs about **2.7×10^38** iterations.  
- That’s roughly **8×10^18 years** — **hundreds of millions of times longer** than the age of the universe (~1.38×10^10 years).

**And for collisions with “quantum magic”?**  
Even specialized quantum tricks land you around **≈ 2^(256/3) ≈ 5×10^25** attempts.  
At 1 trillion ops/sec that’s **~1.5 million years** of nonstop compute.

> Bottom line: quantum methods reduce the exponent, but **don’t make the attack practical**. For audit purposes, SHA-256 remains rock-solid with massive safety margins.

---

### 🧭 Key takeaways

- **Forging a specific hash** (or a twin file) == guessing a winning ticket among **astronomical** possibilities. Practically **impossible**.  
- **Collisions** (“any two different strings with one hash”) are likewise out of reach.  
- In the **SHA-256 + GPG + OTS** stack, tampering or “rewriting history” becomes not merely hard but **unrealistic**.

---

## 2) 🧾 Recommended SHA-256 manifest format

One line per file:
```
<64-char-hash><TWO SPACES><relative/path/to/file>
```

Example:
```
d2a5…9b0f  dist/app-1.0.0.tar.gz
f1c3…7a21  docs/README.md
```

Why this matters:  
- ␠␠ **Two spaces** — the standard for `sha256sum` / `shasum -c`.  
- 📦 **Relative paths** — easy to relocate the repo as a whole.

---

## 3) 🗝️ What a GPG signature adds (authenticity: “who”)

A **GPG signature** is a cryptographic “autograph” of a file (we usually sign the **manifest**).  
The signature is created with the publisher’s **private key** and verified with their **public key**.

A successful check means:  
- the file **hasn’t changed** since signature time; and  
- it was signed by the **holder of the corresponding private key**.

**Important:** ensure the public key is truly the author’s. Verify the **fingerprint** via an independent channel.

### Quick path: if the key ships in the repository
```bash
gpg -n --import-options show-only --import keys/emm_pub_key.asc
# verify the fingerprint via an independent channel
gpg --import keys/emm_pub_key.asc
gpg --verify manifest.sha256.asc manifest.sha256
```

---

## 4) ⛓️ What an OTS anchor adds (time: “when”)

**OpenTimestamps (OTS)** binds a hash (typically the manifest hash) to **time** in the Bitcoin mainnet blockchain.  
The file `manifest.sha256.ots` proves the hash existed **no later than** a specific block. The artifact contents remain private.

**How the layers work together:**

| Layer | Proves | Protects against |
|---|---|---|
| **SHA-256** | Bit-level **integrity** | Silent modification/corruption |
| **GPG signature** | **Authenticity** (who signed) | Substitution of files/hashes |
| **OTS anchor** | **Time attestation** (no later than block time) | Faking the timeline |

---

## 5) 📦 What we ship to auditors

- **Release artifacts** (`…csv`, `…txt`, etc.).  
- **Manifest**: `manifest.sha256`.  
- **Detached GPG signature of the manifest**: `manifest.sha256.asc` (ASCII-armored).  
- **OTS proof**: `manifest.sha256.ots`.  
- **Public GPG key**: `keys/emm_pub_key.asc` and/or its **fingerprint**.

---

## 6) 🧪 Verifying SHA-256 (commands for all OSes)

**macOS**
```bash
# single file
shasum -a 256 /path/to/file
# full manifest
shasum -a 256 -c manifest.sha256
# alternative: gsha256sum -c manifest.sha256 (coreutils)
```

**Linux**
```bash
sha256sum -c manifest.sha256
# or
sha256sum /path/to/file
```

**Windows — PowerShell**
```powershell
# single file
Get-FileHash -Algorithm SHA256 -Path "C:\path\to\file"

# verify via manifest (two spaces required)
Get-Content .\manifest.sha256 | ForEach-Object {
  if ($_ -match '^\s*([0-9a-fA-F]{64})\s\s(.+)$') {
    $hash = $matches[1].ToLower()
    $path = $matches[2]
    $got  = (Get-FileHash -Algorithm SHA256 -Path "$path").Hash.ToLower()
    if ($got -eq $hash) { "OK     $path" } else { "FAILED $path" }
  }
}
```

**Windows — Git Bash / WSL**: same as Linux (`sha256sum -c manifest.sha256`).

You should see `OK` for all lines; `FAILED` indicates byte-level differences.

---

## 7) 🗝️ Verifying the GPG signature

> See the quick steps in §3 for importing the key and checking the fingerprint. Below is the signature-check command itself.

```bash
gpg --verify manifest.sha256.asc manifest.sha256
```
Pass criteria — you see `Good signature …` **and** the fingerprint matches the pre-published value.

---

## 8) ⏱️ Verifying the OTS anchor (including multiple blocks)

Before verifying, **upgrade** the proof if possible:
```bash
ots upgrade manifest.sha256.ots
ots verify  manifest.sha256.ots
```

If automatic verification isn’t available:
```bash
ots info manifest.sha256.ots
```
- Using an external explorer (e.g., mempool.space), confirm the blocks are on **Bitcoin mainnet**.  
- The output may include **multiple** blocks/txids. For the earliest attestation, take the **earliest valid block** from the list.

---

## 9) ✅ Verification flow (TL;DR)

1) **SHA-256** — `shasum -a 256 -c manifest.sha256` (macOS) / `sha256sum -c manifest.sha256` (Linux/WSL/Git Bash) / the PowerShell script (Windows).  
2) **GPG** — import the key + check fingerprint (see §3) → `gpg --verify manifest.sha256.asc manifest.sha256`.  
3) **OTS** — `ots verify manifest.sha256.ots` (use `ots upgrade` / `ots info` if needed).

Together these confirm that:  
- files are identical to the originals (SHA-256);  
- the manifest is signed by the author’s key (GPG);  
- the manifest existed **no later than** the block time (OTS).

---

## 10) 🧯 Common pitfalls (and fixes)

**SHA-256 / manifest**  
- *Tab or single space instead of two spaces.*  
  Use **exactly two spaces** between hash and path; otherwise `-c` won’t parse the line.  
- *BOM at the start of `manifest.sha256`.*  
  Save the file as **UTF-8 without BOM**.  
- *CRLF / line endings.*  
  `sha256sum -c` handles **`\r\n`**, but trailing spaces at line ends will break parsing. Remove them.  

**GPG**  
- *“Good signature,” but a trust warning.*  
  That’s fine: it’s the web-of-trust model. The key thing is **fingerprint match** with a pre-published value.  
- *Imported the wrong key / wrong file.*  
  Double-check you imported the **correct `.asc`**, and re-check the fingerprint.  
- *Signed the wrong file.*  
  Make sure you’re verifying `manifest.sha256` against `manifest.sha256.asc` (not, say, `manifest.txt`).

**OTS**  
- *Old `.ots` without a final anchor.*  
  Run `ots upgrade manifest.sha256.ots`, then `ots verify`.  
- *No network / blocked access.*  
  Use `ots info` to inspect locally, then confirm blocks via an explorer (mainnet).  
- *Testnet vs mainnet confusion.*  
  Verify txid/blocks on **Bitcoin mainnet** (e.g., mempool.space without `/testnet`).

---

# Quick SHA-256 checks

Just compute the hash locally and compare the 64-char string to the published one.

## ✅ One file → one hash

**macOS**
```bash
shasum -a 256 /path/to/file
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
Get-FileHash -Algorithm SHA256 -Path "C:\path\to\file"
# just the 64-char hash
(Get-FileHash -Algorithm SHA256 -Path "C:\path\to\file").Hash
```

## 🔁 Compare “file A vs file B”

**macOS / Linux**
```bash
A=$(shasum -a 256 /path/to/A | awk '{print tolower($1)}')
B=$(shasum -a 256 /path/to/B | awk '{print tolower($1)}')
[ "$A" = "$B" ] && echo "MATCH" || echo "DIFFERENT"
```

**Windows (PowerShell)**
```powershell
$A = (Get-FileHash -Algorithm SHA256 -Path "C:\path\to\A").Hash.ToLower()
$B = (Get-FileHash -Algorithm SHA256 -Path "C:\path\to\B").Hash.ToLower()
if ($A -eq $B) { "MATCH" } else { "DIFFERENT" }
```

> Note. For roll-up manifests containing “bare” hashes, it’s convenient to script a small helper: compute SHA-256 for all files in a directory, normalize/sort the list, and compare against the manifest. That avoids checking each file by hand.
