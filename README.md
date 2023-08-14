# MaskAnimationDiffusion
This is a github repo for recording my workflow, for possible use of diffusion model in animation(all are free tools). I am also wanting to make this into a paper and try attending conference for more in depth sharing so i can know what and how to improve.

## Whats a Diffusion model
In short, a model trained on noise image to try reproduce the original image that have no noise added, which the diffusion model would be trained in x step(x=infinity in original paper), this nature means diffusion models at no point or possibility to be hundred percent to be able to reproduce exact same picture from dataset(hence bad hands you see), thus this repo exist to show how and what i think it would be possible to be used in animation industry.

## Special thanks to following personals(alphabetical order)
- cybermeow for helping me interview his professional lead animator brother in the industry so I know how some in-depth anime scene and motion is made
- kotek_mlotek for lending his 3090 for testing and help in making lora
- KohakuBlueleaf (https://github.com/KohakuBlueleaf) for helping in giving advice on general diffusion-related questions, also about to make extune, for which I would provide the dataset
- neggles (https://github.com/neggles) for help in setting up my WSL2 Docker for SD and assistance in multiple aspects, including letting me run stuff on her 4090
- Nocrypt (https://github.com/NoCrypt) for creating amazing colab and pointing out mistakes during modifying scripts for frame interpolation
- toyxyz (https://twitter.com/toyxyz3) for teaching me what a segmentation mask is, none of this would be possible without his explanation to me as a noob back then
- UuuNyaa (https://github.com/UuuNyaa) for his automation in the process of segmentation modification in 3D models, a main part of the workflow which allows us to do a lot, plus teaching me a lot of MMD-related things
- 6DammK9 (https://github.com/6DammK9) for feeding me the red pill and helping me in another project I work on



## Overview of Workflow
1. Setup and prepare mask nodes for blender
2. Modify blender's setting to allow noise free output for mask
3. Prepare 3D assets and animation
4. Modify 3D asset for compatiability to mask generation nodes
5. Feed to Stable Diffusion with multiple input and mask guidance, forcing the Diffusion model to work on specific stuff we wish to see(in the meanwhile eliminating the instability of the model)
6. Test for styles
7. Full run with frame interpolation to improve consistentcy
8. Auto perfect crop/recoloring for character with scripts(as we prepared the perfect mask, we can use it to do background removal)
9. Feed the output to blender for visual effects, possible light effect manupulation

## What you(or your team) would need
1. Patience and lot of time testing what fit your taste
2. A/multiple(if you want it to be faster,you can cut every scene and feed into different webui instance) GPU with 24G/more vram(altho its able to run in 12G vram but you would miss out the hires fix)
3. Long term mental support and enough 3D software experience(in specific,blender) to at least make a short 3d movie, or have a team to do that for you
4. Some coding experience so at least you would understand what im doing, for AI part i would feed you enough knowledge in this repo
5. Experience with Automatic1111's stable diffusion webui as we would require it to work before comfy allow advance setting for controlnet

## General Explaination
As the nature of Diffusion model is that, its always unstable, we need a way to force it to do what we want it to.In this aspect, controlnet is a method of altering and adding conditions for the diffusion model. Within all the models in controlnet, the following stands out to be the **most influencial and my personal pick for the workflow**:
- segmentation, this is the backbone of everything, without this, it would be almost impossible to keep it all in shape, it defines areas of object, hence keeping absolute shape of an object in the output, this is what i abused to force diffusion model to generate fingers in allign to my 3d assets
- lineartanime, this is what allowed more stylizing than canny, the previous controlnet i used alongside segmentation to provide lines to the shape, however we dont want this to be overpowering the prompt so we would do some tricky thing with it later
- reference-only, tehcnically not a control net model,but a preprocessor that require no model to run, hence not loading model into vram, this influecne coloring, altho its far from enough for our workflow, its the only choice for now
- temporalnetv2, a controlnet model for frame interpolation, this would require its own part in the following read, it also require running from api and a custom version of controlnet extension
- normal, this controlnet read normal mask as input, i have not yet tested this and its on my next to-do list, it should be theoretically do better shape and lighting for the output
- light, this is a rare controlnet that never got finished and is still in beta version, links at (https://aigc.ioclab.com/sd-showcase/light_controlnet.html), it was originally a consideration for light effect but as far as it goes, i need to observe if it ever gonna be finished

With all these controlnet methods, we can do a lot of influence to what the diffusion model would output to us, in another word, do animation in shape.
And what we need to do to make it all work, we prepare dataset for every single frame(with blender it should be easy for professional 3D artist, or if you are not experienced in 3D like me, you can use mmd tool and mmd motion distribution for it) then automatically output mask for every single frame we need, then we shove it into stable diffusion so it does its magic and we wait for after editing with help of automated scripts.

With some observation and modification, i believe this workflow is a more in depth way of doing things which not many people knows about.

The overall aiming for this workflow is to make it works for individual animators to save them time and money, or as a prototype of mass production workcycle for animation studio for reduce labour cost and fasten the production(however its now far from industry standard for now, if you are professional in 3D and wants to contribute/have advice to this workflow, please contact me), this workflow is compatiable with loras and different art style, but for simplicity i would not use something too advance in these in this repo(noise lighting adding as artstyle influence is a thing).

This allow user(animator) to decide what art style its in, with control of how animation is done with the stable diffusion without stucking with low denoising setting in the stable diffusion which limit how art style altered the result can be.

## Setup and prepare mask nodes for blender
Note that all of the workflow in this part is in blender and i am at no way professional in 3D to tell you if this would work in other 3D application, it would work if a similar function exists in that software and the same setting is in such software, but you would need to develop your own setup in that software.
First setup lineart mask in blender, this is relatively easy and should not cost you more than 5 minutes.
We for this case would abuse lineart modifier in blender to simulate real line work of animator in animation industry, or if you are professional that want more control, any custom grease pencil would work.
