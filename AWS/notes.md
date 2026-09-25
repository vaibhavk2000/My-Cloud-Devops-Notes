# What is Virtualization?

Virtualization is the process of creating a virtual version of physical hardware, such as servers, storage devices, or networks.also it allow to run multiple virtual machine on a single physical machine.

# Types of Virtualization?
Server Virtualization: abstracts physical server hardware (CPU, RAM, storage) into multiple independent virtual machines (VMs), allowing multiple operating systems to run on a single physical host.

OS-Level Virtualization (Containerization): shares the host OS kernel to run isolated user-space instances (e.g., Docker, LXC). It is much lighter and faster than traditional full-hardware virtualization.

Network Virtualization: decouples network management from physical hardware. Creates virtual switches, routers, firewalls, and VPNs (e.g., VMware NSX, AWS VPC).

Storage Virtualization: pools physical storage from multiple network storage devices into what appears to be a single storage unit (e.g., SAN/NAS virtualization, Software-Defined Storage like Ceph).

Desktop Virtualization (VDI): hosts desktop environments on a centralized server, allowing users to access their virtual desktops remotely (e.g., Citrix Virtual Desktops, VMware Horizon).

# Types of Hypervisor

```mermaid
graph TD
    %% Type 2 Hypervisor Architecture
    subgraph Type2 [Type 2: Hosted Hypervisor]
        T2_HW[Physical Hardware<br>CPU, RAM, Storage, Network] --- T2_OS[Host OS<br>Windows, Linux, macOS]
        T2_OS --- T2_Hyp[Hypervisor / VMM<br>e.g., VirtualBox, VMware Workstation]
        T2_Hyp --> T2_VM1[Guest OS / VM 1]
        T2_Hyp --> T2_VM2[Guest OS / VM 2]
        T2_VM1 --> T2_App1[Applications]
        T2_VM2 --> T2_App2[Applications]
    end
    %% Type 1 Hypervisor Architecture
    subgraph Type1 [Type 1: Bare-Metal Hypervisor]
        T1_HW[Physical Hardware<br>CPU, RAM, Storage, Network] --- T1_Hyp[Hypervisor / VMM<br>e.g., VMware ESXi, KVM, Hyper-V]
        T1_Hyp --> T1_VM1[Guest OS / VM 1]
        T1_Hyp --> T1_VM2[Guest OS / VM 2]
        T1_VM1 --> T1_App1[Applications]
        T1_VM2 --> T1_App2[Applications]
    end

    %% Styling
    style T1_HW fill:#2c3e50,stroke:#333,stroke-width:2px,color:#fff
    style T1_Hyp fill:#2980b9,stroke:#333,stroke-width:2px,color:#fff
    style T1_VM1 fill:#27ae60,stroke:#333,stroke-width:1px,color:#fff
    style T1_VM2 fill:#27ae60,stroke:#333,stroke-width:1px,color:#fff
    style T2_HW fill:#2c3e50,stroke:#333,stroke-width:2px,color:#fff
    style T2_OS fill:#8e44ad,stroke:#333,stroke-width:2px,color:#fff
    style T2_Hyp fill:#2980b9,stroke:#333,stroke-width:2px,color:#fff
    style T2_VM1 fill:#27ae60,stroke:#333,stroke-width:1px,color:#fff
    style T2_VM2 fill:#27ae60,stroke:#333,stroke-width:1px,color:#fff
