# amd-opencl-patches

Patches to make OpenCL work on my AMD FirePro GPU on Ubuntu 16.04.

Background is in my blog post
[OpenCL on a dual graphics Dell M4800](http://io.zwets.it/2016/08/18/opencl-on-a-dual-graphics-dell-m4800/).

This repository collects the fixes and patches I applied to the AMDGPU-PRO
packages and Catalyst Pro (fglrx) driver to make them work and install cleanly
on Ubuntu 16.04.

Please note that this is only about the OpenCL (computational) bits of these
packages.  I don't use the GPU for graphics.  Both AMDGPU-PRO and Catalyst
(fglrx) contain a package which implements an OpenCL platform exposing both
the **C**PU and the **G**PU as OpenCL devices.

See my [blog post](http://io.zwets.it/2016/08/18/opencl-on-a-dual-graphics-dell-m4800/)
for all information.

| File | Description | 
|------|-------------|
| `fglrx-core-15.302-fixes.md` | Script to fix various issues in the `fglrx-core` Debian package. |
| `fglrx-core-15.302-driver.patch` | Driver code patches to make `fglrx` work on kernel 4.4 (gleaned from <https://github.com/kolasa/fglrx-core-15.201/commits/master>). |

