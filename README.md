Download Link: https://assignmentchef.com/product/solved-epfl-task-1-mapping-and-demapping
<br>
We start our <strong>guided project </strong>with a review of the very basic communication system model shown in Fig. 1.1. In the transmitter, a source emits a bit stream (representing the information) which is mapped onto a set of symbols. The symbols are then fed into a channel. The output of the channel (which does not necessarily have to be from the same set of symbols) is then demapped back to bits. The set of symbols and the channel model are chosen to replicate (or at least approximate) the physical behavior of the used channel.

<h1>        1.1    The BSC channel</h1>

Figure 1.2: The binary symmetric channel (BSC) model

The binary symmetric channel (BSC) shown in Fig. 1.2 is one of the easiest to understand channel models in communication theory. The BSC has a discrete binary (and identical) set of input and output symbols. With probability p the channel causes a flip of the transmitted symbol value, otherwise the symbol is transmitted unchanged. Despite its very simplistic nature there are real communication systems (e.g. photons in quantum communication) which can be modeled by it.

Based on student submissions from previous years we have assembled a MATLAB implementation of a BSC simulation. It generates a random bitstream, applies the BSC and calculates the resulting bit error rate (BER).

Listing 1.1: bscpoor.m – Bad BSC Example Code

<ul>

 <li><em>% Start time     measurement</em></li>

 <li><strong>t i c </strong>( ) ;</li>

</ul>

3

<ul>

 <li><em>% Source : Generate random        bits</em></li>

 <li>t x b i t s =          randi ([0          1] ,1000000 ,1);</li>

</ul>

6

7 <em>% Mapping : Bits to symbols </em>8 tx = {};

<ul>

 <li><strong>for </strong>i =1:1000000</li>

 <li><strong>i f </strong>t x b i t s ( i )   == 0</li>

 <li>tx { i } = ’A’ ;</li>

 <li><strong>else</strong></li>

 <li>tx { i } = ’B’ ;</li>

 <li><strong>end</strong></li>

 <li><strong>end</strong></li>

</ul>

16

<ul>

 <li><em>% Channel : Apply BSC</em></li>

 <li>rx = {};</li>

 <li>i = 1000000; 20 <strong>while </strong>i <em>&gt;</em>= 1</li>

</ul>

21    randval      = <strong>rand </strong>( 1 ) ;

22

<ul>

 <li><strong>i f </strong>randval <em>&lt; </em>p</li>

 <li>switch tx { i }</li>

 <li>case ’A’</li>

 <li>rx { i } = ’B’ ;</li>

 <li>case ’B’</li>

 <li>rx { i } = ’A’ ;</li>

 <li><strong>end</strong></li>

 <li><strong>else</strong></li>

 <li>rx { i } = tx { i };</li>

 <li><strong>end</strong></li>

 <li>i =          i − 1;</li>

 <li><strong>end</strong></li>

</ul>

35

<ul>

 <li><em>% Demapping : Symbols           to         bits</em></li>

 <li>r x b i t s =          [ ] ;</li>

 <li><strong>for </strong>i =1:1000000</li>

 <li><strong>i f </strong>rx { i } ==        ’A’</li>

 <li>r x b i t s ( i ) =          0; 41    <strong>else</strong></li>

 <li>r x b i t s ( i ) = 1;</li>

 <li><strong>end</strong></li>

 <li><strong>end</strong></li>

</ul>

1.2: YourTasks-PartI

45

<ul>

 <li><em>% BER: Count errors</em></li>

 <li>e rro rs =          0;</li>

 <li><strong>for </strong>i =1:1000000</li>

 <li><strong>i f </strong>r x b i t s ( i )   ˜=        t x b i t s ( i )</li>

 <li>e rro rs =          e rro rs +          1;</li>

 <li><strong>end</strong></li>

 <li><strong>end</strong></li>

</ul>

53

<ul>

 <li><em>% Output result</em></li>

 <li><strong>disp </strong>( [ ’BER: ’  <strong>num2str </strong>( e rro rs /1000000∗100)     ’%’ ] )</li>

