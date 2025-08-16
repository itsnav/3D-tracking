# Youtube Transcript of https://www.youtube.com/watch?v=xx85eyN1Xc0

00:00:00
In this tutorial, I'm going to show you how you can 3D track your own footage fully automated using only free and open- source software. This workflow is 100% hands-off. You simply place your footage in a folder, you run the script, wait for a few minutes or hours, and eventually you will most likely get a fully automated precise reconstruction of how the camera moved in 3D space when the footage was recorded. And all of this is done without placing a single manual tracker, without specifying any


00:00:30
camera settings like focal length, aspect ratio, anything like that. You can just dump all your video files in the folder and it just works or 90% of the time it works every time. Okay, so the trick here is to use photoggramometry. You know when you take a bunch of pictures of an object from multiple angles and then there's an algorithm that reconstructs the footage into a 3D model. Turns out this algorithm is capable of some insanely precise estimates for where the camera must have been in 3D space when each


00:01:01
picture was taken. So our workflow today is kind of simple actually. We're just going to convert a video file to an image sequence. Then we're going to run all those images through the photoggramometry software. So it creates this animated camera that goes through the exact locations of all the photoggramometry cameras. This is such a game changer if you're doing visual effects and stuff like that. It's such a cool workflow. So, here is a video clip I shot earlier this summer. It has a lot


00:01:24
of camera shake, but there are enough details there that I think we should be able to track this with a fully automated setup. Let me show you how to do this. So, in this workflow, I have prepared some links, and you will find all these links in the description of this video. And what's really cool about this is that everything is 100% free and open source. So, the first piece of software we're going to download is called Cole Map. This is doing most of the heavy lifting here because this is


00:01:46
the photoggramometry algorithm. So if you scroll down a little bit on this site to the right here you can see you have releases and here you can see that coal map comes in two versions. So if you have an Nvidia GPU you can use the CUDA version. If you don't have an Nvidia GPU don't worry it will still work but you will using your CPU instead. So then you will click no CUDA. So I'm going to download the CUDA version to use it with my Nvidia GPU. So now let's close this. And here you can


00:02:08
see we have a zip file. I'm going to right click seven zip. I'm just going to extract here. So now here you can see we have call map. So if you want to, you can double click on this and you can see how the graphical user interface looks. But we're not going to be using this today. We're going to using a script I prepared which is a lot easier to use. But now we're running into a problem because Cole Map does not support video files. It only supports still images. So we need to convert this video file into


00:02:32
an image sequence. So to automate this, let's head to the second link which is ffmpeg.org. So let's go to download. And for Windows, I like to use the builds from gon.dev. And we're looking for a static build. So let's scroll down here. So under release builds, we're going to download ffmpeg release essentials. This one. So now we have another zip file here. I'm going to right click on it, seven zip, and I'm going to extract it here. Okay. So now we have Cole Map to do the photoggramometry. And we have


00:02:58
ffmpeg, which is the command line tool to convert our video file to still images. Now we just need the script. So I'm going to go to the third link, which is the batch reconstruction script that I put together. So if you go to the link, you can click this raw button here. And now you can see that this website ends with.bat. So that means that if you simply hold your mouse here, you can press Ctrl S to download this. And if you go to your desktop, you can just download this as batch reconstruct.bat.


00:03:25
And if you go save as file type, I'm going to set it to all files. And then click save. Okay. So now that we have downloaded all the ingredients to run this fully automated tracking workflow, let's create the folder structure. So let's right click. Let's make a new folder. I'm going to call this automated tracker. And in this folder, we're going to create five folders. You can use control shiftn to make a new folder. The first folder needs to be called 01 call map. It's important that you write this


00:03:50
exactly like I do because in this batch file, it has requirements that these folder names are exactly like this. So in this first folder, we're going to take everything that was inside the colap zip file and we're going to place it in there like this. And then we're going to make a new folder. Control shift n. And we're going to call this 02 videos. And in this folder, we're going to place our video file like this. And then the third folder is going to be called 03 ffmpeg. And in this folder,


