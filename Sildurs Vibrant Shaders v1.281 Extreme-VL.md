# Sildurs Vibrant Shaders v1.281 Extreme-VL - Manual+ Focus Mode Tutorial
## Part 0/4
If you're new to this stuff, I recommend using [Notepad++](https://notepad-plus-plus.org/) as it's lightweight and also it just works.  
  
Before you start, don't forget to unzip the shaderpack to its' folder. So file path is gonna be like `.minecraft\shaderpacks\Sildurs Vibrant Shaders v1.281 Extreme-VL\`.
  
You can download shaderpack [here](https://sildurs-shaders.github.io/).

## Part 1/4 `\..\shaders\program\composite3.glsl`
* Make a new line under LINE 67 (which looks like this `uniform float centerDepthSmooth;`) and add this:
```glsl
uniform float screenBrightness;
```

* Replace LINE 232-236 or this part:
```glsl
	#ifdef smoothDof
	float focus = ld(centerDepthSmooth)*far;
	#else
	float focus = ld(texture2D(depthtex0, vec2(0.5)).r)*far;
	#endif
```
* with this:
```glsl
	float focus; float focus_x;
	if (DoF_Mode == 2){
		focus = DoF_MaxFocalPoint * screenBrightness;
	}	else if (DoF_Mode == 1) {
		focus = DoF_MaxFocalPoint;
			}	else {
				#ifdef smoothDof
				focus = ld(centerDepthSmooth)*far;
				#else
				focus = ld(texture2D(depthtex0, vec2(0.5)).r)*far;
				#endif
			}
```

## Part 2/4 `\..\shaders\shaders.settings\`
* Under LINE 149, make a new line and add this to the new line:
```glsl
        #define DoF_MaxFocalPoint 5             //Focus Distance for Manual DoF Mode. Maximum Focus Distance for Manual+ DoF Mode. [0.1 0.2 0.3 0.4 0.5 0.7 0.9 1 1.1 1.2 1.3 1.4 1.5 1.6 1.8 1.9 2 2.1 2.2 2.3 2.4 2.5 2.6 2.7 2.8 2.9 3 4 5 6 7 8 9 10 12 14 16 24 32 40 48 56 64 72 80 88 96 104 112 120 128 136 144 152 160 168 176 184 192 200 208 216 224 232 240 248 256]
        #define DoF_Mode 0                      // Focus mode of the camera. 'Auto' focuses on whatever the player is looking at. 'Manual' allows the player to set the distance the camera tries to focus at in meters. 'Manual+' allows to adjust focal point with Brightness value in Video Settings. [0 1 2]
```

# Part 3/4 `\..\shaders\shaders.properties`
* In LINE 44 or in LINE, which begins with `screen.DOF_SCREEN`is located, add this after `DoF_Strength`:
```glsl
DoF_Mode DoF_MaxFocalPoint
```
which results in:
```properties
screen.DOF_SCREEN=Depth_of_Field DoF_Strength DoF_Mode DoF_MaxFocalPoint smoothDof <empty> Distance_Blur Dof_Distance_View
```
* In LINE 57 or in LINE, which begins with `sliders`, add this at the end of the line:
```glsl
DoF_MaxFocalPoint
```
which results in:
```properties
sliders=emissive_R emissive_G emissive_B waterCR waterCG waterCB waterA light_brightness r_multiplier g_multiplier b_multiplier shadow_samples Nearshadowplane Farshadowplane shadowDistance Brightness Contrast DoF_Strength Dof_Distance_View wFogDensity uFogDensity POM_DIST POM_DEPTH minlight cloudsIT cloudreflIT cloud_height skyboxblendfactor sunPathRotation rainNoise waveSize causticsStrength AS_sharpening waves_amplitude wtexblend metalStrength metallicSky DoF_MaxFocalPoint
```

## Part 4/4 `\..\lang\en_US.lang`
Under LINE 227 (empty LINE under LINE 226 basically), add a new line and add this:
```properties
option.DoF_Mode=DoF Focus Mode
value.DoF_Mode.0=Auto
value.DoF_Mode.1=Manual
value.DoF_Mode.2=Manual+
option.DoF_MaxFocalPoint=DoF Focus Distance
suffix.DoF_MaxFocalPoint= m
```
Optionally, you can make another new empty line under the new code, so it looks good, at least when viewing the code.

## FINALE (Almost)
And that's it. Now you can enjoy Manual+ Focus Mode.<br><br>
Though there's one thing, this only applies to Overworld dimension, meaning it won't work in other dimensions like Nether and End, so if you want to use this also in Nether/End, then use Optional Fix below.

## Optional - Nether and End DOF "Fix"
Soon™️