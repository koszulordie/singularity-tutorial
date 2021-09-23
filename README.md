# bbg-agora: Singularity Tutorial


# Goal

- Installing Singularity 3.7.4
- Running a few examples
- Understanding by doing


# Main reference

[Singularity User Guide](https://sylabs.io/guides/3.7/user-guide/)


# Installation

You will need a Linux system to run Singularity natively.

Users of Mac or Windows machines should read deeper documentation. See [Quick Installation Steps](https://sylabs.io/guides/3.7/user-guide/quick_start.html#quick-installation-steps) and subsequent links.


### 1. Install system dependencies

```
$ bash system-dependencies.sh
```


### 2. Install Go

```
$ bash install-go.sh
$ echo 'export PATH=/usr/local/go/bin:${PATH}' >> ~/.bashrc
$ source ~/.bashrc
```


### 3. Get Singularity source code

```
$ git clone https://github.com/hpcng/singularity.git
$ cd singularity
$ git checkout release-3.7
```


### 4. Compile Singularity from source code

```
$ ./mconfig && make -C builddir && sudo make -C builddir install
```


### Troubleshooting

At compilation the following Go error might occur:

```
checking: host Go compiler (at least version 1.13)... not found!
```

One possible solution found [here](https://github.com/hpcng/singularity/issues/4765) is to completely
remove the /usr/local/go directory like

```
$ sudo rm -rf /usr/local/go
```

then repeat step 2 above.



# Examples


### 1. Toy example: pre-computed images from external resources


Download pre-built images from an external resource like the [Container Library](https://cloud.sylabs.io/library) 
or [Docker Hub](https://hub.docker.com/). 

The following command will create a new file ubuntu.sif (sif ~ Singularity Image File).

```
$ singularity build ubuntu.sif library://ubuntu
```

Now let's do our first interaction with an image, like so:

```
$ singularity shell ubuntu.sif
```

What do we get as file system? 

Let's check the OS version with:

```
$ cat /etc/os-release
```

You can get images from other OS, like Debian

```
$ singularity build library://debian
```


### 2. Toy example: running programs from Singularity images

```
$ singularity build lolcow.sif docker://godlovedc/lolcow
```

```
$ singularity shell lolcow.sif
> echo $PATH
> which cowsay
> cowsay moo!
```

```
$ singularity exec lolcow.sif cowsay moo
```


### 3. Toy example: Singularity images can run by themselves!


```
$ singularity run lolcow.sif
```


### 4. Making sure we can use this in the bbg-cluster

```
rsync --progress debian.sif /workspace/users/<username>/
```

Open an interactive session in the bbg-cluster.

```
interactive
```

```
/opt/singularity/bin/singularity --version
/opt/singularity/bin/singularity shell debian.sif
```


### 5. Toy example: definition files a.k.a. recipes


```
echo lolcow.def
```

Recipes can be used to create a Singularity image:

```
sudo singularity build new-lolcow.sif lolcow.def
```


### 6. A real life example: legacy code


Recently we tried to use the [MEME Suite](https://meme-suite.org/meme/). However intalling it seemed quite a dirty task requiring many dependencies. 
Moreover we want it to run in the cluster, which is a computing environment subject to many installation restrictions.

```
echo meme-suite.def
```

### 7. Another real life example: boostDM


[boostDM docs](https://boostdm.readthedocs.io/en/latest/)

[boostDM analyses](https://github.com/bbglab/boostdm-analyses)

[boostDM pipeline](https://bitbucket.org/bbglab/boostdm/src/release/)
