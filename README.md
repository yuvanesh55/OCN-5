# Free-Space Optical Communication (FSO) Implementation

## Aim: 
To implement and analyze a free space optical communication link using a laser/LED transmitter and photodiode/APD receiver.

## Apparatus/Software Required:
Python Colab

## Theory:
	FSO uses light beams to transmit data through free space instead of fiber.
	Performance depends on distance, alignment, and atmospheric attenuation.
	Received power decreases exponentially with distance.

## Formula:
Loss=10⋅〖log⁡〗_10 (P_in/P_out )

## 1
## Setup the Transmitter
Info
Prepare the optical source for transmission.
	Use a Laser/LED source
	Mount it on an optical bench
	Ensure stable power supply

## 2
## Align the Optical Path
Warning
Proper alignment ensures maximum received signal.
	Place transmitter and receiver in line of sight
	Adjust using lenses/mirrors if needed
	Minimize beam divergence

## 3
## Connect the Receiver
Setup the photodiode/APD to detect the optical signal.
	Mount detector opposite to transmitter
	Connect to oscilloscope for signal observation
	Record received power

## 4
## Measure Received Power
Collect data at different distances.
Loss = 10 log10(Pin / Pout)
	Vary distance (1 m, 5 m, 10 m, etc.)
	Note received power in µW
	Calculate link loss

## 5
## Plot Graphs
Visualize performance of FSO link.
	Plot Received Power vs Distance
	Observe exponential decay trend
	Compare with theoretical model

## 6
## Conclude Results
Success
Summarize findings of the experiment.
	Reliable communication up to ~10 m indoors
	Loss increases with distance
	Alignment critical for performance

## Sample Output
Graph: Received Power vs Distance (decay curve)

<img width="571" height="455" alt="download (4)" src="https://github.com/user-attachments/assets/1d3812a0-eea6-4fb3-94d8-d164e47a2164" />
<img width="562" height="455" alt="download (5)" src="https://github.com/user-attachments/assets/6d679c3b-5c43-499b-a2ad-19aae458a5ae" />
<img width="563" height="455" alt="download (6)" src="https://github.com/user-attachments/assets/7978557c-e3f1-4163-bc59-63d0109ba317" />




## Python Code
```
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------
# PARAMETERS
# -----------------------------
lambda_nm = 1550          # wavelength (nm)
lambda0 = 550             # reference wavelength (nm)
L = 2                     # link distance (km)
V_range = np.linspace(0.1, 20, 100)  # visibility range (km)
R_range = np.linspace(0, 50, 100)    # rain rate (mm/hr)

# -----------------------------
# KRUZE MODEL FUNCTION
# -----------------------------
def kruse_q(V):
    if V > 50:
        return 1.6
    elif V > 6:
        return 1.3
    else:
        return 0.585 * (V ** (1/3)) + 0.34

# Vectorize q
q_values = np.array([kruse_q(v) for v in V_range])

# -----------------------------
# ATTENUATION CALCULATION
# -----------------------------
alpha = (3.91 / V_range) * ((lambda_nm / lambda0) ** (-q_values))

# Total attenuation (dB)
tau = 4.343 * alpha * L

# -----------------------------
# RAIN ATTENUATION
# -----------------------------
A_rain = 1.076 * (R_range ** 0.67)

# -----------------------------
# PLOTS
# -----------------------------

# 1. Visibility vs Attenuation
plt.figure()
plt.plot(V_range, tau)
plt.xlabel("Visibility (km)")
plt.ylabel("Atmospheric Attenuation (dB)")
plt.title("FSO Attenuation vs Visibility (Kruze Model)")
plt.grid()
plt.show()

# 2. Rain attenuation
plt.figure()
plt.plot(R_range, A_rain)
plt.xlabel("Rain Rate (mm/hr)")
plt.ylabel("Rain Attenuation (dB/km)")
plt.title("Rain Attenuation in FSO Link")
plt.grid()
plt.show()

# 3. Wavelength impact
lambda_range = np.linspace(800, 1600, 100)
tau_lambda = []

for lam in lambda_range:
    q = kruse_q(2)  # fixed low visibility (fog)
    alpha_l = (3.91 / 2) * ((lam / lambda0) ** (-q))
    tau_l = 4.343 * alpha_l * L
    tau_lambda.append(tau_l)

plt.figure()
plt.plot(lambda_range, tau_lambda)
plt.xlabel("Wavelength (nm)")
plt.ylabel("Attenuation (dB)")
plt.title("Attenuation vs Wavelength (Fog Condition)")
plt.grid()
plt.show()
```

## Output
 
```text
FSO Communication Results
-------------------------
Distance (m) | Received Power (µW) | Loss (dB)
          1 |               860.71 |     0.65
          2 |               740.82 |     1.30
          3 |               637.63 |     1.95
          4 |               548.81 |     2.61
          5 |               472.37 |     3.26
          6 |               406.57 |     3.91
          7 |               349.94 |     4.56
          8 |               301.00 |     5.21
          9 |               259.24 |     5.86
         10 |               223.13 |     6.51
```

The graph shows that the received optical power decreases exponentially as the distance between the transmitter and receiver increases. The link loss increases with distance, demonstrating the effect of atmospheric/path attenuation on an FSO communication link.

  
## Result
The free-space optical communication link was successfully implemented and analyzed using Python. The received optical power decreased exponentially with increasing transmission distance, while the link loss increased correspondingly. The results demonstrate that distance and proper alignment significantly affect FSO link performance, and reliable indoor communication can be achieved over short distances under suitable conditions.

