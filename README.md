This document is about how to use git hub to pull all source code about hypervisor for kvm and xen,
It also include the build system to create a hyp enviroment by qemu. This project will include 2 parts:
1. how to pull code of this project
2. how to use building system in this project

1. how to pull code of this projet:
please use below command to pull all source code:

repo init -u https://github.com/hypervisorForFun/manifest.git -m default.xml --repo-url=git://codeaurora.org/tools/repo.git
repo sync

2. How to use building system in this project
A. download toolchain by below command:
   make -f toolchain.mk toolchains
   
B. build system:
   cd build;make -f qemu_v8.mk all
