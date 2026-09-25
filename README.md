# Performance Analysis of Type 1 and Type 2 Hypervisors

## Executive Summary

This repository contains the complete experimental setup, empirical benchmark data, performance visualization, and technical report comparing the CPU performance of a **Type-1 Bare-Metal Hypervisor (Proxmox VE)** and a **Type-2 Hosted Hypervisor (VMware Workstation)**.

Both hypervisors were deployed with identically configured **Ubuntu Virtual Machines** (2 vCPU, 2 GB RAM, 20 GB Disk). The standard `sysbench` CPU prime-number calculation benchmark (`--cpu-max-prime=20000`) was executed on both virtual machines under identical workload conditions.

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Hypervisor Architecture](#hypervisor-architecture)
- [Virtual Machine Configuration](#virtual-machine-configuration)
- [Experimental Procedure](#experimental-procedure)
- [Benchmark Configuration](#benchmark-configuration)
- [Performance Results](#performance-results)
- [Performance Comparison](#performance-comparison)
- [Performance Dashboard](#performance-dashboard)
- [Technical Analysis](#technical-analysis)
- [Conclusion](#conclusion)
- [Repository Structure](#repository-structure)

  
## Project Overview

This project presents a comparative performance analysis of **Type-1 and Type-2 hypervisors** using a CPU-intensive benchmark.

The experiment compares **Proxmox VE**, a Type-1 (bare-metal) hypervisor, with **VMware Workstation**, a Type-2 (hosted) hypervisor. Both environments use equivalent virtual machine configurations and the same **Sysbench CPU benchmark** to maintain a consistent testing environment.

The performance is evaluated using key metrics such as:

- Events per second
- Total events processed
- Minimum latency
- Average latency
- Maximum latency

The results are analyzed to understand the performance characteristics of both virtualization approaches under the tested workload.


## Objectives

The main objective of this experiment is to evaluate and compare the CPU performance of a **Type-1 hypervisor** and a **Type-2 hypervisor** under the same benchmark workload.

### Hypervisors

| Hypervisor | Type | Architecture |
|---|---|---|
| Proxmox VE | Type-1 | Bare-metal hypervisor |
| VMware Workstation | Type-2 | Hosted hypervisor |

### Benchmark

The **Sysbench CPU benchmark** is used to generate a consistent CPU workload in both virtualized environments.

```bash
sysbench cpu --cpu-max-prime=20000 run
```

## Hypervisor Architecture

### Type-1 Hypervisor — Proxmox VE

```text
┌─────────────────────────────────────┐
│        Virtual Machine (VM)         │
│          Ubuntu Linux OS            │
├─────────────────────────────────────┤
│         Virtual Hardware            │
│       2 vCPU | 2 GB RAM             │
├─────────────────────────────────────┤
│            Proxmox VE               │
│         Type-1 Hypervisor           │
├─────────────────────────────────────┤
│          Physical Hardware          │
│       CPU | RAM | Storage | NIC     │
└─────────────────────────────────────┘
```

### Type-2 Hypervisor — VMware Workstation

```text
┌─────────────────────────────────────┐
│        Virtual Machine (VM)         │
│          Ubuntu Linux OS            │
├─────────────────────────────────────┤
│         Virtual Hardware            │
│       2 vCPU | 2 GB RAM             │
├─────────────────────────────────────┤
│       VMware Workstation            │
│         Type-2 Hypervisor           │
├─────────────────────────────────────┤
│       Host Operating System         │
├─────────────────────────────────────┤
│          Physical Hardware          │
│       CPU | RAM | Storage | NIC     │
└─────────────────────────────────────┘
```

## Virtual Machine Configuration

To ensure a fair comparison, both virtual machines were configured with equivalent hardware resources and the same guest operating system.

| Parameter | Proxmox VE (Type-1) | VMware Workstation (Type-2) |
|---|---|---|
| Guest Operating System | Ubuntu Linux | Ubuntu Linux |
| CPU Allocation | 2 vCPU | 2 vCPU |
| Memory Allocation | 2 GB RAM | 2 GB RAM |
| Disk Allocation | 20 GB | 20 GB |
| Network Configuration | vmbr0 | NAT |

The same virtual machine resource configuration was maintained across both hypervisors so that the Sysbench CPU benchmark results could be compared under equivalent conditions.

## Experimental Procedure

The experiment was performed separately on the Type-1 and Type-2 hypervisor environments using equivalent virtual machine configurations.

### Type-1 — Proxmox VE

1. Create a virtual machine using the Proxmox VE web interface.
2. Install Ubuntu Linux as the guest operating system.
3. Configure the VM with **2 vCPU, 2 GB RAM, and 20 GB disk**.
4. Start the virtual machine and verify the allocated resources.
5. Install Sysbench inside the Ubuntu VM.
6. Execute the CPU benchmark:
   ```bash
   sysbench cpu --cpu-max-prime=20000 run
   ```

### Type-2 — VMware Workstation

1. Created a virtual machine using VMware Workstation.
2. Installed Ubuntu Linux as the guest operating system.
3. Configured the VM with 2 vCPU, 2 GB RAM, and 20 GB disk.
4. Started the VM and verified the allocated resources.
5. Installed Sysbench inside the Ubuntu VM.
6. Executed the same CPU benchmark:
   ```bash
   sysbench cpu --cpu-max-prime=20000 run
   ```
7. Recorded the execution time, total events, events per second, and latency statistics.

### Experimental Consistency

The same Sysbench CPU workload and equivalent virtual machine configurations were used for both hypervisors. This provides a consistent basis for comparing the measured CPU performance of the Type-1 and Type-2 virtualization environments.