</ul>

56

<ul>

 <li><em>% Stop time     measurement</em></li>

 <li><strong>toc </strong>( )</li>

</ul>

Please note that this is exceptionally <strong>POOR </strong>code and combines many of the mistakes most commonly done in MATLAB programming. You shpuld never hand in such code. Nevertheless the script is valid MATLAB code and you can use it as a reference for the language syntax.

<h1>       1.2    Your Tasks – Part I</h1>

<ol>

 <li>Download the m MATLAB script from the course moodle and measure how long it takes to run. The code already contains the tic and toc functions for measuring the execution time. Reduce the number of bits to 1000 and measure the execution time again.</li>

 <li>Implement a better version of the BSC simulation which is smaller and faster. It should<strong>NOT </strong>contain any loops, cell-arrays, duplicated constants or if statements. Compare the improvement in speed for 1000000 and for 1000 bits. You should be at least 20x faster for the first case.</li>

</ol>

<h2>       1.2.1    Some General MATLAB Advice</h2>

MATLAB (MATrix LABoratory) is a very powerful tool for numeric evaluation. It will be our main tool during the course. While the program is very flexible, it is also very slow for general programming tasks. In order to MATLAB offers a lot of highly optimized built-in routines for vector and matrix operations. Writing your MATLAB code in the right way can save you a lot of time and trouble. The second important goal of good MATLAB code is readability. In many cases simpler MATLAB-aware code also leads to faster execution speed. Unfortunately this is not always the case and sometimes you have to find a trade-off between speed and readability.

We have assembled some general rules which can help you to achieve good code: • Use vector or even matrix operations instead of slow loops.

<ul>

 <li>Write <strong>short </strong>comments in your code in order to document its behaviour. But keep in mind that code which is well structured and easy to read is even the better documentation.</li>

 <li>Use variables for parameters in order to keep the code reconfigurable.</li>

 <li>Give all your variables meaningful names. It can help to stick to a unified naming pattern.</li>

 <li>Use indention and newlines to keep your code readable. MATLAB offers a built-in smart indent function which provides good results.</li>

 <li>Optimize only performance critical parts, for all other parts readability is more important.</li>

</ul>

<h1>1.3  The AWGN channel</h1>

Transmitter                         Channel                            Receiver

Figure 1.3: System model for Lab 1

We now replace the BSC by another very common but a bit more sophisticated channel model. In the additive white gaussian noise (AWGN) channel the symbols are complex numbers which represent the amplitude and phase of the transmitted signal and carry the digital data. In Lab 3, we will have a closer look into the synthesis of the transmitted signal from the data symbols. After transmission over a channel which disturbs the symbols with additive noise, the demapper converts the received noisy symbols back to a bit stream.

In the following, the transmitter and channel blocks are explained in more detail.

<ul>

 <li>Source: Throughout the lab, we use grayscale images as payload data. The pixels are transmitted rowwise, starting with the upper left pixel. The brightness of each pixel is represented by an unsigned 8-bit value, and the <em>least significant bit </em>(LSB) is transmitted first.</li>

 <li>Mapper: The modulation scheme used by our system is <em>quadrature phase shift keying</em>± ± √</li>

</ul>

(QPSK), which means that the bits are pairwise mapped onto symbols <em>a </em>= ( <a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> <em>j</em>)<em>/ </em>2, as shown in Figure 1.4. With this modulation scheme, the information lies solely in the phase, since all symbols have the same amplitude |<em>a</em>| = 1, hence the name <em>Phase Shift Keying</em>.

<ul>

 <li>AWGN channel: In the channel, the transmitted symbols <em>a</em>[<em>n</em>] are disturbed by complex-valued <em>additive white Gaussian noise </em>(AWGN) <em>w</em>[<em>n</em>] with zero mean and variance . (A complex-valued Gaussian distributed random variable <em>w </em>is obtained by taking two independent real-valued Gaussian random variables with half the variance, , and combining them to a complex number <em>w </em>= <em>w</em>1 + <em>jw</em>2.) “White” in this context means that the noise samples are uncorrelated,</li>