00:04:19
we're going to place our ffmpeg files. So I'm going to open this and we're going to take everything like this and pull it into the ffmpeg folder. And then the fourth folder 04 is going to be called scenes. And we don't need to put anything here because this is basically output folder where we get every reconstructed scene. And then the final folder which is 05 script. In here we will place our batch reconstruct.bat script. And that's it. Now we can just get rid of everything else. And in our


00:04:49
automated tracker folder we now have call map videos ffmpeg scenes script. And you can confirm this by reading the instructions in the batch file. Here you can see here's also the folder structure. Now if we go in the script folder, we can simply double click on the batch reconstruct file and it will start working on every video file that we have in the folder here. Oh yeah, the first time you try to do this, Windows will tell you like, oh, are you sure you want to trust this script? To that I say


00:05:15
yes, you should trust it. But if you're worried, you can simply copy the script and paste it into chatbt for example and just ask is this safe and it will tell you this is safe. So now you can see it opens a terminal window here and you can see that it's extracting all the frames. It's basically taking the video and turning it into images. So if you go into scenes now we will have a handheld folder because that was the name of the video file. And here you can see we have a bunch of images. And right here now it's


00:05:42
going through every single frame and it's looking for features. Look at that. It's finding 12,000 features in every image. So this is pretty crazy. Okay. But this process is actually going to take quite a while. So, I'm not going to sit here and wait for it. Spoiler, I've actually already tracked this. So, I'm going to close this and I'm going to show you what the result looks like. Okay. So, once the reconstruction is finished, you can go to the scenes file and you can find the folder with the


00:06:06
name of your video file, handheld. And then here you can see you have images, sparse, and then you have a database db. How do you import this to blend? Of course, someone has made an add-on, and it's absolutely amazing. So, in the last link, we're going to go to github.com/sbcv. This guy has made an amazing Blender add-on. Thank you so much. Where you can import basically all the photoggramometry data we need. So, let's go down to releases here and let's download this. It says it's compatible


00:06:34
with Blender 4.0.2, but I've tested this with Blender 4.4 and 4.5 and it works. You just need to not use Vulcan. You need to use OpenGL. So, let's download this photoggramometry importer. I'm going to save it to my desktop and let's open up Blender. So, here we are in London version 4.5 and I'm going to just delete everything in my scene. Let me enable my screencast keys here so you can see my hotkeys. And let's go edit preferences and under add-ons, we can click this little drop down arrow here


00:07:02
and let's go install from disk. And on my desktop, I'm going to click the photoggramometry importer. Don't unzip this first. You are just installing the Blender add-on as a zip file. So now let's click install from disk. And look at that. Now we have the photoggramometry import export add-on. This is very powerful. So now to import the reconstructed scene with a 3D track and everything. Let's go file import. And now you have a lot of options here. I'm going to select call mapap or


00:07:27
model/workspace. This one. And then you go to our desktop and let's find the automated tracker folder. And in the scenes folder, you can find the clip that you have tracked. This one handheld. You don't need to select anything here. You can simply click import. But before you click import, I highly recommend that you click suppress distortion warnings. Or else we're going to get a ton of warnings that we don't need anyways. So now let's just click import call map model. And this is going


00:07:49
to take a while. Maybe 10 or 20 seconds. Oh, there it is. Look at that. That is amazing. So if you can't see the point cloud here now, that is because you need to go to edit preferences and under system, you make sure that the back end is set to OpenGL because Vulcan doesn't work. Look at that. This is so cool. Now you can see that we have every single camera here. Every frame has its own camera. And what's even cooler is that if you hide all these cameras here, clicking this eye icon, look at that.


00:08:20
There is an animated camera. This camera goes through the position of all the still images. So if we select this animated camera, you can go view cameras, set active object as camera. Look at this. If you zoom in here, you already have the image texture set up as the background texture. So now you can scrub through this and you can see that this track is rock solid. Look at how cool it looks with the point cloud just on top of that. Very, very nice. And we can zoom out there to see more of this.


00:08:51
So here you can see our entire clip. It's really long. It's really shaky, but the track is rock solid. It's very, very cool. And also, you can disable the overlays so we only see the point cloud. Look at how cool that is. That's so trippy. You can even see the car tires. Look at that. You can see like the patterns of this manhole cover. So, if I enable the video and disable the point cloud, it's these square patterns and then the point cloud lines up like that. But you might notice


