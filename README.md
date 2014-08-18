# kubernetes CoreOS libvirt

This project aims at automatizing the setup of a kubernetes cluster on top of a CoreOS cluster running on virtual machines provisioned via libvirt/KVM/QEMU

## Usage

### Download the materials

This has to be done only once.
This has to be done as root as the downloaded materials need to be put in the libvirt images directory which is usually not world-writable.

```bash
su
./prepare_cluster
exit # from the root shell
```

This script will:
- Enable [KSM](https://www.kernel.org/doc/Documentation/vm/ksm.txt)
- Download the latest QEMU CoreOS image and put it in the directory corresponding to the default libvirt pool storage (usually `/var/lib/libvirt/images/`)
- Download the latest kubernetes binaries and put them in the directory corresponding to the default libvirt pool storage (usually `/var/lib/libvirt/images/`)

Relaunching this script will check if the materials have been updated and download them again only if a new version is available.

### Start a cluster with 5 VMs.

```bash
./boot_cluster
```

This script instantiates, through libvirt:
- Two networks (Check the [kubernetes networking guide](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/networking.md) for further details)
- A volume for each VM
- All the VM instances

If `boot_cluster` fails, try to do a `destroy_cluster` in order to clean the libvirt objects.

### Play with the kubernetes cluster

The master is reachable at 192.168.10.1.
The VMs have automatically been fed with your ssh public keys

```bash
ssh core@192.168.10.1
/opt/kubernetes/bin/kubecfg list minions
```

### Stop the cluster

```bash
./destroy_cluster
```
