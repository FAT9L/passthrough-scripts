# VFIO-resources
This is a small collection of some of the libvirt hooks and automation scripts I've written for my primary VFIO virtual machine.

NOTE: There is a high probability of this repo being outdated. If you have questions about anything listed here, open an issue or contact me directly. I try to keep it up-to-date, but there are a lot of minor optimizations that might be forgotten. 

## Specs:
* Host: Arch
* Guest: Win10 LTSC
* Kernel: 6.0.12.arch1-1
* Shell: bash 
* WM: i3
* DE: none
* DM: none
* CPU: Intel i7-12700KF @ 5.0GHz 
* GPU: RTX 4080
* Memory: 32GB 
* Using qemu-kvm (libvirt)
* Single GPU passthrough

## Passthrough items:
* "Primary" GPU (single card system)
* All P-cores, E-cores are reserved for host and I/O
* PCI audio controller + its IOMMU group
* USB mouse and keyboard


## Notes:
The "fix" for black screens on VM close is hacky at best, and a possible security issue at worst. I am attempting to find a better solution, but this seems to potentially be a kernel bug.

The main issue is the fact that the kernel doesn't seem to want to utilize the GPU after it's been unhooked from a guest machine. You will be returned to the tty/login manager/whatever (and can interact with them blindly), but have no video until xinit spins up and a new X session hooks into the framebuffer. 

I am not aware of another workaround - mine is one that I wrote on the fly and tested the kinks out of. Unless there's a better way to automate changing systemd unit configurations, this is how it's probably going to stay for me. I lack the knowledge and/or experience to dive deep into potential firmware/driver interactions that could be causing this. If YOU do though, please contact me (and the developers, obviously).
