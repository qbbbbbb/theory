# A Bang-Bang Phase-Locked Loop Using Automatic Loop Gain Control and Loop Latency Reduction Techniques
## Introduction

1. loop gain、noise sources and loop latency all decide PLL's output jitter. and the ouput jitter decide the BBPLL gain.

2. TDC power-consuming, and enter a strong nonlinear region.

3. BBPD jitter dependent.

4. A proper loop gain can minimize the output jitter. 

## Loop Latency Reduction of the DPLL
1. Topology A

   - no retiming stage D=0. 
   - The retiming stage placed at DLF output to isolate glitches.
   - The larger the D, the poorer the jitter performance.
   - D = 1, also has a noticeable degration on the jitter performance.

2. Topology B

   - no adder, feed the proportional path and the integral path to a split-control DCO.
   - integral path has a latency.

3. Discrete-time noise model

   - Sequences of random pulses are used to discribe refernce clock noise and quantization noise.
   - the ratio of alpha/beta is much less, sometimes it can be ignored.

4. Topology B realizes the loop latency, provide nearly the same jitter performance as the topology A(D=0)
   
topology A：
![topology A](https://imgur.com/a/uQoLBx6)
topology B：
![topology B](https://imgur.com/a/w0aMvnv)

## Noise Analysis of the DBPLL
 1. [17] take into account only the DCO noise. [21] only reference clock noise.

 2. Closed-Form Expression of the BBPD Gain

    - reference clock phase noise approximated by white noise.
    - different time instants, phase noise of the reference is uncorrelated. So J<sub>PLL,k-1</sub> comprises only the older reference clock noise pulses.
    - the equation reveal:

        when DCO noise, reference clock noise increase, K<sub>BB</sub> decrease.
  ![BBPD gain](https://imgur.com/a/VoWc9h2)
    - the relationship between Δ and K<sub>BB</sub> is nonmonotonic.

        when Δ is small, increasing Δ would increase K<sub>BB</sub> , but when Δ is large, increasing Δ would decrease K<sub>BB</sub>.
    - when DCO noise and reference clock noise increase, loop gain decreases.
    - Δ increases, loop gain increases. loop gain has an upper bound of 4/pi.
    - if Δ is small, DCO noise cannot be suppressed and ignored any more.
    - the equation have a great agreement with the G<sub>sim</sub> no matter which noise(DCO OR REF) dominant.

3. Jitter of the BBPLL
   
   - for given device noise, have an optimal value for beta to achieve the best jitter performance.
   - if DCO noise is large, the overall loop gain should be larger to suppress the DCO noise more. and beta<sub>opt</sub> should be larger.
   - the beta<sub>opt</sub> only depend on the DCO random noise and DCO quant noise, independent of the magnitude of the reference clock noise.
   - the larger reference clock noise, the smaller optimal loop gain, but the beta<sub>opt</sub> need not to decrease.
   - beta<sub>opt</sub> is function of K<sub>T</sub> and DCO random noise. A smaller K<sub>T</sub> or a larger DCO random noise gives rise to a larger beta<sub>opt</sub>. But the beta<sub>opt</sub> is hard to predict.

## Proposed DBPLL With Automatic Loop Gain Control
1. the algc automatic attain the nominal loop gain.
2. Intutive Observation for Loop Gain Control

   - The BBPD and the DCO in the DBPLL are analogous to the quantizer and the integrator in the DSM.
   - The magnitude of the loop gain in the DBPLL also affects the BBPD output(SIGN).
   - If the loop gain is too small, the jitter at the BBPD input will enter a random-noise regime. The jitter mainly results from low-frequency noise. no limit cycle.
   - If the loop gain is too large, the jitter at the BBPD input will enter a limit-cycle regime. SIGN tend to alternate between opposite polarities. Has more high frequency.
   - If the loop gain is near optimal one, enter a stochastic-resonance regime. It means the jitter comes from wide-band noise (nearly white). high frequency and low frequency component is equal.

3. Automatic Loop Gain Control
   
   - AUTO_SUM tends to be positive, negative, or zero when the loop gain is too small, too large or adequate.
   - when β < β<sub>OPT</sub>, β will increase. When β > β<sub>OPT</sub>, β will decrease.
   - the larger reference clock noise indeed deteriorates the output jitter, but it has little impact on β<sub>OPT</sub>.

4. Split-Control DCO
   
   - A larger K<sub>T</sub> provides a larger tuning range for the loop gain, but a larger K<sub>T</sub> increases the contribution from the quantization noise of the ΔΣDAC to the output jitter. 
   - To reduce this quantization noise, KT should be as small as possible. but beta has a upper bound, K<sub>T</sub> also has a minimum.
