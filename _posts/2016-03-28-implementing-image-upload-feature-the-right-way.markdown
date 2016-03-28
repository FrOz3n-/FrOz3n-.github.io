---
published: true
title: Implementing image upload feature - the right way
layout: post
---
You’ve read the title and you’re saying to yourself, I’ve done this a thousand times, what could I’ve possibly done wrong? Well, some of you may have gotten it right from the first time, and the others, just as I did it - wrong. Let’s say you’ve made an image uploading feature on your project(s) and it just works fine. What you basically did? - Get the image content in bytes and save it on your server side/blob storage or any other possible storage unit. 
“_Yeah, that does the job._” - At least that’s what I thought so, until one of the clients uploaded an image taken with their iPhone. Right, not a big deal, nevertheless they took it with the phone in upside down position (rotated 180°) and then uploaded it to the WebApp. And this is where it gets interesting. The photo on their phone looks perfect, rotated according to the current phone’s orientation, but when they saw it uploaded on the WebApp, it was rotated for 180° basically upside down. Then as you can imagine, they reported a bug saying that our system is rotating their images, and when I saw that I just thought “_I’ve never written any code that is rotating images!_”, and yes I was right. 

Let’s state the problem
----------------------------------
When uploading images downloaded from the internet or re-worked, they most of the time, have their [EXIF metadata](https://en.wikipedia.org/wiki/Exchangeable_image_file_format) removed, and everything works just fine, BUT when uploaded directly from the device (camera/phone) they keep their headers, and later on when processed through .NET’s class [Image](https://msdn.microsoft.com/en-us/library/system.drawing.image(v=vs.110).aspx), the EXIF metadata get’s applied to the image’s bytes. What really happens is that if your device set’s the orientation metadata attribute later it gets applied on your image before saving on your storage service, and when retrieved, it is rotated. 

Let’s see how I was doing it wrong:
<script src="https://gist.github.com/ice-j/8e68717d6b315988e890.js"></script>

What did I do to remove the bug?
-------------------------------------------------

   Well, I’ve googled it (as any developer would have :]), and I came across this good [EXIF Extractor lib](http://www.codeproject.com/Articles/11305/EXIFextractor-library-to-extract-EXIF-information). It helped me extract the image’s metadata and apply any orientation in the inverse direction (if existing) and return the rotation metadata to the way I want it to be. You can either do that, or remove any EXIF metadata attributes there are. It’s your choice, but I’ve decided to keep them.


Final result:
<script src="https://gist.github.com/ice-j/977b6c799276dfd4c842.js"></script>

Talk is cheap, show me the code!
-------------------------------------------------

If you'd rather fork or pull the whole example you can do that [here](https://github.com/ice-j/SimpleImageUploader).

Hope I saved someone’s precious time, helped them learn something new, or have a good laugh :) 
Anyway, it was a good experience for me. - Ice.
