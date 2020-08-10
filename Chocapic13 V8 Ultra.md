# Chocapic13 V8 - Ultra (CurseForge)
## Part 0/2
If you're new to this stuff, I recommend using [Notepad++](https://notepad-plus-plus.org/) as it's lightweight and also it just works.  
  
Before you start, don't forget to unzip the shaderpack to its' folder. So file path is gonna be like `.minecraft\shaderpacks\Chocapic13_V8_Ultra\`.
  
You can download shaderpack [here](https://www.curseforge.com/minecraft/customization/chocapic13-shaders/download/2901475).

## Part 1/2 `Chocapic13_V8_Ultra\shaders\composite15.fsh`
* In LINE 20, or:<br> `#define AUTOFOCUS`,<br> replace it with this:
```glsl
#define FOCUSMODE 0 // 0 is for Auto Focus. 1 is for Manual Focus. 2 is for Manual+ Focus. [0 1 2]
```
* Under LINE 374, or:<br> `uniform float frameTimeCounter;`,<br> make new LINE by pressing `Enter` and then, add this:
```glsl
uniform float screenBrightness;
uniform float centerDepthSmooth;
```
* From LINE 432 to LINE 436, or:
```glsl
        #ifdef AUTOFOCUS
			float focus = ld(texture2D(depthtex0, vec2(0.5)).r)*far;
		#else
			float focus = MANUAL_FOCUS;
		#endif
```
replace with this:
```glsl
        float focus;
		#if FOCUS_MODE == 0
			  focus = ld(centerDepthSmooth)*far;
		#endif
		#if FOCUS_MODE == 1
			  focus = MANUAL_FOCUS;
		#endif
		#if FOCUS_MODE == 2
			  focus = MANUAL_FOCUS * screenBrightness;
		#endif
```
## Part 2/2 `Chocapic13_V8_Ultra\shaders\shaders.properties`
* In LINE 60, or:<br>
```glsl
screen.DepthOfField = DOF HQ_DOF HEXAGONAL_BOKEH AUTOFOCUS focal aperture MANUAL_FOCUS FAR_BLUR_ONLY
```
replace `AUTOFOCUS` with `FOCUSMODE`.

## FINALE (Almost)
Though there's one thing, this only applies to Overworld dimension, meaning it won't work in other dimensions like Nether and End, so if you want to use this also in Nether/End, then use Optional Fix below.

## Optional - Nether and End DOF "Fix"
Soon™️