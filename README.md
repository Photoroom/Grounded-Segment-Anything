![](./assets/Grounded-SAM_logo.png)

# Optimized Grounded-Segment-Anything

This is a public fork of [Grounded-Segment-Anything](https://github.com/IDEA-Research/Grounded-Segment-Anything) with some optimizations fro faster inference.

**🚀 Optimizations**

Ram and SAM:

- First iteration 4 its/s 1% gpu util, 20% gpu mem taken:  4.13 s
- Don't save raw image, 4.03 s
- Add torch cuda sync for measure. Do NMS on gpu, 4.05 s
- rejig image loading, 4.03 s
- move sam definition out LOL 😱, time taken:  0.86 s
- torch inference mode, 0.79 s
- compile sam, 0.48 s
- compile ram, 0.47 s
- fast-sam and autocast, 0.2 s
- removed torch.cuda.synchronizes and used turbojpeg 0.1-0.16
- add scaled dot product attention to RAM swin. SDPA, 87images/s
- batch everything -most important - 10x. Batch 512, workers=8,  256images/s
- Move to gpu in int8 and normalise on gpu 420 images/s. 🔥 🧑‍🍳 Now we're cooking son
- Move all to compile 593.92 images/s peak
- Using autocast, can try with fp16 direct. FP16 732 images/s ✅ 



Grounding Dino:

- Load csv and index file. Match ids to idx
- Loop through images 1x1 record output with sam mask first check looks ok. 
- Starting point no dataloading only fwd 0.07254481315612793 s/ image.
- FP16 0.05 s/image
- create tensor on gpu direct 0.04 s
- Pad to a square and check results.
- torch compile ❌ For some reason not faster.
- batch - not worth it likely 😕 
- image to gpu in uint8 for both grounding dino and SAM. And fast_sam

On H100 it takes 15hrs for 1M images ~18 images/s



## Installation
The code requires `python>=3.8`, as well as `pytorch>=1.7` and `torchvision>=0.8`. 

### Install with Docker

TODO


## Authors

Marco Forte, ML Team @ Photoroom