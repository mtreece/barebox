# configure the build
time make ARCH=mips O=build-tgt qemu-malta_defconfig

# note, the 32bit-emul= option here is to override a linker option that is not
# supported; may need to modify this value depending on what your linker
# supports
time make ARCH=mips O=build-tgt CROSS_COMPILE=mips-elf- -j4 32bit-emul=elf32ebmip
