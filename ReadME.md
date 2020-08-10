# Manual+ Focus Mode
This repo includes text tutorials on how to edit *almost* **any Minecraft: Java Edition GLSL Shaderpack, that has DOF.**<br>
Just open `.md` file of your preferred shaderpack and you can start adding `Manual+ Focus Mode`.

## What does each Focus Mode mean?
* `Auto` will focus at the object closest to the camera center.
  
* `Manual` will focus at the distance set by `Manual Focus Distance` Setting.
  
* `Manual+` will focus at the distance set by `Manual Focus Distance` Setting, and that distance is multiplied by `Brightness` setting in Video Settings. 100% of Brightness will multiply distance by 1; 50% of Brightness will multiply distance by 0.5, meaning it will cut distance in half.
## How does "Manual+ Focus Mode" work?
Basically, almost every shaderpack uses [`centerDepthSmooth` uniform](https://github.com/sp614x/optifine/blob/master/OptiFineDoc/doc/shaders.txt#L175) as focus point, that means it makes Depthmap based on where is your crosshair aiming at<br>or by sampling `depthtex0` (like in Chocapic13 V8 Ultra).
Based on that, we can also create our own focus point with this function:
```glsl
( (far * (x - near)) / (x * (far - near))
```
It's basically gonna replicate `centerDepthSmooth` uniform, but we can set the focus distance (in meters) with `x`.<br>
*Some shaderpacks already do these like for example Continuum 2.0/2.1 and Chocapic V8.1 (Atleast in Ultra and High versions).*<br><br>
But the problem is that everytime we wanna change focus distance, we gotta go to Shader Settings and set it there, not only that but it will also recompile the shaderpack each time we click Done, which can slow down things a lot.<br><br>
My solution to this is using [`screenBrightness` uniform](https://github.com/sp614x/optifine/blob/master/OptiFineDoc/doc/shaders.txt#L173).<br>If we use that uniform to multiply focus distance, we can change focus distance just by using slider or [McHorse's Aperture Mod's](https://www.curseforge.com/minecraft/mc-mods/aperture) Keyframe Feature (Feature is currently still only for Developer Build Testers).
Function would look like this:
```glsl
( (far * ( [ x * screenBrightness ] - near)) / ( [ x * screenBrightness ] * (far - near))
```
While `x` is now gonna become **maximum focus distance**, because `screenBrightness` uniform ranges from `0-1`, in-game it's shown in percentages, so 100% equals to 1.<br><br>
So if we add this stuff, you can now adjust focus distance more easily and don't have to recompile each time we adjust it. This will save time for both Screenshot Takers and Machinima Creators, who wanna utilize the power of focus.
## Are there any side effects of using `screenBrightness` uniform?
There are some shaderpacks, which already utilize `screenBrightness` uniform for some elements, like for example brightness of dark areas (like in BSL v7.1.04.1). Fortunately, it's very easy to get rid of them.
## How would I go on actually adding "Manual+ Focus Mode" to my/other shaderpack/s?
### **NOTE: This is just for other Minecraft Shader Developers, if there's your preferred shaderpack in Repo, use that instead.**
If you have DOF and use `centerDepthSmooth` uniform (as almost everyone does),<br> then it's really about replacing that with a function, or rather,<br>adding an option between both (or three, if you count Manual) modes of Auto and Manual+.
<br> <br>
You can start by adding this piece of code at the start of (or before you use `centerDepthSmooth` uniform) DOF function.
```glsl
    #define getDepthExp(x) ( (far * (x - near)) / (x * (far - near)) )
    #define DOF_MODE 0 // [0 1 2]
    #define DOF_MANUAL_FOCUS_DISTANCE 5 // [0.1 0.2 0.3 0.4 0.5 0.7 0.9 1 1.1 1.2 1.3 1.4 1.5 1.6 1.8 1.9 2 2.1 2.2 2.3 2.4 2.5 2.6 2.7 2.8 2.9 3 4 5 6 7 8 9 10 12 14 16 24 32 40 48 56 64 72 80 88 96 104 112 120 128 136 144 152 160 168 176 184 192 200 208 216 224 232 240 248 256]

    float focus;
    #if DOF_MODE == 0
    focus = centerDepthSmooth;
    #endif
    #if DOF_MODE == 1
    focus = getDepthExp(DOF_MANUAL_FOCUS_DISTANCE);
    #endif
    #if DOF_MODE == 2
    focus = getDepthExp(DOF_MANUAL_FOCUS_DISTANCE * screenBrightness);
    #endif
```
If you added that, then only left things to do is replace `centerDepthSmooth` in DOF function and also adding uniforms to the top of shader programs, where you use DOF function. These are all the uniforms you will need to add, like this:
```glsl
uniform float screenBrightness;
uniform float far;
uniform float near;
```
and lastly, adding these options in `shaders.properties`. You can also optionally add better names for them in `en_US.lang` or other language files available, if you wish to like this:
```
option.CAMERA_FOCUS_MODE=Camera Focus Mode
option.CAMERA_FOCUS_MODE.comment=Focus mode of the camera. 'Auto' focuses on whatever the player is looking at. 'Manual' allows the player to set the distance the camera tries to focus at in meters. 'Manual+' allows to adjust focal point with Brightness value in Video Settings.
option.CAMERA_FOCAL_POINT=Camera Focal Point
option.CAMERA_FOCAL_POINT.comment=Distance the camera tries to focus at when the camera is set to manual focus mode.
suffix.CAMERA_FOCAL_POINT= m
```
**Be aware that if you do use `screenBrightness` uniform in other stuff, it WILL conflict, so you will want to disable that stuff (removing them altogether or making `if` statements).**
## Special Thanks to...
* [McHorse](https://twitter.com/McHorsy) for **making my (and also others) dream of adjustable focus distance while recording Minecraft Machinimas/Cinematic Videos COME TRUE**<br> by using my solution to the problem of Static Focus Distance/Auto Focus.