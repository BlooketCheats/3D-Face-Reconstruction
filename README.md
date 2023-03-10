Hello My Name Is Noah And i Am (NOT THE INVENTOR) of this project this is made 
by the group of people named Aaron S. Jackson, Adrian Bulat, Vasileios Argyriou and Georgios Tzimiropoulos

Please Know i did not get any verbal or email for consent.







Try out the code without running it! Check out our online demo here. Alternatively, pull the DockerHub image `asjackson:vrn`, see docs in the vrn-docker repo.

http://aaronsplace.co.uk/papers/jackson2017recon/preview.png

Please visit our project webpage for a link to the paper and an example video run on 300VW. This code is licenses under the MIT License, as described in the LICENSE file.

This is an unguided version of the Volumetric Regression Network (VRN) for 3D face reconstruction from a single image. This method approaches the problem of reconstruction as a segmentation problem, producing a 3D volume, spatially aligned with the input image. A mesh can then be obtained by taking the isosurface of this volume.

Several example images are included in the examples folder. Most of these are AFLW images taken from 3DDFA.

If you are running the code to calculate error for a potential publication, please use the MATLAB version, as this is what was used to compute the error for the paper.

Prebuilt Docker Image for CPU version
I have released an image on Docker Hub which has everything to get the CPU version running under Docker. I’ll extend this to have CUDA support at some point.

docker pull asjackson/vrn:latest
docker run -v $(pwd)/data:/data:Z vrn /runner/run.sh /data/turing.jpg
The repo holding this is available at vrn-docker and the models (which have been cast to CPU floats already) were stored in Git LFS but took too much space. If you have an issue with this docker container, please use the vrn-docker issue tracker rather than the vrn issue tracker.

Software Requirements
A working installation of Torch7 is required. This can be easily installed on most platforms using torch/distro. You will also require a reasonable CUDA capable GPU.

This project was developed under Linux. I have no idea if it will work on Windows and it is unlikely that I will be able to help you with this. If you are running Mac OS, issue #1 might be of interest to you.

Quick overview of requirements:

Torch7 (+ nn, cunn, cudnn, image). See “Installation Example” below.
NVIDIA GPU, with a working CUDA (7.5 or 8.0) and CuDNN (5.1).
Either,
MATLAB
bash, ImageMagick, GNU awk, Python 2.7 (+ visvis, imageio, numpy)
Please be wary of the version numbers for CUDA, CuDNN and Python.

Bulat’s face alignment code is included as a submodule. Please check his README for dependencies.

Getting Started
git clone --recursive https://github.com/AaronJackson/vrn.git
cd vrn
./download.sh
Running with MATLAB
MATLAB offers better functionality for taking the iso surface of the volume. It also has some code to calculate per-vertex colouring on the mesh. If you have MATLAB I recommend this route.

To run, type “run” from MATLAB.

Running with Python
No longer is MATLAB an absolute requirement! I’ve included a slightly crazy (but don’t worry, I had fun writing it) shell script which performs the face normalisation, and runs the vis.py script to render the regressed volume.

Unfortunately this does not yet apply any colouring or texture to the mesh (you’re welcome to contribute) and it has some issues if you don’t have a fully working OpenGL setup. Some GPUs won’t like the background image not being a power of two, so it might make the results look odd. I’ll work on this sometime.

To run it on the included example images without MATLAB, make the run.sh executable with chmod u+x run.sh and type ./run.sh from your terminal.

Using your own images
You are, of course, welcome to try out this method on your own set of images. dlib, the face detector included with Bulat’s face alignment code struggles to find side poses. You are welcome to modify the code to provide bounding boxes.

Available Options
The MATLAB “run.m” script contains a few options which you can change. Here is a very quick description of them:

input_folder, as the name suggests, the folder to glob for JPEG images.
output_folder, the directory to store the regressed volumes.
model_file, the name of the Torch model to load.
gpunum, specify which GPU to use, starting at 0.
texture, rudimentary texture mapping by taking the 2D projections nearest neighbour (MATLAB only).
Installation Example
I’ve had a few requests to describe a little better how to configure Torch so that everything works correctly. I’ve tested this on Fedora 24 and CentOS 7. I’m assuming it will also work on Ubuntu if you have the correct development packages installed.

