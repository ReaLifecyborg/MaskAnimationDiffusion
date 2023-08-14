# MaskAnimationDiffusion
This is a github repo for recording my workflow, for possible use of diffusion model in animation. I am also wanting to make this into a paper and try attending conference for more in depth sharing so i can know what and how to improve.

## Whats a Diffusion model
In short, a model trained on noise image to try reproduce the original image that have no noise added, which the diffusion model would be trained in x step(x=infinity in original paper), this nature means diffusion models at no point or possibility to be hundred percent to be able to reproduce exact same picture from dataset(hence bad hands you see), thus this repo exist to show how and what i think it would be possible to be used in animation industry.

## Special thanks to following personals(alphabetical order)
- cybermeow for helping me interview his professional lead animator brother in the industry so I know how some in-depth anime is made
- kotek_mlotek for lending his 3090 for testing and help in making lora
- KohakuBlueleaf (https://github.com/KohakuBlueleaf) for helping in giving advice on general diffusion-related questions, also about to make extune, for which I would provide the dataset
- neggles (https://github.com/neggles) for help in setting up my WSL2 Docker for SD and assistance in multiple aspects, including letting me run stuff on her 4090
- Nocrypt (https://github.com/NoCrypt) for creating amazing colab and pointing out mistakes during modifying scripts for frame interpolation
- toyxyz (https://twitter.com/toyxyz3) for teaching me what a segmentation mask is, none of this would be possible without his explanation to me as a noob back then
- UuuNyaa (https://github.com/UuuNyaa) for his automation in the process of segmentation modification in 3D models, a main part of the workflow which allows us to do a lot, plus teaching me a lot of MMD-related things
- 6DammK9 (https://github.com/6DammK9) for feeding me the red pill and helping me in another project I work on



## Overview of Workflow
1. Setup and prepare mask nodes from blender
2. Modify blender's setting to allow noise free output for mask
3. Prepare 3D assets and animation
4. Modify 3D asset for compatiability to mask generation nodes
5. Feed to Stable Diffusion with multiple input and mask guidance, forcing the Diffusion model to work on specific stuff we wish to see(in the meanwhile eliminating the instability of the model)
6. Test for styles
7. Full run with frame interpolation to improve consistentcy
8. Auto perfect crop/recoloring for character with scripts(as we prepared the perfect mask, we can use it to do background removal)
9. Feed the output to blender for visual effects, possible light effect manupulation

## What you would need
1. Patience and lot of time testing what fit your taste
2. A GPU with 24G/more vram(altho its able to run in 12G vram but you would miss out the hires fix)
3. Long term mental support and enough 3D software experience to at least make a short 3d movie, or have a team to do that for you
4. Some coding experience so at least you would understand what im doing, for AI part i would feed you enough knowledge in this repo
5. Experience with Automatic1111's stable diffusion webui as we would require it to work before comfy allow advance setting for controlnet

## General Explaination