00:09:24
if you try to press play that this video is lagging quite a lot, especially if you go to somewhere you haven't been before. It's very, very slow. So, to make the video play a lot faster, we can create a proxy file, which is super easy. So, if you right click on this line, you can do horizontal split. Let's go up here and let's set this editor type to movie clip editor. And then in this dropown menu here, you just select your footage, which is probably called frame 001 or something. Import this. And


00:09:50
here the colors will look different. But don't worry, we don't need to use these colors. And then you can click set scene frames because sometimes the frames aren't correct. And look at this. We can create our own proxy video from this window, which is super easy. So if you enable proxy time code here, we can build original. For example, if you set it to 50%, and you disable 25%, and then I want to do a proxy custom directory. You don't need to do this, but I'm just going to save this to my desktop. And


00:10:16
then let's just click build proxy. So, now what's happening is that it's taking all these images and it's just saving them in a lower resolution so our video will play a lot smoother. So, if it still doesn't play smooth, you can always try and do a 25% build instead. So, now let's close this by just right clicking and going join down. And let's select our animated camera here and let's go to the object data properties and let's open up the background images. And here you can see you can set the


00:10:40
proxy render size to 50%. So now when you press play, look at that rock solid 30 frames per second basically all the time. Now you can scrub through this super smooth. If you're wondering how good was this track, I need to tell you a little secret. Let me show you something. I have this other project here. This one. This is the video clip that you just saw in the beginning of this video. If you look at this video file, do you notice anything special? I've added an element here, but it's


00:11:07
impossible to see because the track is so good. I didn't even do any shading or anything. It's just a 2D projected image and it just lines up very nicely. So, look at this car. It was possible to see the license plate of this car just really subtly through this uh fence here. So, I was like, I should probably blur that. But then I was like, wait, I already have the 3D track. So if we zoom in here, you can see if I disable the point cloud. Look at that. There is this just stupidly simple plank thing that


00:11:37
you can see that looks like this. It just has the texture of that actual plank there. So now if I take this plank, I can just press G and I can move it and it literally placed in 3D space. So if I fast forward here, I just placed it exactly where the fence was and it didn't need any sort of editing. It's just perfectly tracked. And I didn't even render this with rate tracing. I literally just disabled origins, I think. Yeah. And also these axis. And I just did a simple viewport render image.


00:12:07
Since there's so much shake and the motion just fits, it's almost impossible to see. Okay, let me show you some other tracks I've done. I've actually tracked a ton of video files. Look at these. So, I'm going to go F4, import call map. Oh, yeah, this one. This is a 44 second track. I'm going to click suppress distortion warnings. I'm going to import this. Yeah, this is a very, very big one. So, it's probably going to take a while to import this. Oh, there it is. That took a while to import. Look at


00:12:39
that. That is a 360° walking around this field here. But one thing you might notice if you're trying to get a better look at this is that it's a little bit difficult to navigate in Blender when things are rotated like this. So, what I recommend that you do is you can go to edit preferences and under navigation, you can change it from turntable to track ball. So, I don't know how to explain this, but it just behaves differently. Look at that. So, now you can kind of rotate around like


00:13:05
this. Okay, look at this. Here you can see you have this camera is moving around. You can even see how like the camera is wiggling a little bit because I was walking. Look, if we disable these camera is kind of such a cool movement. It's so detailed. Okay, let me show you the shot here. So, if we view this from the camera view, look at this. This is a 360 degree track and it just worked on the first try. By the way, in the object ed properties under background images, you can uh increase the opacity. So, it has a


00:13:37
little bit more contrast. So, now if you walk around, you can see you have this long shot here. But if you enable the point cloud, it is rock solid. This is such an amazing track. That is phenomenal that that worked. It is a 44 second video clip in 4K and it managed to do it. That is crazy. Okay, let me show you another one. Oh yeah, this one here. I accidentally pressed record actually, but I just tried to 3D track it anyways. Look at this crazy movement there of the camera. And this is even a vertical shot as


