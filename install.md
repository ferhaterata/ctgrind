```bash
# clone valgrind
git clone git://sourceware.org/git/valgrind.git  
cd valgrind  
git checkout VALGRIND_3_16_1   
cd ..
# clone ctgrind
git clone git@github.com:ferhaterata/ctgrind.git
cd ctgrind
# patch ctgrind
patch --directory=../valgrind/ --strip=1 < valgrind.patch
cd ..
# install openmpi 4.0.6
# first download https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.6.tar.gz
gunzip -c openmpi-4.0.6.tar.gz | tar xf -
cd openmpi-4.0.6
# ./configure Open MPI v4.0.6 with --enable-mpi1-compatibility (in ubuntu 20.04 open mpi is not compatible).
./configure --enable-mpi1-compatibility --prefix=/opt/openmpi-4.0.6
make
sudo make install
cd ..
# configure and make valgrind with ctgrind patch
cd valgrind
./configure --with-mpicc=/opt/openmpi-4.0.6/bin/mpicc --prefix=/opt/valgrind-3.16.1.ctgrind-patch
make
sudo make install
cd ..
# run test
cd ctgrind
make
../valgrind/vg-in-place ./a.out 
```
