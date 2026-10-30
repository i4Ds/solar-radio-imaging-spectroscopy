# A CALLISTO receiver: replace it, or build another

Separate from the sabbatical plan. Prices are US dollars on 25 October 2026
unless a shop date is given. CALLISTO is the maker’s price of 430 Swiss
francs, converted at 1.213 dollars per franc.

## What was measured

A basic tinySA and a generic RTL-SDR with an LNA were tried on the bench.
Neither replaces a CALLISTO station. The analyser reaches 350 MHz in about
0.4 s on the radio (290 points, sweeptime 411 ms) and is about 18 dB noisier
than CALLISTO’s 10 dB maximum. The RTL-SDR locked from 10 to 1766 MHz and
sees 2.4 MHz at once. Its noise figure is the LNA’s, and that was not
measured.

## Buying something else

Nothing between $500 and $1000 watches 45–870 MHz at once. The only radios
in that window are two bladeRF boards, and their published receive range
starts at 70 MHz. The quieter radio is cheaper: an RSPdx-R2 at about $240.
The instrument that sees the whole band at the same time is an Ettus USRP
X410, at $33,020.

| Product | Price | Band | At once | How 45–870 MHz is covered | Noise |
|---|---:|---|---|---|---|
| This RTL-SDR | $35 | Centers 10–1766 MHz, measured | 2.4 MHz, 8-bit | Hundreds of retunes | Set by the LNA, not measured |
| This tinySA | $63 | 0–350 MHz, filtered | One sweep, 290 points | 0.41 s to 350 MHz. The rest is missing | About 28 dB on the low input |
| tinySA Ultra+ | $170 | 100 kHz–900 MHz on the low input | One sweep, 450 points | About 1.8 MHz between points | About 5 dB with the LNA |
| RX-888 MKII, Alcalá | $170–$290 | 1 kHz–1.7 GHz, split at 64 MHz | 64 MHz below 64 MHz; 10 MHz above | Direct sampling through 64 MHz, then a tuner | Not published |
| RSPdx-R2 | $240 | 1 kHz–2 GHz | 10 MHz, 14-bit | About 80 steps. A 300 kHz channel is available | 2.1–6.0 dB from 100 to 660 MHz |
| LimeSDR Mini 2.0 | $399 | 10 MHz–3.5 GHz | 40 MHz, 12-bit | About 21 steps. Includes 45 MHz | Not published for the board |
| CALLISTO | $522 | 45–870 MHz | 300 kHz | 200 channels in 0.25 s | 10 dB maximum |
| bladeRF 2.0 micro xA4 | $540 | Receive 70 MHz–6 GHz | 56 MHz, 12-bit, two channels at one tune | About 15 steps. Misses 45–70 MHz | Not published for the board |
| bladeRF 2.0 micro xA9 | $860 | Same as the xA4 | Same radio, larger programmable logic | Same coverage as the xA4 | Not published for the board |
| Ettus B200mini | $1,503 | 70 MHz–6 GHz | 56 MHz, one receiver | About 15 steps. Misses 45–70 MHz | Chip 2 dB at 800 MHz, before the board |
| Signal Hound SM200B | $17,750 | 100 kHz–20 GHz | 160 MHz for 2 seconds; sweep 1 THz/s | Under 1 ms at 30 kHz resolution | About 18 dB below 700 MHz |
| Signal Hound SM200C | $21,495 | 100 kHz–20 GHz | 160 MHz streamed continuously | Under 1 ms at 30 kHz resolution | About 18 dB below 700 MHz |
| Ettus USRP X410 | $33,020 | 1 MHz–7.2 GHz | Four independent 400 MHz receivers | The whole band at the same time | 8 dB from 500 MHz to 3.1 GHz |

The RX-888 is the receiver a 2025 master’s thesis at the University of Alcalá
(Rabadán Parra, http://hdl.handle.net/10017/69083) validated as a spectrometer
for the network. The thesis reports that the design goals were met. It does
not give a sensitivity number. The X410 datasheet used here has no noise
figure below 500 MHz. The Signal Hound noise below 700 MHz is read from a
displayed floor of −156 dBm/Hz. The $35 RTL-SDR is an RTL-SDR Blog V3
dongle, the current named equivalent of the generic R820T on the bench,
without the amplifier. The LimeSDR price is the campaign price. Ham Radio
Outlet listed the RSPdx-R2 at $239.95; SDRplay listed £188 ex VAT.

## The setting that mimics a CALLISTO channel

On an RSPdx-R2:

- Zero IF, sample rate 6.0 MHz, IF bandwidth 5.0 MHz. That stays in the
  14-bit range. Above 6.048 MHz the converter drops bits, and above about
  9.2 MHz it is 8-bit.
- AGC off, gain fixed, FM notch on.
- FFT length 320. Each bin is 18.75 kHz, and 16 bins add to 300 kHz.
- Center those channels on the station frequency list, on the 62.5 kHz grid.
- Average the power for 1 ms, store decibels, write one spectrum every 0.25 s.

That copies the channel width, the integration, and the file cadence. It
does not hop the whole 45–870 MHz band in a quarter of a second: 5 MHz is
one slice. The single-channel alternative is IF bandwidth 300 kHz, sample
rate 2 MHz, decimation 4, which is the pairing SDRplay documents for that
bandwidth.

## Keeping the instrument, without one builder

A new board that behaves as a CALLISTO can be made with 2026 parts and a
Chinese prototype line. A copy of the 2006 board cannot. The obsolete part
is the Philips tuner module, the CD1316, a shielded can, not a chip that
can be sent back into production. On 25 October 2026 the project price list
showed 20 of those spares, at 20 Swiss francs, described only as
“CALLISTO-compatible”, not as a specific suffix. The instrument uses the
CD1316LS/IV-3. No current part drops into that footprint, and no
replacement tuner was confirmed in distributor stock that day. The MxL661
is still an active MaxLinear part (44–1002 MHz). Broker sites claim stock;
Digi-Key did not show a count.

The logarithmic detector, an AD8307, is still manufactured. Its 25 mV/dB
slope is the instrument’s 25.4 mV/dB. The schematics, firmware archive and
manufacturing checklist are already published. What sits with one person
is the alignment, the pass-fail test, the spare tuners, and support of the
stations.

1. One spare tuner, one board, a second person. Record the quartz, the coil
   alignment, the detector slope, and the spectrum against a working station.
2. Remake the printed board and fit it with the 20 spare tuners. That does
   not create more tuners.
3. Design a new front end with the same behaviour: 45–870 MHz, 300 kHz
   channels on the 62.5 kHz grid, 200 channels in 0.25 s, noise figure no
   worse than 10 dB, the same files as now. Choose the tuner the week the
   schematic is drawn, from a part that is actually in stock. Do not start
   from a silicon tuner already withdrawn from new designs. Replace the
   hand-wound coils with catalog filters.
4. Same antenna, same hour, the new board beside a CALLISTO. Compare the
   burst, the quiet spectrum, the noise figure, the channel frequencies, and
   stability over at least 100 seconds.
5. Leave Gerbers, a bill of materials, a test jig, and the firmware where a
   second person can order the board.

Until that test passes, the Alcalá receiver is a software path to the same
files, not a new CALLISTO board.
