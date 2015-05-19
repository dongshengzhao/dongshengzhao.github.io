---
layout: post
title: Introduction To Usually Used Softwares
description: "Introduction To Usually Used Softwares"
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



