These are the Ubuntu Core images as [downloadable from Ubuntu][1]. See the [Ubuntu Core wiki page][2] for more information. The following Ubuntu versions will be built:

* 14.04, trusty
* 13.10, saucy
* 12.04, precise

The images were made using the script [build-all.sh](build-all.sh).

## Usage

Just use `FROM mazzolino/armhf-ubuntu:<VERSION>` in your Dockerfile to
use [pre-built images from the Docker
index](https://index.docker.io/u/mazzolino/armhf-ubuntu/). I will update the images
regularly (I promise).

Alternatively, you can build and push your own images, as described in
the following section.

## Building

You can build and push your own version of the images to the Docker
index. Building is possible on armhf ("armv7l") as well as x86 machines
(64 bits only, as that is what Docker supports). See the section on
_Emulation support_ on how to install the prerequisites.

Build all distributions:

    ./build-all.sh

Build a specific Ubuntu version:

    ./build.sh 14.04

Images will automatically pushed as `<YOUR-DOCKER-USER>/armhf-docker`.

## Emulation support ##

The image includes the amd64 version of `qemu-arm-static`. This means you can build and run ARM containers on your 64bit machine, as explained in [this post][3]. On your host, you need to install [qemu-user-static][4]. Also, the following command must be executed before building or running any ARM containers (the build script does this automatically):

    sudo sh -c 'echo ":arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm-static:" >/proc/sys/fs/binfmt_misc/register'

## Tagging ##

Tip: On your arm device, additionally tag the images as **ubuntu** in order to be able to build Dockerfiles which use the ubuntu base image:

    docker tag mazzolino/armhf-ubuntu:14.04 ubuntu:14.04
    docker tag mazzolino/armhf-ubuntu:14.04 ubuntu:latest
    docker tag mazzolino/armhf-ubuntu:14.04 ubuntu:trusty

    docker tag mazzolino/armhf-ubuntu:13.10 ubuntu:13.10
    docker tag mazzolino/armhf-ubuntu:13.10 ubuntu:saucy

    docker tag mazzolino/armhf-ubuntu:12.04 ubuntu:12.04
    docker tag mazzolino/armhf-ubuntu:12.04 ubuntu:precise

    # Maybe also the stackbrew images
    docker tag mazzolino/armhf-ubuntu:12.04 stackbrew/ubuntu:12.04
    [...]

Look at the [official Ubuntu image description](https://index.docker.io/_/ubuntu/) to see what else should be tagged.

  [1]: http://cdimage.ubuntu.com/ubuntu-core/releases/
  [2]: https://wiki.ubuntu.com/Core
  [3]: https://groups.google.com/forum/#!msg/coreos-dev/YC-G_rVFnI4/ncS5bjxYWdc
  [4]: https://wiki.debian.org/QemuUserEmulation
