# amd-opencl-patches

Patches to make OpenCL work on my AMD FirePro GPU on Ubuntu 16.04.

Background is in my blog post
[OpenCL on a dual graphics Dell M4800](http://io.zwets.it/2016/08/18/opencl-on-a-dual-graphics-dell-m4800/).

This repository collects the fixes and patches I applied to the AMDGPU-PRO and Catalyst
Pro Ubuntu packages to make make them work *and* cleanly install on Ubuntu 16.04.

| File | Description | 
|------|-------------|
| `fglrx-core-15.302-fixes.md` | Script to fix various issues in the `fglrx-core` Debian package. |
| `fglrx-core-15.302-driver.patch` | Driver code patches to make `fglrx` work on kernel 4.4 (gleaned from <https://github.com/kolasa/fglrx-core-15.201/commits/master>. |

