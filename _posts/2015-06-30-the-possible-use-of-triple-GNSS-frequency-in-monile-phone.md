---
layout: post
title: 'The possible use of triple GNSS frequency in mobile phone'
description: "This post gives a brief introduction to the latest research about the use of carrier phase used in mobile phone, as well as serves as the social impact of my research."
modified: 2015-06-30
tags: [GNSS, Application of my research]
image:
  feature: abstract-11.jpg
  <!-- credit: dargadgetz
  creditli: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/ -->
---

>Scientists, mainly including Kenneth M. Pesyna, Jr., Robert W. Heath, Jr., and Todd E. Humphreys, in the Cockrell School of Engineering at The University of Texas at Austin demonstrates for the first time that centimeter accurate positioning is possible based on data sampled (single frequency data) from a smartphone-quality Global Navigation Satellite System   (GNSS) antenna. Their work is believed to have a great potential of social application, which could be the social impact of my research, although my research will not looking at mobile phone. This post aims at summering their work.

<!-- more -->

They conducted this experiment using single-frequency (GPS L1) carrier-phase differential techniques. However they insist that the faster convergence times and improved robustness would obtained when multiple frequencies is involved. The main challenge for introducing centimeter-accurate location into smartphones lies in that its antenna has extremely poor multipath rejection.

When tracking with seven satellites,  the smartphone grade antenna needs around 400 seconds to get 90% successful ambiguity resolution, which is much longer than that of survey-grade antenna and needs to improve later, as higher quality antennas are better combat multi-path. 

They believed that more signals tracked equates to a shorter time to ambiguity resolution. They estimate that if 15 or more satellites cold be tracked using the smartphone, then the time to ambiguity resolution could be brought to under 30 seconds. Multiple-frequencies and multiple-constellations would obviously be necessary to achieve these numbers. 

Receiver motion is another good way to mitigate multi-path and shorten the ambiguity resolution time. The time for the phase residuals decreases from hundreds of seconds, for the static antenna, to much less than a second, for the moving antenna. A shorter residual autocorrelation time, enabled through random antenna motion, effectively increases the
 information content provided to the filter at each measurement epoch, allowing it to more swiftly resolve the underlying phase ambiguities.

 ![Receiver motion](/images/receiver_motion.jpg "Receiver motion")

However for the survey-grade antenna, motion provided little benefit. The reason for this is that there is a trade-off that occurs within the CDGPS filter. While the phase residuals do indeed decorrelate much faster when the antenna is moving, the filter can no longer use the knowledge that the antenna is static throughout data collection, a valuable constraint that it can apply to lower the time to ambiguity resolution. As it turns out, the survey grade antenna is already so good at mitigating multipath that the benefit of decorrelating what little multipath errors there are via antenna motion is approximately offset by no longer being able to assume that the static motion profile is known.

The possible social application of this technique is as follows.
>The researchers' new system could allow unmanned aerial vehicles to deliver packages to a specific spot on a consumer's back porch, enable collision avoidance technologies on cars and allow virtual reality (VR) headsets to be used outdoors. The researchers' new centimeter-accurate GPS coupled with a smartphone camera could be used to quickly build a globally referenced 3-D map of one's surroundings that would greatly expand the radius of a VR game. Currently, VR does not use GPS, which limits its use to indoors and usually a two- to three-foot radius. 

![deliver package](/images/post_pics/150505083031_1_540x360.jpg "deliver package")

The main sources used in this post are available as follows:

K.M. Pesyna, R.W. Heath, and T.E. Humphreys, "Centimeter Positioning with a Smartphone-Quality GNSS Antenna," ION GNSS+, Tampa, FL, September 2014. [*pdf*](http://radionavlab.ae.utexas.edu/images/stories/files/papers/ion2014Pesyna.pdf)

Their presentation [pdf](https://radionavlab.ae.utexas.edu/images/stories/files/presentations/PesynaION2014_pres.pdf) and recording of the presentation:

<iframe width="560" height="315" src="https://www.youtube.com/embed/rCOvklUB5vQ?list=UUsLrWtibSIdQw2FrYMjomIA" frameborder="0" allowfullscreen></iframe>

Some other reports about this research is available in the following:

[http://www.sciencedaily.com/releases/2015/05/150505083031.htm](http://www.sciencedaily.com/releases/2015/05/150505083031.htm)

[http://www.gpsdaily.com/reports/New_centimeter_accurate_GPS_system_could_transform_virtual_reality_and_mobile_devices_999.html](http://www.gpsdaily.com/reports/New_centimeter_accurate_GPS_system_could_transform_virtual_reality_and_mobile_devices_999.html)

[http://www.eurekalert.org/pub_releases/2015-05/uota-ncg050415.php](http://www.eurekalert.org/pub_releases/2015-05/uota-ncg050415.php)

[http://phys.org/news/2015-05-centimeter-accurate-gps-virtual-reality-mobile.html](http://phys.org/news/2015-05-centimeter-accurate-gps-virtual-reality-mobile.html)



