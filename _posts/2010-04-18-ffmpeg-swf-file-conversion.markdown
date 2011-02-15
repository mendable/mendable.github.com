--- 
layout: post
title: FFMpeg SWF File Conversion
wordpress_id: 286
wordpress_url: http://www.mendable.com/?p=286
---
FFMpeg has multiple problems and bugs with writing out to SWF files, I run into a few of these at my day-job and wrote a patch for one of the problems and submitted it back to FFMpeg. It was a nice change to go back and do some C programming for a while.

Anyway, list of ffmpeg swf audio problems as follows:
1) Converting to SWF was hard coding the number of audio frames to 6000 in the generated SWF file, whereas the SWF Format specification document from adobe says this must match the number of frames in the file. It was also hard-coding an arbitrary file size into the SWF header as well, rather than the correct file size of the generated file. These are the problems I fixed.

2) FFMpeg SWF encoder writes too many frames to a file. Yeah, really. It writes all of your audio, then a few minutes of empty sound as well. Haven't got a fix for it yet, need to step through an FFMpeg run in GDB to figure out why it's doing it. You can open a SWF in a <a href="http://www.suavetech.com/0xed/0xed.html">binary editor</a> and manually fudge the number of frames in the SWF Header if you have a scrobbler that is dependent upon the number of frames to determine play duration / progress.. or do something a little more automated.

3) The SWF encoder in FFMpeg is writing out audio only streams using version 4 of the SWF file format, where as the file format specification is now up to version 10. Not sure why it's writing out in such an old version (maximum compatibility?). Could probably use some documentation.

This was the patch I submitted for problem #1.
<a href="http://lists.mplayerhq.hu/pipermail/ffmpeg-devel/attachments/20100415/c02bf648/attachment.obj">http://lists.mplayerhq.hu/pipermail/ffmpeg-devel/attachments/20100415/c02bf648/attachment.obj</a>
------
<a href="http://lists.mplayerhq.hu/pipermail/ffmpeg-devel/2010-April/086938.html">On the mailing list:</a>

Problem Description:
When converting any Audio to SWF, the File Size is always hard-coded
as 104857600, and Frame Count is always hard coded to 6000. The
incorrect frame count can cause problems when ffmpeg generated swfs
are used inside a flash application and the _totalframes method is
used.

Example command line:
{% highlight tcsh %}
ffmpeg -i 157346.mp3 -ar 44100 -ab 96k -ac 1 -y 157346.swf
{% endhighlight %}

When inspecting the generated swf using the swfdump tool
(swftools.org), the following is shown:

{% highlight tcsh %}
==== Error: Real Filesize (1495887) doesn't match header Filesize
(104857600) ====
[HEADER]        File version: 4
[HEADER]        File size: 104857600
[HEADER]        Frame rate: 10.000000
[HEADER]        Frame count: 6000
[HEADER]        Movie width: 320.00
[HEADER]        Movie height: 200.00
[02d]         6 SOUNDSTREAMHEAD2
[013]       317 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 1 (00:00:00,000)
[013]       318 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 2 (00:00:00,100)
.........
[013]       318 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 4595 (00:07:39,382)
[013]       317 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 4596 (00:07:39,482)
[000]         0 END
{% endhighlight %}

As you can see, the last frame is 4596, and the file size specified in
the header does not match the real file size (swfdump gives a warning
about this).

File size should indicate 1495887 bytes:
{% highlight tcsh %}
$ ls -al 157346.swf
-rw-r--r-- 1 username default 1495887 Apr 14 11:25 157346.swf
{% endhighlight %}


Problem Solution:
Correctly set the File Size in the SWF header section to the real
number of bytes written, and correctly set the number of frames
included in the SWF.

Details of the SWF file format header information can be confirmed on
page 25 of the SWF File Format Specification document from Adobe,
available here: http://www.adobe.com/devnet/swf/

After my patch is applied, using ffmpeg with the same command line
options as above and then inspecting the generated swf with swfdump
shows:

{% highlight tcsh %}
[HEADER]        File version: 4
[HEADER]        File size: 1495887
[HEADER]        Frame rate: 10.000000
[HEADER]        Frame count: 4596
[HEADER]        Movie width: 320.00
[HEADER]        Movie height: 200.00
[02d]         6 SOUNDSTREAMHEAD2
[013]       317 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 1 (00:00:00,000)
[013]       318 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 2 (00:00:00,100)
.........
[013]       318 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 4595 (00:07:39,382)
[013]       317 SOUNDSTREAMBLOCK
[001]         0 SHOWFRAME 4596 (00:07:39,482)
[000]         0 END
{% endhighlight %}

Note that the Frame Count is correct, and the file size is correctly
reflects the total number of bytes in the file:
{% highlight tcsh %}
$ ls -al 157346.swf
-rw-r--r-- 1 username default 1495887 Apr 15 06:58 157346.swf
{% endhighlight %}
