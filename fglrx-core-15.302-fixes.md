## Fixing the fglrx-core package

The steps below fix the issues with the `fglrx-core` package which I discussed in
[OpenCL on a dual graphics Dell M4800](http://io.zwets.it/2016/08/18/opencl-on-a-dual-graphics-dell-m4800/),


### Steps 

Unpack the debfile just as it is layed out

    dpkg-deb -R fglrx-core_*.deb fglrx-core
    cd fglrx-core

Rename clinfo to clinfo-amd (or rm it altogether)

    mv usr/bin/clinfo usr/bin/clinfo-amd
    sed -i -e 's/clinfo$/clinfo-amd/' DEBIAN/md5sums

Remove the OpenCL libraries

    rm -f usr/lib/libOpenCL.so.1 usr/lib32/libOpenCL.so.1
    sed -i -e '/libOpenCL\.so\.1$/d' DEBIAN/md5sums

Add `/etc/ati/amdpcsdb.default` which AMD should have added to in this package.
Without it `clinfo` segfaults from the second time you call it.

    mkdir etc/ati
    /path/to/amd-driver-installer-*.run --extract tmp
    cp tmp/common/etc/ati/amdpcsdb.default etc/ati
    md5sum etc/ati/amdpcsdb.default >> DEBIAN/md5sums
    rm -rf tmp

(Optional) remove the 32-bit ICD and 32-bit libraries

    rm -rf usr/lib32 etc/OpenCL/vendors/amdocl32.icd
    sed -i -e '/usr.lib32/d' DEBIAN/md5sums
    sed -i -e '/amdocl32.icd/d' DEBIAN/conffiles
    sed -i -e '/libOpenCL/d' DEBIAN/shlibs

Fix the dependencies in the control file

    sed -i \
        -e 's/^Depends.*/Depends: libc6 (>= 2.3.4), libgcc1 (>= 1:4.2), libstdc++6 (>= 4.1.1), dkms/' \
        -e 's/^Provides.*/Provides: fglrx-driver-core/' \
        -e 's/^Conflicts.*/Conflicts: fglrx-driver-core, amdgpu-pro-opencl-icd/' \
      DEBIAN/control

Patch the driver code to make it compile against kernel 4.4 and 4.5.

    patch -p0 < fglrx-core-15.302-driver.patch

Done.  Make everything root again and repack it.  Note the restore of the setuid
and setgid attributes on `amd-console-helper`.  do we really need that?

    sudo chown -R root:root .
    sudo chmod ug+s usr/bin/amd-console-helper
    cd ..
    sudo dpkg-deb -b


