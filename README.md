# cryptsetup-2.0.1-luksAddNuke

For Arch Linux
cryptsetup 2.0.1-1 Patch add Nuke Keys

#Download PKGBUILD for cryptsetup 2.0.1-1

wget https://git.archlinux.org/svntogit/packages.git/plain/trunk/PKGBUILD?h=packages/cryptsetup

mv PKGBUILD?h=packages%2Fcryptsetup PKGBUILD

#download and extract not build

makepkg -o

#Extract cryptsetup-2.0.1.tar.xz to folder src

mkdir src

tar -xJvf cryptsetup-2.0.1.tar.xz -C src

#download hooks-encrypt, install-encrypt, install-sd-encrypt

wget https://git.archlinux.org/svntogit/packages.git/plain/repos/core-x86_64/hooks-encrypt?h=packages/cryptsetup

mv hooks-encrypt?h=packages%2Fcryptsetup hooks-encrypt

wget https://git.archlinux.org/svntogit/packages.git/plain/repos/core-x86_64/install-encrypt?h=packages/cryptsetup

mv install-encrypt?h=packages%2Fcryptsetup install-encrypt

wget https://git.archlinux.org/svntogit/packages.git/plain/repos/core-x86_64/install-sd-encrypt?h=packages/cryptsetup

mv install-sd-encrypt?h=packages%2Fcryptsetup install-sd-encrypt

#copy 4 patch file to src folder
cp libcryptsetup.h.patch src
cp keymanage.c.patch src
cp setup.c.patch src
cp cryptsetup.c.patch src

#copy hooks-encrypt, install-encrypt, install-sd-encrypt to src folder
cp hooks-encrypt src
cp install-encrypt src
cp install-sd-encrypt src

#edit PKGBUILD

nano PKGBUILD

# add to source at PKGBUILD
'hooks-encrypt'
'install-encrypt'
'install-sd-encrypt'
'cryptsetup.c.patch'
'keymanage.c.patch'
'libcryptsetup.h.patch'
'setup.c.patch'

#add luksAddNuke patch before make DESTDIR=”${pkgdir}” install at PKGBUILD

msg "Patching source to enable luksAddNuke"
patch -p1 < ../libcryptsetup.h.patch
patch -p1 < ../keymanage.c.patch
patch -p1 < ../setup.c.patch
patch -p1 < ../cryptsetup.c.patch

#save then update sha256sums
updpkgsums

#install

makepkg -ei
