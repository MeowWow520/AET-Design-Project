# AGENTS.md

## Repo overview

This is a **documentation-only repo** for an undergraduate course design project: water temperature control system using analog electronics. No code, no build system, no CI.

## Authoritative files

- **`details.md`** — the primary, complete design document. All circuit parameters, calculations, and design decisions originate here. This is the source of truth.
- **`circuit_wiring_method_ai_v1.01.0.md`** — the corrected Proteus simulation wiring guide. This supersedes v1.00.0.

## Superseded / outdated files (do not reference)

- **`circuit_wiring_method_v1.00.0.md`** — original Proteus wiring; contains critical topology errors (see below). Replaced by v1.01.0.
- **`temp_chat.md`** — snapshot of the old (incorrect) wiring, same errors as v1.00.0. Ignore.
- **`assets/explain_rg1rf1.md`** — describes the old/wrong v1.00.0 topology. Misleading.

## Key design parameters

| Parameter | Value |
|-----------|-------|
| Sensor | NTC thermistor, R₂₅=10kΩ, B=3900K (Murata NTSD0XV103) |
| Bridge supply | +5V |
| LM358 supply | ±12V dual (Pin 4 → −12V **not** GND) |
| LM393 supply | +12V single |
| Stage 1 | Differential amplifier, gain ×10 (R_inv1=10kΩ, R_fb1=100kΩ, R_ninv1=10kΩ, R_div1=100kΩ) |
| Stage 2 | Non-inverting amp, gain ×10 (R_g2=1kΩ, R_fb2=9kΩ) |
| Total gain | ×100 |
| Low-pass filter | R=15.9kΩ, C=1μF, fc≈10Hz |
| Vref pot (RV2) | **100kΩ** (not 10kΩ — 10kΩ kills hysteresis) |
| R7 pull-up | 10kΩ to +12V on LM393 Pin 1 |

## Critical correction: v1.00.0 → v1.01.0

v1.00.0 wired the first stage as a **non-inverting amp on Vout+ only**. Vout+ is fixed at 2.5V DC (bridge balanced at 25°C); amplifying it ×10 gives 25V, which saturates LM358 (max output ≈10.5V) and renders the circuit non-functional.

v1.01.0 corrected this to a **differential amplifier** taking both Vout+ and Vout−, canceling the common-mode 2.5V DC and outputting 0V at balance — the only working topology.

If an agent generates or modifies circuit wiring documentation, it must follow the v1.01.0 differential-amplifier topology, not the v1.00.0 topology.

## Report structure

Report content is in `docx_main/` organized by chapter. The outline is in `pdf_content.md`. Word document exports are `*.docx` in the root. The `docx_main/摘要-Abstract/me.md` is just a feature checklist, not the final abstract — see `ds_v1.01.0.md` for the actual abstract text.

## File versioning

Documents use `v<major>.<minor>.<patch>` versioning. `ds_v1.01.0.md` is the current abstract; `ds_v1.00.0.md` is the prior draft. When creating new documents, follow this convention.