</ul>

i.e..

1.4: YourTasks-PartII

Figure 1.4: Mapping of bit pairs <em>b</em>0<em>b</em>1 to QPSK symbols

Figure 1.5: BER for uncoded QPSK modulation, AWGN channel

The received symbols are then given as

<em>r</em>[<em>n</em>] = <em>a</em>[<em>n</em>] + <em>w</em>[<em>n</em>]<em>.                                     </em>(1.1)

An important measure in communication systems is the ratio of average signal power to average noise power (<em>signal-to-noise ratio</em>, SNR), usually expressed in decibel:

SNR[dB] = 10 log<em>.                    </em>(1.2)

Since the power of the data symbols is normalized to 1 (|<em>a</em>[<em>n</em>]|<sup>2 </sup>≡ 1), the SNR of our system is . The best achievable <em>bit error rate </em>(BER) for an uncoded QPSK modulation is plotted in Figure 1.5 as a function of the SNR.

Figure 1.6: Alternative Mapping of bit pairs <em>b</em>0<em>b</em>1 to QPSK symbols

<h1>1.4  Your Tasks – Part II</h1>

<ol>

 <li>You are given a transmitted signal, as well as the width and height of the transmitted image. The signal can be loaded into the Matlab workspace with the command load &lt;filename&gt;.</li>

</ol>

Your task is to generate a noisy received signal by adding, dependent on a SNR value in dB, correctly scaled noise to the loaded signal.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> In a second step demap the received symbols into bits and display the received image with the provided function imagedecoder.

Display the images using different SNR values, so that you get an impression of how badly the pictures are disturbed under different channel conditions.

<ol start="2">

 <li>Adapt your code in order to plot the received symbols as clouds in the complex plane.Compare plots of the noisy constellations of the received signal for different channel conditions.</li>

 <li>Now write a function that experimentally reproduces the BER plot from Figure 1.5. Createa random bit sequence, map it onto symbols according to Figure 1.4, and add scaled noise. Demap the noisy symbols to bits and determine the bit error rate. Repeat these steps for different SNR values and then create a BER plot with the function semilogy (in order to get a logarithmic scale on the <em>y</em>-axis).</li>

 <li>Finally, create another BER plot where you use the symbol mapping as shown in Figure 1.6. Compare the two plots. Which mapping scheme is better, and why?</li>

 <li>On the moodle you can download a MATLAB function compresseddecoder which is an alternative image decoder based on lossy image compression. Unfortunately the script was programmed by a lazy PhD student and is therefor full of errors. Your mission is to hunt down all bugs.</li>

</ol>

<strong>Hint: </strong>Don’t try to understand the functionality of the code but use the debugger to track development and shape of the variables to identify odd behaviour.

1.4: YourTasks-PartII

<h2>       1.4.1    Plotting in MATLAB</h2>

Guidelines for plotting graphs:

<ul>

 <li><strong>Plot the BER in a logarithmic scale (</strong>semilogy<strong>).</strong></li>

 <li>Make sure that the ranges of the axis are meaningful.</li>

 <li>If you have multiple curves in one plot, assign different colors and or markers to it.</li>

 <li>Always label the axis (xlabel/ylabel).</li>

 <li>Assure that each plot has a meaningful legend. (legend)</li>

</ul>

Use the MATLAB function for export of pictures (print) NOT a screenshot

<a href="#_ftnref1" name="_ftn1">[1]</a> The notation <em><sub>x </sub></em>∼N(<em><sub>µ,σ</sub></em><sup>2</sup>) means that the random variable <em><sub>x </sub></em>is Gaussian distributed with mean <em><sub>µ </sub></em>and variance <em>σ</em><sup>2</sup>, i.e. the <em>probability density function </em>(PDF) is.

<a href="#_ftnref2" name="_ftn2">[2]</a> In later chapters, the signals that you get will already contain the impairments introduced by the channel, e.g., a timing or phase error. However, you will always have to add the noise yourself, so that you can simulate your system at arbitrary SNR values.