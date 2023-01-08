# SRIOV
SRIOV Guide 

Single Root I/O Virtualization (SR-IOV) is a technology that allows a physical PCI Express (PCIe) device to appear as multiple virtual devices to the operating system. This can be used to provide direct access to the device to specific virtual machines (VMs) in a cloud computing environment, while still allowing the device to be used by the host operating system. SR-IOV allows multiple VMs to share a single physical device, which can improve performance and resource utilization compared to using device emulation. It is commonly used with network devices, such as network interface cards (NICs), but can also be used with other types of PCIe devices.

## How to enable SRIOV
### Some Points to remember
* Motherboard and CPU have enabled Vt-x and VT-d (on Intel) or SVM and IOMMU (on AMD). 
* SR-IOV needs to be supported too.
* The network card has to be SR-IOV capable. That basically means a server grade NIC is required. 

`Note: sometimes you don't see the option to enable SRIOV, this can be due to your machine already enabled with SRIOV or it does'nt support this.`
![image](https://user-images.githubusercontent.com/81817735/211187200-5282d24b-ef7f-4a7a-84e8-2bbed0603c84.png)
![image](https://user-images.githubusercontent.com/81817735/211187212-66774930-b2d6-4b54-9892-b71c258d1fc1.png)

### Enable SRIOV in ubuntu grub
* Add `intel_iommu=on` or `amd_iommu=on` to grub (`/etc/default/grub` on Ubuntu).
![image](https://user-images.githubusercontent.com/81817735/211187377-821b8f04-29d4-4f7a-a9b9-dd5dbdd0713b.png)

* Update grub (Run: `update-grub`   on Ubuntu)

## Activate VFs for your NIC

### Run `lspci -k` and find your NIC and locate its Driver
in my case
![image](https://user-images.githubusercontent.com/81817735/211187494-c8e075fb-746e-4259-9da6-dc56903b2554.png)

### Put no of VFs in the following command.

Put options [NAME_OF_DRIVER] max_vfs=2 to /etc/modprobe.d/[NAME_OF_DRIVER].conf
eg `echo "ixgbe max_vfs=22 >> /etc/modprobe.d/ixgbe.conf`

### Regenerate initramfs
Run: `update-initramfs -u`