00:14:12
well. So look at this. I'm kind of walking around just looking at my shadow, just mindlessly moving my phone around. I didn't expect this to track. I wasn't deliberately filming to make sure it was tracked. You know what I mean? But it just worked. Anyways, it's very cool. I'm so impressed by this that it managed to track this. It's crazy. Oh, I need to show you one more. This is probably the most impressive one. Look at this shot here. If we fast forward a little bit, it has moving vehicles. Look


00:14:50
at this huge moving objects just moving by. The software just figures out what is the camera movement. Usually, you need to mask out that kind of stuff. And this big car here as well. Let's import this and see. Oh, and by the way, instead of just clicking the suppress distortion warnings, you can also add points as mesh objects, which will allow you to get mesh objects in your scene instead of just a point cloud. So, you can actually place objects and stuff like that. So, if I enable this, I'm


00:15:16
going to click import. It's very powerful, I think. Oh, look at that. Oh, it's upside down, actually. So, let me Okay, there we go. So, if I set the viewport shading to material preview now look at this. Now we have a geometry node system where every blob here has kind of like the correct color of the ground underneath. So if I view this from the camera now and you disable all the other cameras. Look at that. It is a perfect track. Can zoom out there. Even there's too much stuff in the Yeah, this is harmless but


00:15:46
it's still kind of annoying. Let me see. Can I delete this? If I go to edit mode tab to go to edit mode. Yeah, I think you can just box select and just press X and delete. Yeah. Okay, so the point cloud is still there, but yeah, we did get rid of that stuff. Okay, nice. You can see you have the cars there and the track. It's just no problem. The track just works very nice. It even works to go down like this. Then you have this other vehicle. And if you want, you can select this mesh point cloud and go to modify


00:16:17
properties and you can just lower the point radius here 2 for example or 0.1. Or you can also do this for the point cloud here. If you press N to bring up this uh photoggramometry importer menu, select the point cloud. You can tweak the size here. So you set it to 15. Now you have these huge blobs like this. Or you can lower it. Oh, hang on. Let me show you a very cool effect. Since there are so many gray points, they kind of disappear. But if you set the viewport shading background to custom, set this


00:16:50
to black. Look at how cool that looks. You can see the grass here and there's all the details and it actually lines up with the video. It is crazy. Okay, so now that we have our uh mesh point cloud here, let me show you something very cool. If you select this point cloud and you press tab to go to edit mode. If you just zoom in, you can select a few points. Maybe like this. Yeah. If you set it to wireframe, you can see that now you have, for example, this point that looks like it's in the


00:17:20
center there. Right? So then you can basically expect that that is exactly where the asphalt is. So if you press shift S and go cursor to selected, if you press tab to go back to object mode, now you can hide the mesh point cloud again. And you can go shift a mesh plane. And this plane if you rotate it on the x- axis or y. Now you can see that this plane is perfectly placed in the middle of the scene. And you could start doing your visual effect shots with making this into a shadow catcher for example. As you can see, if you


00:17:51
disable the point cloud, this is rock solid on the ground here. It's actually crazy. Yeah. If we go down here, you can see it's perfectly lining up. So yeah, that is the fully automated 3D tracking workflow. And just a quick heads up before you go out and start recording your own footage, it is very important that you disable any kind of image stabilization in your camera. Any optical or digital image stabilization in the video recording will make it extremely difficult to get a successful


00:18:17
3D track. Here you can see in my testing and the difference between stabilized and not stabilized is huge. So if you're recording with your phone to turn off image stabilization, I recommend using the Blackmagic camera app. Not sponsored, by the way. It's just the best camera app. Happens to be free. This app gives you full control over all the cameras in your phone. And you can disable image stabilization completely by just setting it to off like this. I also recommend that you go to settings


00:18:41
and under camera, enable lens correction so you won't have to deal with lens distortion in your footage. So yeah, that's it. I'm very excited to see what you'll create with this. And I just think it's so cool that we finally have a fully automated 100% free and open- source 3D tracking workflow. And if you're into VFX and compositing, don't worry. I'm working on a very powerful compositing tutorial. So, make sure you subscribe if you want to see that. Thanks for watching.


00:19:14
[Music]

