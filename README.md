# Intro

This details the function and operation of the firmware. Wondering how the impedance measures are made? Want to get phase and magnitude out and play with that? Want to try different electrode stimulation patterns? This is the place to be. Not that there is another branch called gcc-toolchain of this firmware which gives a separate set of installation instructions. 

# Hardware 
This firmware is designed for the Spectra: 32 electrode EIT device, which is also capable of bio-impedance spectroscopy and time series impedance measurements.

<p align="center">
	<img src="images/PCB.jpeg" height="400">
</p>

# Running the installed firmware. 

The device comes pre-programmed with this firmware. Each time you reset the device a set of menu options is communicated via UART or Bluetooth. Once you select an option, press the return key and that option will start executing until you power cycle once more. 

Options Outlines: 
A) Time series - 
   This outputs the impedance magnitude at 40 frames per second at 50kHz(or whatever frequency is set in firmware). 

   How to take a measurement with bioimpedance spectroscopy: A+, V+, V- ,A-

   Example of Use: 
   
<p align="center">
	<img src="images/timeseriesexample.png" height="300">
</p>

B) Bioimpedance Spectroscopy - 
	This outputs impedance magnitudes at the following frequencies, and then repeats the cycle. 
	frequencies = {200,500,800,1000,2000,5000,8000,10000,15000,20000,30000,40000,50000,60000,70000};

	Example of use: 

<p align="center">
	<img src="images/spectrums.png" height="400">
</p>

C) Electrical Impedance Tomography - This is configurable in firmware to 8, 16, 32 electrodes dependent on desired timing and resolution trade offs etc. 

The ordering of electrodes is created using pyEIT as follows: 
ex_mat = eit_scan_lines(ne=8, dist=3)  step = 3
ex_mat = eit_scan_lines(ne=16, dist=8) step = 8
ex_mat = eit_scan_lines(ne=32, dist=16) step = 16
In the file the ordering is:
A+, A-, V+, V-
A, B, M, N
where
    A : current driving electrode
    B : current sink
    M, N : boundary electrodes, where v_diff = v_n - v_m

We have chosen an opposition based electrode patten instead of the more commonly used adjacent scheme, as we came across these papers supporting improved contrast if opposition schemes were used: 
[1]	A. Adler, P. O. Gaggero, and Y. Maimaitijiang, “Adjacent stimulation and measurement patterns considered harmful,” Physiol. Meas., vol. 32, no. 7, pp. 731–744, 2011.
[2]	F. M. Yu, C. N. Huang, F. M. Hsu, and H. Y. Chung, “Pseudo electrodes driven patterns for Electrical Impedance Tomography,” Proc. SICE Annu. Conf., pp. 2890–2894, 2007.

It's possible other electrode arrangement schemes are better yet. 

	Example of use: 

<p align="center">
	<img src="images/picturegrid.png" height="500">
</p>

<p align="center">
	<img src="images/LungscomparedtoCTScan.png" height="500">
</p>


A note on which debugger to use: 
- I recommend using a Segger J-link debugger with SWD programming cable. If you are not using it for commercial use you can get the EDU version at a reasonable price(you don't even have to be part of a school). The configuration on the PCB also allows for the VCOM port to work which means you can also read serial through the same connection. The specific configuration can be seen in the .bat file contained in the EXE directory. 
