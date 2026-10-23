# Robot reCAPTCHA — Assembly Edition v5.0

**256 stages. 256 checks. One certified robot.**

An offline, single-file, reCAPTCHA-inspired parody game that reverses the ordinary challenge: instead of proving that you are human, prove that you can behave like a machine. Every stage presents a rule in **x86-64 / Intel-style assembly notation**; successful stages earn individual checkmarks. Clear all 256 stages to unlock a **personalized, downloadable PDF Certificate of Robotic Identity**.

> **Unofficial game:** This is a creative, simulated verification interface. It is **not Google reCAPTCHA**, does not perform secure identity verification, and does not issue an official credential.

## Quick start

1. Download **`robot_recaptcha_assembly_v5.html`**.
2. Open it in a modern browser (Chromium, Firefox, Safari, or Edge). No server, installation, account, or internet connection is required.
3. Enter an optional **Player Name** and edit your generated **Robot ID** in the *Operator / Unit Registry*.
4. Click **I'm a robot** to begin. Solve the current stage and press **VERIFY**.
5. When you pass a stage, the main checkbox immediately turns into a green checkmark. The corresponding cell in the **256-stage register** is also marked. Press **NEXT** to continue.
6. Once the stage register reaches **256 / 256**, click **DOWNLOAD CERTIFICATE .PDF**. The button also remains available after returning to the page in the same browser.

## Challenge protocols

| Family | Frequency | What the machine evaluates |
|---|---|---|
| `CRC32_SCAN` | 1 in 4 stages | Identify image tiles whose pixel-buffer CRC32 differs from the reference. |
| `RGB_CHANNEL_TEST` | 1 in 4 stages | Check the parity of a specified color-channel byte at an exact pixel coordinate. |
| `LSB_SIGNATURE` | 1 in 4 stages | Recover an eight-bit signature from least-significant bits in image pixels. |
| `CPYTHON_FLOAT` | 1 in 4 stages | Identify the exact output of a CPython 3.13 expression, including IEEE-754 binary64 rounding surprises. |

The pixel challenges contain densely generated technical textures with differences that may be as small as a single channel's least-significant bit. Use the **eye icon** to view machine-vision diagnostics. The Python challenges display the expression and candidate outputs as assembly-like, byte-oriented listings.

**Assembly notation is the puzzle interface:** the browser uses JavaScript to execute the game logic. It does *not* execute native x86-64 machine code, launch Python locally, or make network requests.

## Stage-by-stage checkmarks

- **Immediate feedback:** passing a stage turns the main checkbox green before moving on.
- **Persistent register:** a 16 × 16 matrix contains one checkmark for each of the 256 main stages.
- **Saved progress:** the current stage, retry count, last cleared stage, and operator identity are stored locally in the browser.
- **Reload-safe completion:** if you pass a stage and refresh before pressing NEXT, the game remembers that result.
- **Endless mode:** unlocking the certificate does not prevent you from continuing beyond stage 256.

The 256-stage register tracks the *main campaign*, not the optional endless stages.

## Personalized robotic certificate

Enter your identity before or after completing the campaign:

- **Player Name:** free-form name or alias, up to 36 characters; defaults to `ANONYMOUS MACHINE` when blank.
- **Robot ID:** generated automatically on first use and editable, up to 28 characters.

After completing every main stage, the page generates a **single-page, A4-size PDF directly inside the browser**. Its industrial design includes an obsidian background, etched circuit grid, classification bars, machine-style status stamps, a 256-bit completion register, an assembly listing, a UTC completion timestamp, a local serial number, and an explicit *UNOFFICIAL / ENTERTAINMENT ONLY* marking.

### Certificate classifications

| Failed attempts | Code | Tier | Designation |
|---:|---|---|---|
| 0 | `T0` | **OMEGA ZERO** | FLAWLESS SYNTHETIC |
| 1–3 | `T1` | **TITANIUM CORE** | HIGH-PURITY MACHINE |
| 4–15 | `T2` | **CHROME CLASS** | PRECISION CALIBRATED |
| 16–49 | `T3` | **CIRCUIT TESTED** | ADAPTIVE SYSTEM |
| 50 or more | `T4` | **PATCHED CONSTRUCT** | PERSISTENT MACHINE |

The tier is determined solely by the recorded number of incorrect VERIFY submissions. **A lower tier does not prevent certification.** The classification shown before the campaign is finished is provisional: later mistakes may change it.

Example certificate listing:

```asm
BITS 64
section .verified

    mov eax, 0x00000100      ; 256 / 256 PASSED
    mov ebx, 0x00000000      ; FAILED ATTEMPTS
    cmp eax, 0x00000100
    jne certificate_locked
    mov byte [ROBOT_FLAG], 0x01
    ret
```

The PDF is generated as a high-resolution rendered page embedded in a standard PDF container. Text therefore appears visually in the document but is not intended to be selectable or editable as separate PDF text objects.

## Privacy and trust model

- **Offline by design:** no accounts, telemetry, or remote APIs are needed.
- **Local data:** progress, failure count, Player Name, and Robot ID are saved using browser `localStorage`. The downloaded PDF stays on your device unless you share it.
- **Reset:** the **Reset all progress** control clears campaign progress and the completion record but leaves the player identity fields available for reuse.
- **Not tamper-proof:** because the game and verification run entirely in the client, a technically capable user can inspect or modify local game state. The certificate is a **game achievement**, not an independently verifiable identity credential or cryptographic attestation.

## Files

```text
Robot_reCAPTCHA_Assembly_v5/
├── robot_recaptcha_assembly_v5.html   # Complete offline game
├── README_Robot_reCAPTCHA_Assembly_v5.md
└── robot_v5_certificate_preview.png  # Example certificate visualization
```

## Troubleshooting

**The certificate download is disabled.** Complete every main stage until the register shows `256 / 256`. Returning to a partly completed game does not unlock it.

**My progress disappeared.** Check whether you opened the game in another browser, a private window, or a different site/origin. The game state resides in that browser's local storage.

**I cannot distinguish the images.** That is deliberate: this game includes pixel-level challenges designed for diagnostic tools, not unassisted human vision. Use the eye icon in image stages.

**I changed my name after winning.** Download the certificate again. The new export reads the current name and Robot ID fields.

**Is this official reCAPTCHA?** No. The name and visual references are part of a parody; no Google verification token is generated.

---

**`cmp stage_register, 256` · `je certificate_unlocked`**
