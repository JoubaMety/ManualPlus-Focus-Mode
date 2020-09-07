# BSL v7.2 - Manual+ Focus Mode Tutorial
## Part 0/4
If you're new to this stuff, I recommend using [Notepad++](https://notepad-plus-plus.org/) as it's lightweight and also it just works.  
  
Before you start, don't forget to unzip the shaderpack to its' folder. So file path is gonna be like `.minecraft\shaderpacks\BSL_v7.2\`.
  
You can download shaderpack [here](https://www.curseforge.com/minecraft/customization/bsl-shaders/download/3037267).

## Part 1/4 `BSL_v7.2/shaders/program/composite3.glsl`
* Under LINE 17, or:
```glsl
uniform float centerDepthSmooth;
```
add this:
```glsl
uniform float screenBrightness;
uniform float far;
uniform float near;
```
* Under LINE 97, or:
```glsl
	float hand = float(z < 0.56);
```
add this:
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
* In LINE 113, or:
```glsl
	float coc = max(abs(z - centerDepthSmooth) * DOF_STRENGTH - 0.01, 0.0);
```
replace `centerDepthSmooth` with `focus`

## Part 2/4 `BSL_v7.2/shaders/lib/lighting/forwardLighting.glsl`
* In LINE 77, or:
```glsl
    float minLighting = (0.009 * screenBrightness + 0.001) * (1.0 - eBS);
```
delete `* screenbrightness` OR replace that line with this:
```glsl
    float minLighting = (0.009 + 0.001) * (1.0 - eBS);
```
## Part 3/4
* In LINE 31, or:
```properties
screen.POST_PROCESS=<empty> <empty> DOF DOF_STRENGTH MOTION_BLUR MOTION_BLUR_STRENGTH BLOOM BLOOM_STRENGTH LENS_FLARE LENS_FLARE_STRENGTH AA SHARPEN AUTO_EXPOSURE [COLOR_GRADING] VIGNETTE DIRTY_LENS RETRO_FILTER
```
replace `DOF_STRENGTH` with `[DOF]`
<br>**DON'T FORGET A SPACE SEPARATING EACH SETTING!**
<br>like this:
```properties
screen.POST_PROCESS=<empty> <empty> DOF [DOF] MOTION_BLUR MOTION_BLUR_STRENGTH BLOOM BLOOM_STRENGTH LENS_FLARE LENS_FLARE_STRENGTH AA SHARPEN AUTO_EXPOSURE [COLOR_GRADING] VIGNETTE DIRTY_LENS RETRO_FILTER
```
* In LINE 31 (same line as above), select the end of LINE and create new line underneath LINE 31 by pressing `Enter` and then paste this into new line:
```properties
screen.DOF=<empty> <empty> DOF DOF_STRENGTH DOF_MODE DOF_MANUAL_FOCUS_DISTANCE
```
* In LINE 81, or:
```properties
sliders=AO_STRENGTH LIGHT_SHAFT_STRENGTH DESATURATION_FACTOR PARALLAX_DEPTH SELF_SHADOW_ANGLE PARALLAX_QUALITY PARALLAX_DISTANCE DIRECTIONAL_LIGHTMAP_STRENGTH DOF_STRENGTH MOTION_BLUR_STRENGTH BLOOM_STRENGTH LENS_FLARE_STRENGTH SHARPEN TONEMAP_EXPOSURE TONEMAP_WHITE_CURVE TONEMAP_LOWER_CURVE TONEMAP_UPPER_CURVE SATURATION VIBRANCE CG_RR CG_RG CG_RB CG_RI CG_RM CG_RC CG_GR CG_GG CG_GB CG_GI CG_GM CG_GC CG_BR CG_BG CG_BB CG_BI CG_BM CG_BC CG_TR CG_TG CG_TB CG_TI CG_TM shadowMapResolution shadowDistance sunPathRotation LIGHT_MR LIGHT_MG LIGHT_MB LIGHT_MI LIGHT_DR LIGHT_DG LIGHT_DB LIGHT_DI LIGHT_ER LIGHT_EG LIGHT_EB LIGHT_EI LIGHT_NR LIGHT_NG LIGHT_NB LIGHT_NI AMBIENT_MR AMBIENT_MG AMBIENT_MB AMBIENT_MI AMBIENT_DR AMBIENT_DG AMBIENT_DB AMBIENT_DI AMBIENT_ER AMBIENT_EG AMBIENT_EB AMBIENT_EI AMBIENT_NR AMBIENT_NG AMBIENT_NB AMBIENT_NI BLOCKLIGHT_R BLOCKLIGHT_G BLOCKLIGHT_B BLOCKLIGHT_I SKY_R SKY_G SKY_B SKY_I WATER_R WATER_G WATER_B WATER_I WATER_A WATER_F WEATHER_RR WEATHER_RG WEATHER_RB WEATHER_RI WEATHER_CR WEATHER_CG WEATHER_CB WEATHER_CI WEATHER_DR WEATHER_DG WEATHER_DB WEATHER_DI WEATHER_BR WEATHER_BG WEATHER_BB WEATHER_BI WEATHER_SR WEATHER_SG WEATHER_SB WEATHER_SI WEATHER_MR WEATHER_MG WEATHER_MB WEATHER_MI WEATHER_VR WEATHER_VG WEATHER_VB WEATHER_VI NETHER_NR NETHER_NG NETHER_NB NETHER_NI NETHER_VR NETHER_VG NETHER_VB NETHER_VI NETHER_CR NETHER_CG NETHER_CB NETHER_CI NETHER_WR NETHER_WG NETHER_WB NETHER_WI NETHER_BR NETHER_BG NETHER_BB NETHER_BI END_R END_G END_B END_I CLOUD_THICKNESS CLOUD_AMOUNT CLOUD_HEIGHT CLOUD_OPACITY CLOUD_SPEED CLOUD_BRIGHTNESS HORIZON_DISTANCE SKYBOX_BRIGHTNESS WATER_OCTAVE WATER_BUMP WATER_LACUNARITY WATER_PERSISTANCE WATER_SHARPNESS WATER_SIZE WATER_SPEED EMISSIVE_BRIGHTNESS WEATHER_OPACITY FOG_DENSITY WORLD_CURVATURE_SIZE ANIMATION_SPEED
```
add this at the end:
```
DOF_MANUAL_FOCUS_DISTANCE
```
**DON'T FORGET A SPACE BEFORE SETTING FOR SEPARATING!**

## Part 4/4 `BSL_v7.2/lang/en_US.lang`
At the end of this file (or LINE 550), select the end of file/end of last line and add a new line by pressing `Enter` and add this:
```properties
screen.DOF=DoF Settings
option.DOF_MODE=DoF Focus Mode
option.DOF_MODE.comment=Focus mode of the camera. 'Auto' focuses on whatever the player is looking at. 'Manual' allows the player to set the distance the camera tries to focus at in meters. 'Manual+' allows to adjust focal point with Brightness value in Video Settings.
value.DOF_MODE.0=Auto
value.DOF_MODE.1=Manual
value.DOF_MODE.2=Manual+
option.DOF_MANUAL_FOCUS_DISTANCE=DoF Focus Distance
option.DOF_MANUAL_FOCUS_DISTANCE.comment=Distance the camera tries to focus at when the camera is set to manual focus mode.
suffix.DOF_MANUAL_FOCUS_DISTANCE= m
```
## FINALE
And that's it. Now you can enjoy Manual+ Focus Mode.  