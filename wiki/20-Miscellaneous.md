# Amplifying the SDR output signal

Most SDRs are not designed to directly drive a TETRA transmitter chain at operational power levels. External amplification is therefore sometimes considered.

> ⚠Please note:  
> The information below is intentionally high-level and provided for orientation only.  
> In many small experimental setups (e.g. lab testing, very short-range work, hotspot-style use), typical SDR output levels are often already sufficient.  
> Adding external amplification should only be considered after a careful assessment of the points below, and with appropriate RF measurement equipment and regulatory awareness as incorrect amplification can easily result in out-of-band emissions, even if the baseband signal itself is correct.


When adding an external power amplifier, several pitfalls should be considered:

- **Linearity**  
  Even though TETRA uses constant-envelope modulation, strict spectral masks apply. Non-linear amplification can cause significant spectral regrowth.

- **Adjacent channel power (ACP)**  
  Poor amplifier linearity or insufficient filtering can lead to excessive ACP, potentially causing interference on neighbouring channels.

- **Gain staging**  
  Overdriving an amplifier from an SDR output (close to the [P1dB](https://en.wikipedia.org/wiki/Compression_point)) is a common mistake. Conservative drive levels and adequate headroom are essential.

Commercial TETRA base stations typically rely on carefully linearised RF power amplifiers (far from the P1dB point for each stage) and closed-loop power control and monitoring.  

When experimenting, it is strongly recommended to:
- Start at very low output power  
- Use a spectrum analyser to verify spectral purity  
- Ensure compliance with local regulatory limits  

## References
Here are some more documentation on this specific issue:

- “TETRA” (Wikipedia - Chapter "Technical Aspects"). Details modulation scheme and why linear amplification is required. https://en.wikipedia.org/wiki/TETRA
- “Distortion in RF Power Amplifiers” (PDF). Explains spectral regrowth and nonlinearity in RF amplifiers. https://gctjaipur.files.wordpress.com/2015/08/distortion-in-rf-power-amplifiers-ebook-lib.pdf
- Ahmed, A. “Feedback Linearization of RF Power Amplifier for Wireless Systems” (article). Discusses adjacent channel power ratio (ACPR), EVM, and linearization approaches. https://beei.org/index.php/EEI/article/download/276/186
- Briffa, M. A. *Linearization of RF Power Amplifiers* (PhD thesis). Includes treatment of Cartesian feedback linearization. https://vuir.vu.edu.au/374/7/BRIFFA%20Mark-thesis_nosignature.pdf