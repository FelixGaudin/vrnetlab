vrnetlab / Cisco CSR1000v
===========================
This is the vrnetlab docker image for Cisco CSR1000v.

On installation of CSR1000v the user is presented with the choice of output,
which can be over serial console, a video console or through automatic detection of one or the other. 
Empirical studies show that the automatic detection is far from infallible and 
so we force the use of the serial console by feeding the VM an .iso image 
that contains a small bootstrap configuration that sets the output to serial console. 
This means we have to boot up the VM once to feed it this configuration and then restart it 
for the changes to take effect. Unfortunately docker doesn't allow us to run docker build 
with --privileged so there is no KVM acceleration making this process excruciatingly slow 
were it to be performed in the docker build phase. 
Instead we build a basic image using docker build, which essentially just assembles 
the required files, then run it with --privileged to start up the VM and feed it the .iso image. 
After we are done we shut down the VM and commit this new state into the final docker image. 
This is unorthodox but works and saves us a lot of time

Building the docker image
-------------------------

Put the .qcow2 file in this directory and run `make docker-image` and
you should be good to go. The resulting image is called `vr-csr`. You can tag
it with something else if you want, like `my-repo.example.com/vr-csr` and then
push it to your repo. The tag is the same as the version of the CSR image, so
if you have csr1000v-universalk9.16.04.01.qcow2 your final docker image will be called
vr-csr:16.04.01

It's been tested to boot and respond to SSH with:

 * 16.03.01a (csr1000v-universalk9.16.03.01a.qcow2)
 * 16.04.01 (csr1000v-universalk9.16.04.01.qcow2)

Usage
-----
```
docker run -d --privileged --name my-csr-router vr-csr
```

Interface mapping

| vr-csr | vr-xcon |
| :---:  |  :---:  |
| Gi2 | 10 |
| Gi3 | 1 |
| Gi4 | 2 |
| Gi5 | 3 |
| Gi6 | 4 |
| Gi7 | 5 |
| Gi8 | 6 |
|Gi9 | 7 |
|Gi10| 8 |
|Gi11| 9 |

System requirements
-------------------
CPU: 1 core

RAM: 4GB

Disk: <500MB


FUAQ - Frequently or Unfrequently Asked Questions
-------------------------------------------------
##### Q: Has this been extensively tested?
A: Nope. 