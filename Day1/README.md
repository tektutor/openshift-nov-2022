# Day 1 ( 4 Hours )

## What is Hypervisor ?
- is a general reffered to the Virtualization Technology
- allows running many Operating Systems side by side on the same Desktop/Laptop/Workstation/Server
- many OS can actively run on the same machine
- Processors
  - Intel Processor - Virtualization feature - VT-X
  - AMD Processor - Virtualization feature - AMD-V
- there are two types 
  1. Type 1 - Bare Metal Hypervisor ( VMWare vSphere/V-Center )
  2. Type 2 - VMWare Workstation, VMWare Fusion(Mac), Oracle VirtualBox, Microsoft Hyper-V
- Type 1 is used in Servers/Workstations ( This doesn't require Host OS )
- Type 2 is used in Laptop/Desktop/Workstations ( This requires Host OS )
- This type of Virtualization is called Heavy-weight Virtualization
  Each VM(Guest OS) are allocated with dedicated 
    - Hardware resources ( CPU, RAM and Hard Disk[Storage] )

## What are Containers ?
- this is an application virtualization technology
- lightweight virtualization technology
- based on Linux Kernel Feature
  1. Namespace - used to isolate one container from other container
  2. Control Group (CGroups) - used to apply resource quota restrictions on a container-level

## How Containers are different from Virtual Machines ?
- Virtual Machines are aka Guest OS
- VMs are fully function Operating System with dedicated hardwares
- Guest OS has its own dedicated OS Kernel
- Containers are application process that runs in a separate namespace
- Containers are not Operating System, it just represents a single application
- Guest OS can run many application process including many containers
- Container are never going to replace Virtual Machines, they complement each other

## What is Container Runtime ?
- Examples
  runC is a container Runtime
  CRI-O is a container Runtime
- Container Runtimes help us manage containers
  - creating containers
  - listing containers
  - deleting containers
  - modifying containers
  - killing/aborting containers
  - stop/start/restarting containers
- Container Runtimes are used by Container Engines to manage containers

## What is Container Engine ?
- Container Engine provides user-friendly commands to use create and manage containers and container images without worrying about low-level OS Kernel/Container technology details
- end-users use Container Engine
- Docker depends on runC Container Runtime to manage containers
- Docker also depends on other tools to manage Container Images
- Examples
  - Docker
  - Podman

## What is Container Orchestration Platform ?

## What is Kubernetes ?

## Kubernetes High-Level Architecture

## What is OpenShift ?

## OpenShift High-Level Architecture


