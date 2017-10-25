# configure the build
time make ARCH=mips O=build-tgt demo_qemu_defconfig

# note, the 32bit-emul= option here is to override a linker option that is not
# supported; may need to modify this value depending on what your linker
# supports
time make ARCH=mips O=build-tgt CROSS_COMPILE=mips-elf- -j4 32bit-emul=elf32ebmip

# build will include absolute path filenames; use this to all-in-one sanitize
# it a bit
time (DIR=$(mktemp -d /tmp/staging.XXX); rsync -av $(pwd)/* $DIR/barebox/; cd $DIR/barebox; make ARCH=mips O=build-tgt CROSS_COMPILE=mips-elf- demo_qemu_defconfig _all -j4 32bit-emul=elf32ebmip; readlink -f build-tgt/barebox-flash-image)

# QEMU run
qemu-system-mips -nodefaults -M malta -m 256 -nographic -serial stdio -monitor null -bios /tmp/staging.*/build-tgt/barebox-flash-image
