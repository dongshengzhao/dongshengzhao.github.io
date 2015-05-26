---
layout: post
title: Introduction To Usually Used Softwares in GNSS
description: "Introduction To Usually Used Softwares in GNSS"
modified: 2015-05-19
tags: [GNSS]
image:
  feature: abstract-4.jpg
  <!-- credit: dargadgetz
  creditli: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/ -->
---
<!-- [top1](#1) -->
>GNSS, as a typical engineering discipline, needs computer programming to support the data processing, analyzing or even generating. Fortunately, most of these work can be done by adopting the exited softwares. In this blog, several useful and usually used softwares are listed and brief introduced. Most importantly, the learning methods and some useful comments are given for beginners. 

<!-- more -->
<hr>
<p id="para1">Teqc</p>
<!-- <p id='1'></p> -->
Teqc is a comprehensive toolkit for solving many problems when preprocessing GNSS data. Basically, it has three main functions.

1. translate the native binary file to RINEX
2. modify RINEX files, e.g. connect several files together
3. quality checking of GPS and/or GLONASS data, which is the most significant part of this software

[UNAVCO](https://www.unavco.org/) provides a very good tutorial for this software. which can be separated into two ways, [online tutorial](https://www.unavco.org/software/data-processing/teqc/tutorial/tutorial.html) and a PDF file, which can be download from [here](/files/UNAVCO_Teqc_Tutorial.pdf) for the version June 6, 2014. The newest version can be download from the website of [UNAVCO](https://www.unavco.org/software/data-processing/teqc/tutorial/tutorial.html). I suggest the beginners to choose the PDF for starting, since it is good written and easy to understand. UNAVCO suggests the learning process should be the following:

>New teqc users should, at the minimum, review sections 1 through 4, and other sections which describe the particular processing you need. 

But for me, I do not think this is a good suggestion. Actually, it is not a very complex software, so two or three days would be enough for learning this software. However if you do want some suggestions, the following order might be in your consideration.

>Section 1 to 3 --> section 7  --> section 4, 5, 6, 8

###*Some comments for some sections*

#### Section 2 Installing Teqc

How to set teqc as a command that can be used at any directory.

1. using command **cd** to set current working directory to the root file.
2. **subl .bash_profile** to use Sublime Text software to open .bash_profile. The following information may be found in the file

{% highlight python %}
PATH="/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"
export PATH
{% endhighlight %}

	Make some changes as following:
{% highlight python %}
PATH="/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"
alias teqc=/Users/zhaodongsheng/Study/software/teqc
export PATH teqc
{% endhighlight %}

Then teqc becomes a command that can be used in any directory.

#### Section 3

**P9**
{% highlight python %}
& teqc +qc inputfile 2> /dev/null
{% endhighlight %}
The symbols *>* and *2>* are Linux and UNIX output redirection commands. *>* means sending stdout (standard output) to a file. *2>* means sending stderr (standard error) into a file. */dev/null* means a null file or a bin.

{% highlight python %}
& teqc +qcq +sym inputfile > outfile 2>&1
{% endhighlight %}
*2>&1* means sending stderr into the same file as used before, which is the outfile here.

{% highlight python %}
& teqc  -O.dec 60 BCH164.12o > BCH164_decim60s.12o
{% endhighlight %}
Pay attention to the *-O.dec*, where the O is a capital letter.

**P10**

In chapter 3.1.4 splicing, what is the reason that the writer only mentioned the RINEX nav files? Why not other files? From my own perspective, it would not make sense for other files. However I am not sure about this. Any comments are welcome about this.

**P12**

In UNIX, the following ways can be used to represent a list of files:

- SITE*.12o
- SITE[0-120]0.12o
- STA9120[A-X].12o

This is particular important for GPS files, since usually GPS files and named similarly. 

#### Section 4 

>There is a mnemonic governing the effect of the ­ or + preceding each option:
>leading +: output something (besides stdout and/or stderr), or turn on some option. 
>leading ­-: input something (besides stdin or a target file list), or turn off some option.

#### Section 7

This is a very important chapter. I suggest to learn this chatter once finishing the second chapter.

>Teqc quality checking computes several measures of quality of the input data and specifies problems with GNSS observables stored in GNSS observable files. Teqc quality checks GNSS signals and receiver behavior using data from most GNSS receivers. 

>Teqc can create files with values of SV sky paths (azimuth and elevation) as seen from the receiver, site multipath combinations, receiver signal to noise ratios, and ionosphere delays.

>You can change the time scale (duration of time covered with qc processing ) and time bin duration with teqc qc options.
Here comes a question, how to change the time scale? It seems that they do not provide the command for this? I am not sure about this.

**RMS** means Root Mean Square or 二次方的平均值 in Chinese, which can be expressed in the following formula.

$$\color{blue}{x_{rms}=\sqrt{\frac{1}{n}(x_1^2+x_2^2+\cdot\cdot\cdot+x_n^2)}}$$

**P42**

Here is a question: how to view .azi  and .sn5 data graphically?

**P44**

This following is a very good explanation for Ionospheric delay:

>Ionospheric delay is a significant source of error in processing GNSS signals. Ionospheric conditions are variable and affect the speed of GNSS radio signals, changing apparent ranges to satellites. The shift in range due to this effect is called ionospheric delay and is measured in meters. It is frequency­ dependent and so effects the L1 and L2 signals by different amounts. The derivative of ionospheric delay is simply a time rate of change of the ionospheric delay.

>The size of the ionospheric delay depends on the latitude of the receiver, the elevation of the satellite in view at the time of observation, the season, the time of day, and the level of solar activity. Delay from satellites overhead can be several tens of meters. The elevation angle to the satellite is quite significant since delay increases with lower elevations; up to about five times greater near the horizon than overhead. This is largely due to the longer signal path through the ionosphere. Paths to satellites closer to the horizon (at low elevations) have a slanting line through the ionosphere which of course is longer than a vertical path.

PS: Some words of this part are paste from Teqc tutorial PDF as a reminder for readers. Many thanks to UNAVCO.
<hr>

<head>
<style>

#para1 {
	color: blue;
	text-align: center;
	font-style: italic;
	font-size: 150%;
}

</style>
</head>



