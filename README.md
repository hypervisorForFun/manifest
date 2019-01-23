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



# Repo manifest for HypervisorForFun development

In the HypervisorForFun project we try to gather all technical documentation under the
(https://github.com/hypervisorForFun) git and the build related
instructions for full setups at (https://github.com/hypervisorForFun/build).

Having that said, there are a couple of guidelines and rules that we want to
try to follow when it comes to managing the manifests in this git.

# 1. Remotes
Since most of our projects can be found on GitHub, we are using that as the main
remote. If you need to include other remotes for some reason, then that is OK,
but please double check of there is any **maintained** (and preferably
official) mirror for the project at GitHub before adding a new remote.

# 2. Layout of contents
#### 2.1 Sections
To have some kind of structure of the files, we have split them up in three
sections, one for pure linux gits, one for xen supporting gits found at
linaro-swg and then a third, `misc` section where everything else can be found.
I.e., a template looks like this (this also includes the default remote for
clarity):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
        <remote name="github" fetch="https://github.com" />

        <default remote="github" revision="master" />

        <!-- linaro-swg gits -->
        <!-- Misc gits -->
</manifest>
```

#### 2.2 Project XML elements
All `<projects ... >` lines should be on the format as shown below with the
attributes in this order. The reason for this is to have it uniformly done
across all manifests and that it will make it easier when comparing various
versions of manifests with diff tools. All three attributes are **mandatory**.
The only exception is `revision` which does not have to be stated if it is
`master` that we are tracking.

```xml
        <project path="name_and_path_on_disk" name="upstream_name.git" revision="git_revsion" />
```
#### 2.3 Alphabetic order
Within each of the three sections, all `<project ... >` lines shall be sorted in
alphabetic order (this is again for making it easier to diff manifests). The
only expection here is `build.git` which uses the `linkfile` element. Having
that at the end makes it look cleaner.

#### 2.4 Additional XML attributes
If you are using another remote than the default, then that should come
**after** the `revision` attribute (this is true for all attributes other than
the `path`, `name` and `revision`).

#### 2.5 Alignment of XML attributes
The three mandatory XML attributes `path`, `name` and `revision` should be
column aligned. Alignment of additional XML attributes are optional.

#### 2.6 When to use clone-depth="1"?
With `clone-depth="1"` you are telling `repo` and `git` that you only want a
certain commit and not the entire git log history. You can only use this under
two conditions and that is when `revision` is either a branch or a tag. Pure
`SHA-1's` does not work and will even raise `repo` and `git` sync errors in
some cases. So, the rules are, if you use either `revision="refs/tags/my_tag"`
or `revision="refs/heads/my_branch"`, then you shall add `clone-depth="1"` right
after the `revision` attribute.

#### 2.7 Spaces or tabs?
Only use spaces!

#### 2.8 Example showing the basis for an HypervisorForFun manifest
Here are fictive names etc, but it describes everything said above.
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>
  <remote fetch="https://git.kernel.org/pub/scm/linux/kernel/git/stable" name="linux"/>
  <remote fetch="https://github.com/hypervisorForFun" name="hypervisorForFun"/>
  <remote fetch="https://github.com/qemu" name="qemu"/>
  <remote fetch="https://xenbits.xen.org/git-http" name="xen" />
  <remote fetch="https://github.com/linaro-swg" name="linaro-swg"/>
  <remote fetch="https://github.com/mirror" name="busybox"/>
  <default remote="hypervisorForFun" revision="master"/>
  
  <project name="build" path="build" />
  
  <project name="linux" path="linux" remote="linux" revision="refs/tags/v4.19.16" />
  
  <project name="xen" path="xen" remote="xen" revision="refs/tags/RELEASE-4.11.1" />
  
  <project name="qemu" path="qemu" remote="qemu" revision="refs/tags/v3.0.0" />
  
  <project name="soc_term" path="soc_term" remote="linaro-swg" revision="5493a6e7c264536f5ca63fe7511e5eed991e4f20"/>
  
  <project name="gen_rootfs" path="gen_rootfs" remote="hypervisorForFun" revision="refs/tags/v1.0"/>
  
  <project name="bios_qemu_tz_arm" path="bios_qemu_tz_arm" remote="linaro-swg" revision="0271acbfa81d93e64c2fdd6ae8b0be14a8c45719"/>
  
  <project name="busybox" path="busybox" remote="busybox" revision="refs/tags/1_29_0"/>
</manifest>

```
