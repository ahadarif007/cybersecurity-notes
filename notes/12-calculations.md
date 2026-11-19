[← README](../README.md) · [← 11. Lab walkthroughs](11-lab-walkthroughs.md) · [13. Final review sheet →](13-final-review-sheet.md)

# 12. Calculations

## Clusters, slack and offsets ★★★

**Given:** a 9,400-byte file, 4,096-byte clusters, and data runs (start 120, length 2) and (start 900, length 1).

| Question | Working | Answer |
|---|---|---|
| Clusters needed? | 9,400 ÷ 4,096 = 2.29 → round **up** | **3 clusters** |
| Space allocated? | 3 × 4,096 | **12,288 bytes** |
| Slack? | 12,288 − 9,400 | **2,888 bytes** |
| Cluster read order? | Follow the runs | **120, 121, 900** |
| Where is file byte 8,500? | 8,500 − (2 × 4,096) = 308, so it's in the third cluster | **LCN 900, offset 308** |
| Byte offset within the volume? | (900 × 4,096) + 308 | **3,686,708** |

🧠 **Formulas:** clusters = ⌈size ÷ cluster size⌉ · slack = clusters × cluster size − size · volume offset = LCN × cluster size + offset within the cluster

If you're working from a whole-disk image, also **add the volume's starting offset**.

## Decoding a data run list ★

`11 02 78 21 01 0C 03 00`

| Bytes | Meaning |
|---|---|
| `11` | Header. Low nibble = 1 byte of length, high nibble = 1 byte of offset |
| `02 78` | Run of 2 clusters starting at LCN 0x78 = **120** |
| `21` | Header: 1 byte of length, 2 bytes of offset |
| `01 0C 03` | Run of 1 cluster. Offset delta (little-endian) 0x030C = 780, relative to the previous run: 120 + 780 = LCN **900** |
| `00` | End of the run list |

Real run lists can use signed (negative) deltas and longer fields. This example has no sparse or compressed runs.

## Time zones ★★★

Subtract a positive offset to get UTC (and add a negative one).
- 08:40 +01:00 → **07:40 UTC**
- 14:30 +02:00 → **12:30 UTC**

## LSB capacity (Week 2 lab) ★★

**A 3,840 × 2,560 RGB image, using 2 bits per colour channel:**

| Step | Working | Answer |
|---|---|---|
| Pixels | 3,840 × 2,560 | 9,830,400 |
| Bits | Pixels × 3 channels × 2 bits | 58,982,400 |
| Bytes | Bits ÷ 8 | **7,372,800** (the lab reports 7,372,771 after header overhead) |

## Argon2id memory ★★★

**Given:** m = 65,536 KiB, p = 4.

| Question | Working | Answer |
|---|---|---|
| Memory per verification? | 65,536 KiB ÷ 1,024 | **64 MiB** |
| Memory per lane? | 64 MiB ÷ 4 lanes | **16 MiB** |
| 20 verifications at once? | 20 × 64 MiB = 1,280 MiB, then ÷ 1,024 | **1.25 GiB** |

This is working memory used during the check, not the size of the stored record.

## Quick conversions ★★

- **Cookie lifetime:** Max-Age=1800 → 1,800 ÷ 60 = **30 minutes**
- **Argon2 version:** `v=19` → decimal 19 = hex **0x13** (not a year)
- **Entropy:** 256 equally likely byte values → log₂ 256 = **8 bits**

---

[← README](../README.md) · [← 11. Lab walkthroughs](11-lab-walkthroughs.md) · [13. Final review sheet →](13-final-review-sheet.md)
