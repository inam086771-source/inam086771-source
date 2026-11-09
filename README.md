# Hi, I'm Inam Ullah

Telecommunications engineer (M.Sc. Electronics and Communication Engineering, University of Siena) based in Naples, Italy, open to relocation within the EU. I use Python to build and verify wireless communication systems from first principles: OFDM physical-layer simulation, and machine learning for 5G URLLC channel prediction.

Below are projects where I wrote the code, tested it, and checked the results against theory.

## Projects

### OFDM From Scratch (public)
A complete OFDM transmit, channel and receive chain in Python, with no built-in OFDM functions.

- QPSK (Gray-coded) over 64 subcarriers, IFFT/FFT and cyclic prefix; perfect reconstruction verified (0 bit errors, error ~1e-16)
- 5-tap Rician-flavored multipath channel (K-factor 7 dB) plus AWGN at 15 dB SNR
- Equalization cut BER from 0.16% to 0.00% with perfect channel knowledge; pilot-based estimation (every 8th subcarrier) gave 0.09%, the expected realistic cost
- BER vs SNR sweep validated against the theoretical QPSK curve, including a real bug found and fixed (a missing factor of 2 in the Es/Eb scaling)

Tools: Python, NumPy, SciPy, Matplotlib. Code: [ofdm-from-scratch](https://github.com/inam086771-source/ofdm-from-scratch)

### URLLC CQI Prediction, 2026 refinement (code private)
In 5G URLLC, the base station picks the modulation and coding scheme from an outdated channel quality report (CQI aging), so it subtracts a safety margin (guard bias) that costs throughput. I revisited my master's thesis code, which uses an LSTM to predict the next CQI, re-ran it end to end, and refined it.

- **Found and fixed a design gap:** the code defined a Rician K-factor of 7 dB but the channel was effectively running pure Rayleigh fading, inflating deep-fade probability by roughly 25x. I implemented the proper Rician combination (line-of-sight plus scattered component, average power preserved).
- **Also fixed:** a chronological train/validation split (no overlapping windows across sets), a finite-blocklength rate floored at 0 so BLER falls monotonically with guard bias, and results averaged over 10 independent channel realizations.
- **Result (target BLER 10⁻⁵):** the LSTM reaches the target with an 8.7 dB guard bias (throughput 0.69), close to the 6.3 dB of the genie-aided ideal predictor, while the "previous CQI" baseline never reaches the target in the 0 to 10.3 dB range.
- **Investigated the remaining gap:** tanh with input normalization, a 20-step context, and a 2-layer 400-unit model (~12x more parameters) all landed at 8.7 to 9.0 dB. This points to an information limit of CQI-only prediction, not a model-capacity limit.

Tools: Python, TensorFlow/Keras, NumPy, SciPy. Code is private; happy to walk through it on request.

### Master's Thesis: Using Machine Learning for URLLC Channel Prediction (code private)
University of Siena, Department of Information Engineering and Mathematics, 2022. Builds on Bartoli and Marabissi, IEEE Transactions on Vehicular Technology, 2022.

- Analyzes how CQI aging affects reliability under the finite-blocklength regime typical of URLLC (32-byte packets, target BLER 10⁻⁴ and 10⁻⁵, no retransmissions).
- Proposes a Deep Recurrent Neural Network (LSTM) that forecasts the next CQI from past reports, and evaluates it by decoding error probability, throughput and SINR.
- Simulation setup: 5G NR numerology μ = 2, 70 km/h UE at 1 GHz, Rician channel (K = 7 dB) with a Jakes Doppler spectrum, compared against a genie-aided ideal predictor and a previous-CQI baseline.
- Result: the DRNN follows the ideal predictor closely and is far better than the previous-CQI benchmark at the same guard bias.

<details>
<summary>Read the full abstract</summary>

Ultra-reliable low latency communication (URLLC) is an emerging use case of 5G New Radio standards introduced in 3GPP Release 15. It is an enabler of a multitude of mission-critical applications that demand very low latencies and very highly reliable service. An important element to fulfil the stringent URLLC requirements is a suitable link adaptation algorithm that must be able to select the appropriate modulation and coding scheme (MCS) according to the wireless link quality. Link adaptation usually depends on the channel quality indicator (CQI), which is feedback information from the user equipment (UE) that tells the base station (BS) about the current state of the channel. Based on CQI measurements computed from the Signal-to-Interference-plus-Noise Ratio (SINR) of the UE, the BS (e.g., the next-generation NodeB, gNB) selects an MCS to be used for the next downlink transmission.

However, communication channels are inherently variant due to changes in traffic load, user mobility, and delays such as scheduling, processing and measurement. Thus the estimate of the SINR at the transmitter side usually deviates from the actual value, which introduces the CQI aging effect: the mismatch between the current state of the channel and its CQI representation. Using outdated CQI values may lead to the selection of a wrong MCS, which badly affects the performance of the communication link. This is particularly relevant in URLLC, where the control of reliability can be negatively impacted, especially when the channel varies fast.

To cope with these challenges, this thesis analyses the effects of CQI aging on URLLC considering transmissions under the finite-blocklength regime that characterises this type of communication, and proposes a possible solution: forecasting the next CQI value using the knowledge of past CQI reports. A Deep Recurrent Neural Network (DRNN) is used to predict the next CQI from the previous reports, and performance is given in terms of decoding error probability, throughput and SINR.

</details>

Code and further detail available on request.

## Contact

[LinkedIn](https://www.linkedin.com/in/inam771) | inam086771@gmail.com

Some projects are private. If you'd like details or a walkthrough of any of them, just get in touch.
