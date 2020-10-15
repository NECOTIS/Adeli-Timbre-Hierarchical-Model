# Overview
This is work is a python implementation of the biologically inspired model proposed in :

**A Flexible Bio-inspired Hierarchical Model for Analyzing Musical Timbre**
by *Adeli, M., Rouat, J., Wood, S., Molotchnikoff, S. and Plourde, E.*
This implementation was developed by Etienne Richan based on the original Octave code as part of his master's thesis.

This model is designed to extract representations of sounds that representative of musical timbre.

The model uses a  gaussian filterbank equally spaced on the ERB scale to split a signal into multiple channels. 
Lateral inhibition between neighbouring frequency bands removes redundant spectral information and "sharpens" the spectral profile.
The second stage of model uses a second filterbank to extract the amplitude modulation in each channel and uses this information to produce a smooth
temporal envelope as well as a time-varying profile of perceptual roughness.

# The ERB Scale
The ERB scale is a nonlinear mapping of frequency to units of equal perceptual sensitivity.

In other words, we perceive a 20 hz difference much more easily at 100 Hz than at 2000 Hz. Because our cochlea is a physical system, 
there is a limited resolution to our percetpion of frequencies and a tone at a specific frequency can "mask" our perception of tones at nearby frequencies.
An ERB band is the range of frequencies around a frequency F the will be masked.

# Using the model
We recommend using the ```FullSignalFilterbankWrapper``` class in ```utils/wrapped``` for feature extraction.

# Examples
We provide some example uses of the filterbank in the ```example``` folder

### Feature extraction
The model can be used to extract features for MIR or machine learning purposes.

The three main features provided by the filterbank are :
* A perceptually scaled spectral envelope
* A smooth temporal envelope
* A time-varying roughness profile

Additional features :
* Statistical momements of the spectral envelope
* Amplitude modulation band energies
* Envelopes of filterbank channels


![Cochleogram](https://github.com/NECOTIS/Adeli-Timbre-Hierarchical-Model/raw/master/readme_images/bass_notes.png)

*Example first stage output for a series of ascending bass notes*


## Multi-channel audio exporting
```example/export_filterbank_channels.py```

Each channel output by first stage of the model is a signal containing the isolated spectral content of that ERB band. 
These channels can be exported to a multi-channel audio file. We recommend using audacity to listen to isolated channels.
The combined signal will be distorted, due to the overlaping of the ERB band filters.

## Stabilized auditory image
To better understand this form of visualization, we recommend reading Chapter 21 of Richard Lyon's book [Human and Machine Hearing](http://dicklyon.com/Lyon_Hearing_book_companion_color.pdf).

ERB band channels are extracted on windowed segments of a source signal. 
A simplified form of autocorrelation is performed on each band, and a 2D image is produced, with ERB bands as the vertical scale
and time-delay as the horizontal scale. These frames can be combined into a video, providing a responsive visualisation of 
spectral content and harmonic structure.

![Stabilized auditory image](https://github.com/NECOTIS/Adeli-Timbre-Hierarchical-Model/raw/master/readme_images/SAI.gif)


# Implementation details
We recommend consulting the original paper on the filterbank.

[IEEE link](http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7407352)

It can also be found on [this page](https://www.gel.usherbrooke.ca/rouat/publications/publiDeJRouat.html).

# References and acknowledgements

This work is a reimplementation of the timbre model proposed by Adeli et al.
```
@article{adeli_flexible_2016,
	title = {A {Flexible} {Bio}-{Inspired} {Hierarchical} {Model} for {Analyzing} {Musical} {Timbre}},
	volume = {24},
	issn = {2329-9290},
	doi = {10.1109/TASLP.2016.2530405},
	number = {5},
	journal = {IEEE/ACM Transactions on Audio, Speech, and Language Processing},
	author = {Adeli, M. and Rouat, J. and Wood, S. and Molotchnikoff, S. and Plourde, E.},
	month = may,
	year = {2016},
	pages = {875--889},
}
```

We use a modified (and less optimized) version [Thomas Grill's implementation](https://github.com/grrrr/nsgt) of the Non-Stationary Discrete Gabor Transform
```
Thomas Grill, 2011-2017
http://grrrr.org/nsgt
Austrian Research Institute for Artificial Intelligence (OFAI)
AudioMiner project, supported by Vienna Science and Technology Fund (WWTF)
covered by the Artistic License 2.0
http://www.perlfoundation.org/artistic_license_2_0
```
