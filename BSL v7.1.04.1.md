# BSL v7.1.04.1 - Manual+ Focus Mode Tutorial
## Part 0/7
If you're new to this stuff, I recommend using [Notepad++](https://notepad-plus-plus.org/) as it's lightweight and also it just works.  
  
Before you start, don't forget to unzip the shaderpack to its' folder. So file path is gonna be like `.minecraft\shaderpacks\BSL_v7.1.04.1\`.
  
You can download shaderpack [here](https://www.curseforge.com/minecraft/customization/chocapic13-shaders/download/2901475).
## Part 1/7  `BSL_v7.1.04.1\shaders\lib\post\depthOfField.glsl`
* In LINE 78, paste this piece of code:
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
* In LINE 93-94 or where's this located:
```glsl
	float coc = max(abs(z-centerDepthSmooth)*DOFStrength-0.0001,0.0);
```
replace `centerDepthSmooth` with `focus`
## Part 2/7  `BSL_v7.1.04.1\shaders\world0\composite3.fsh`
* Below LINE 19, add this:
```glsl
uniform float screenBrightness;
uniform float far;
uniform float near;
```
## Part 3/7  `BSL_v7.1.04.1\shaders\world0\gbuffers_basic.fsh`
* In LINE 149 or:
```glsl
		float minlight = (0.009*screenBrightness + 0.001)*(1.0-eBS);
```
remove `*screenBrightness`
## Part 4/7  `BSL_v7.1.04.1\shaders\world0\gbuffers_basic.vsh`
* In LINE 242, or:
```glsl
		float minlight = (0.009*screenBrightness + 0.001)*(1.0-eBS);
```
remove `*screenBrightness`
## Part 5/7  `BSL_v7.1.04.1\shaders\world0\gbuffers_terrain.fsh`
* In LINE 329, or:
```glsl
		float minlight = (0.009*screenBrightness + 0.001)*floor(color.a*4.0+0.999)/4.0*(1.0-eBS);
```
remove `*screenBrightness`
* In LINE 332, or:
```glsl
		float minlight = (0.009*screenBrightness + 0.001)*(1.0-eBS);
```
remove `*screenBrightness`
## Part 6/7  `BSL_v7.1.04.1\shaders\shaders.properties`
* In LINE 32, or:
```properties
screen.PostProcess=<empty> <empty> DOF DOFStrength MotionBlur MotionBlurStrength Bloom BloomStrength LensFlare LensFlareStrength AA Sharpen AutoExposure [ColorGrading] Vignette DirtyLens
```
after `DOFStrength`, add this:
```
DOF_MODE DOF_MANUAL_FOCUS_DISTANCE
```
**DON'T FORGET A SPACE SEPARATING EACH SETTING!**
* At the end of LINE 75 or this LINE:
```properties
sliders=AOStrength LightShaftStrength DesaturationFactor ReflectAccuracy LightmapDirStrength POMQuality POMDepth POMDistance POMShadowAngle DOFStrength MotionBlurStrength BloomStrength LensFlareStrength Sharpen TonemapExposure TonemapWhiteCurve TonemapLowerCurve TonemapUpperCurve Saturation Vibrance ColR1 ColG1 ColB1 ColS1 ColM1 ColC1 ColR2 ColG2 ColB2 ColS2 ColM2 ColC2 ColR3 ColG3 ColB3 ColS3 ColM3 ColC3 ColR4 ColG4 ColB4 ColS4 ColM4 shadowMapResolution shadowDistance sunPathRotation LightMR LightMG LightMB LightMS LightDR LightDG LightDB LightDS LightAR LightAG LightAB LightAS LightNR LightNG LightNB LightNS SkyMR SkyMG SkyMB SkyMS SkyDR SkyDG SkyDB SkyDS SkyER SkyEG SkyEB SkyES SkyNR SkyNG SkyNB SkyNS TorchR TorchG TorchB TorchS SkyCR SkyCG SkyCB SkyCS WaterR WaterG WaterB WaterS WaterA WaterF WeatherRR WeatherRG WeatherRB WeatherRS WeatherCR WeatherCG WeatherCB WeatherCS WeatherDR WeatherDG WeatherDB WeatherDS WeatherBR WeatherBG WeatherBB WeatherBS WeatherSR WeatherSG WeatherSB WeatherSS WeatherMR WeatherMG WeatherMB WeatherMS NetherR NetherG NetherB NetherS EndR EndG EndB EndS CloudQuality CloudThickness CloudAmount CloudHeight CloudOpacity CloudSpeed CloudBrightness HorizonDistance SkyboxBrightness WaterOctave WaterBump WaterLacunarity WaterPersistance WaterSharpness WaterSize WaterSpeed EmissiveBrightness WeatherOpacity FogRange WorldCurvatureSize AnimationSpeed
```
add this:
```
DOF_MANUAL_FOCUS_DISTANCE
```
**DON'T FORGET TO MAKE A SPACE BEFORE THIS!**
## Part 7/7  `BSL_v7.1.04.1\shaders\lang\en_US.lang`
At the end of this file (or LINE 475), add a new line by pressing `Enter` and add this:
```properties
option.DOF_MODE=Camera Focus Mode
option.DOF_MODE.comment=Focus mode of the camera. 'Auto' focuses on whatever the player is looking at. 'Manual' allows the player to set the distance the camera tries to focus at in meters. 'Manual+' allows to adjust focal point with Brightness value in Video Settings.
value.DOF_MODE.0=Auto
value.DOF_MODE.1=Manual
value.DOF_MODE.2=Manual+
option.DOF_MANUAL_FOCUS_DISTANCE=Camera Focal Point
option.DOF_MANUAL_FOCUS_DISTANCE.comment=Distance the camera tries to focus at when the camera is set to manual focus mode.
suffix.DOF_MANUAL_FOCUS_DISTANCE= m
```

## FINALE (Almost)
And that's it. Now you can enjoy Manual+ Focus Mode.  
Though there's one thing, this only applies to Overworld dimension, meaning it won't work in other dimensions like Nether and End, so if you want to use this also in Nether/End, then use Optional Fix below.

## Optional - Nether and End Brightness "Fix"
Soon™️