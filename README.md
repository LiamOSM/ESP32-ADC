# ESP32-ADC
ESP32 How to correct the ADC

<h2>It is annoying: The results of the ESP32 Analog to Digital Converter are really bad !</h2><br><br>
See: Original_Output.jpg<br>

<b>Is it possible to get precise results from ESP32-ADC ?</b><br>

Here we build a LookUpTable to correct this behavior<br>

Sense is:

Read value Y and correct it like:<br>
   Y = ADC_LUT[Y] <br>


FOR ME IT WORKS VERY WELL. (See: Correct_with_LUT.jpg)


I estimate the ESP32-DAC produces correct values.<br>

Then I want to build a LUT: <br>

1) Send a value X to DAC
2) Read back value Y from ADC. (pins 25 and 35 are connected)
3) Value Y (0..4095) mostly is too low. We need a LUT (0..4095) to correct the measured value.
4) Correct the value: Y =LUT[Y]
5) Plot the value Y which should match X now. (Attention: Y should be > about 70 !)

Because the DAC uses 8 bit I have to interpolate to get the LookUpTable 0..4095. <br>
But the errors caused by that are much less than the usual noise. <br>


I am really surprised how good the results are. Look at the image. <br>

Here is the program.
How to use: (Arduino_IDE, ESP32)
1) Connect pin25 and pin 35
2) Start the program
3) Start the Serial Plotter
4) Wait some minutes - you will see garbage (from the printout of results)
5) See the corrected values as image (see below). The correction is made for YOUR ESP32.
6) Comment out #define GRAPH   <<<<<<<<<<<<<<<<<<
7) Start the program again
8) Wait some minutes.
9) When the program stops you get 2 arrays to include in your sources:
float ADC_LUT[4096] = { .... with best precision
and
int ADC_LUT[4096] = { ... less precision, but with less memory footprint

In your source:
Y=readAnalog(); Y=ADC_LUT[Y]; (look at the program)<br><br>

ESP32_WITH_MY_LUT.ino is a demo program (to use with Serial Plotter).>br>
It contains MY corrections. If you start it you may see the differences of YOUR ESP32 to mine.<br><br>

Then use it with your own built and replaced ADC_LUT.<br>

