# synth_pico

A small hybrid synthesizer: a Raspberry Pi Pico 2 W generates the waveforms and envelopes digitally, and the sound runs through an analog VCA and filter before reaching the line output.

Designed in KiCad 10. **Status:** PCBs and parts ordered, firmware not written yet. The first version is a single voice.


## How it works



- **Oscillators and envelopes** run in firmware on the RP2350. Sine, square, triangle and sawtooth are selectable, and 4 LEDs show the active waveform.
- **Control voltages** come from an MCP4728 and are level-shifted by a TL074H to drive the OTAs.
- **The analog section** runs from ±5 V generated on the board by a TPS65133 from the Pico's VSYS.
- **8 knobs** (Attack, Decay, Sustain, Release, Cutoff, Resonance, Envelope, Volume) are read through a 74HC4051 mux into the Pico's ADC.
- **Display header** for a 4-pin I²C OLED.
- **Line-level output** for active speakers. There is no onboard power amp.

## Boards

| Board | Layers | Size | Notes |
|---|---|---|---|
| Main (`synth_pico.kicad_pro`) | 4 (GND plane on In1, power on In2) | 160 × 78 mm | Powered over the Pico's USB |
| Keyboard (`keyboard/`) | 2 | ~170 × 105 mm | 12 keys (one octave) + 4 function buttons, Cherry MX–compatible switches, 4×4 diode matrix |

The boards connect through a 10-pin 2.54 mm header and an F-F Dupont ribbon:

| Pin | 1 | 2–5 | 6–9 | 10 |
|---|---|---|---|---|
| Signal | GND | ROW0–3 | COL0–3 | 3V3 (not used by the keyboard) |

The function buttons are octave down, octave up, waveform select, and one spare.

## Repository layout

```
synth_pico.kicad_pro / .kicad_sch / .kicad_pcb   main board
keyboard/                                        keyboard board
art.pretty/                                      silkscreen artwork footprints
Library.pretty/, libs/                           custom footprints and symbols
gerbers/                                         fabrication outputs
```

## Notes

- The pot net names in the schematic (`POT_xxx`) don't match the silkscreen labels. The knob order was changed late, and the knob-to-mux-channel mapping is handled in firmware.
- The BOM lists a PCM5102A, but the PCM5101A (and PCM5100A) are pin-compatible drop-in replacements.

---

made by Mihnea Gherasim
