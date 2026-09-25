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

The experiment was performed using two equivalent Ubuntu virtual machines: one running on **Proxmox VE (Type-1)** and the other on **VMware Workstation (Type-2)**. The same CPU benchmark was executed in both environments.

### Step 1: Virtual Machine Creation & Setup

#### 1. Proxmox VE — Type-1 Hypervisor

- Open the Proxmox VE web interface in a browser.
- Select **Create VM**.
- Attach the Ubuntu ISO image as the installation media.
- Configure the virtual machine with:
  - **CPU:** 2 vCPU
  - **Memory:** 2048 MiB (2 GB)
  - **Disk:** 20 GB
  - **Network:** `vmbr0`
- Complete the Ubuntu installation.
- Start the virtual machine after installation.

#### 2. VMware Workstation — Type-2 Hypervisor

- Launch **VMware Workstation** on the host operating system.
- Select **Create a New Virtual Machine**.
- Select **Typical Configuration**.
- Attach the Ubuntu ISO image.
- Configure the virtual machine with:
  - **Processors:** 1 processor × 2 cores = 2 vCPU
  - **Memory:** 2 GB
  - **Disk:** 20 GB
  - **Network:** NAT
- Complete the Ubuntu installation.
- Start the virtual machine after installation.

> **Note:** Both virtual machines use the same guest operating system and equivalent CPU, memory, and disk resources to maintain consistency during comparison.


### Step 2: System Configuration Verification

After logging into Ubuntu, verify the virtual machine configuration before running the benchmark.

Run the following commands in the Ubuntu terminal:

```bash
# 1. Verify hostname and system information
hostnamectl

# 2. Verify CPU configuration
lscpu

# 3. Verify memory allocation
free -h

# 4. Verify disk allocation
df -h

# 5. Monitor CPU and system resource utilization
top
```

Verify that the VM has approximately:

- **2 vCPU**
- **2 GB RAM**
- **20 GB virtual disk**

Press `q` to exit `top`.


### Step 3: Sysbench Benchmark Installation & Execution

Install Sysbench inside each Ubuntu virtual machine.

```bash
# Update package repository
sudo apt update

# Install Sysbench
sudo apt install sysbench -y

# Verify Sysbench installation
sysbench --version
```

Run the CPU benchmark using the same configuration on both virtual machines:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The benchmark performs CPU-intensive prime-number calculations up to `20,000`.

Record the following values from the output:

- Total execution time
- Total events
- Events per second
- Minimum latency
- Average latency
- Maximum latency


### Step 4: Record Benchmark Results

Record the Sysbench output separately for each hypervisor.

#### Proxmox VE — Type-1

| Metric | Result |
|---|---:|
| Total Execution Time | 10.0030 s |
| Total Events | 14,548 |
| Events per Second | 1,453.98 |
| Minimum Latency | 0.57 ms |
| Average Latency | 0.69 ms |
| Maximum Latency | 1.24 ms |

#### VMware Workstation — Type-2

| Metric | Result |
|---|---:|
| Total Execution Time | 10.0006 s |
| Total Events | 7,077 |
| Events per Second | 707.43 |
| Minimum Latency | 1.16 ms |
| Average Latency | 1.41 ms |
| Maximum Latency | 9.00 ms |


### Step 5: Performance Comparison

The recorded results from both environments are compared using:

- **Events per second** — CPU processing throughput
- **Total events** — Number of completed benchmark operations
- **Average latency** — Typical processing latency
- **Minimum latency** — Lowest observed latency
- **Maximum latency** — Highest observed latency

The results are then visualized using individual performance graphs and a multi-panel evaluation dashboard.
7. Recorded the execution time, total events, events per second, and latency statistics.

### Experimental Consistency

The same Sysbench CPU workload and equivalent virtual machine configurations were used for both hypervisors. This provides a consistent basis for comparing the measured CPU performance of the Type-1 and Type-2 virtualization environments.

## Performance Results

The CPU benchmark was executed on both virtual machines using the same Sysbench configuration:

```bash
sysbench cpu --cpu-max-prime=20000 run
```
#### Proxmox VE — Type-1

| Metric | Result |
|---|---:|
| Total Execution Time | 10.0030 s |
| Total Events | 14,548 |
| Events per Second | 1,453.98 |
| Minimum Latency | 0.57 ms |
| Average Latency | 0.69 ms |
| Maximum Latency | 1.24 ms |

#### VMware Workstation — Type-2

| Metric | Result |
|---|---:|
| Total Execution Time | 10.0006 s |
| Total Events | 7,077 |
| Events per Second | 707.43 |
| Minimum Latency | 1.16 ms |
| Average Latency | 1.41 ms |
| Maximum Latency | 9.00 ms |

## Performance Comparison

The benchmark results from both hypervisors are compared using CPU throughput and latency metrics.

### Comparison Table

| Metric | Proxmox VE (Type-1) | VMware Workstation (Type-2) |
|---|---:|---:|
| Total Execution Time | 10.0030 s | 10.0006 s |
| Total Events | 14,548 | 7,077 |
| Events per Second | 1,453.98 | 707.43 |
| Minimum Latency | 0.57 ms | 1.16 ms |
| Average Latency | 0.69 ms | 1.41 ms |
| Maximum Latency | 1.24 ms | 9.00 ms |

### Throughput Comparison

The measured **events per second** were:

- Proxmox VE: **1,453.98 events/sec**
- VMware Workstation: **707.43 events/sec**

Under the tested configuration and workload, Proxmox VE processed approximately **2.05 times** as many benchmark events per second as VMware Workstation.

### Latency Comparison

The measured latency values were:

- **Minimum latency:** 0.57 ms vs 1.16 ms
- **Average latency:** 0.69 ms vs 1.41 ms
- **Maximum latency:** 1.24 ms vs 9.00 ms

The results show lower measured latency values for the Proxmox VE test across all three latency measurements.

### Execution Time

The total execution times were approximately **10 seconds** in both environments:

- Proxmox VE: **10.0030 seconds**
- VMware Workstation: **10.0006 seconds**

Since the Sysbench CPU test runs for approximately the same benchmark duration in both cases, execution time alone does not provide as much differentiation as events per second and latency.

> **Note:** These observations describe the measured results for this specific experiment, VM configuration, host environment, and Sysbench workload. They should not be interpreted as universal performance characteristics of all Type-1 or Type-2 hypervisors.

## Performance Dashboard

The benchmark results are visualized using Python and Matplotlib to make the performance differences easier to interpret.

### Events per Second

<img width="536" height="330" alt="events_per_sec" src="https://github.com/user-attachments/assets/512b7ea9-0528-4378-8650-09344bc6659f" />

This graph compares the CPU processing throughput measured in events per second.

### Total Events

<img width="533" height="334" alt="events" src="https://github.com/user-attachments/assets/9db1e55d-10e0-4ddc-a180-7aaf3970b0e2" />

This graph compares the total number of benchmark events completed during the test.

### Latency Comparison

<img width="601" height="334" alt="cpu_latency" src="https://github.com/user-attachments/assets/37502a7d-5bd3-4714-850c-652f7f2f6d58" />

This graph compares the minimum, average, and maximum latency observed for each hypervisor.

### Combined Performance Dashboard

<img width="521" height="332" alt="overall_evaluation" src="https://github.com/user-attachments/assets/a2c90745-f30e-48be-b3c5-fbcbcf86fc0e" />

The combined dashboard provides an overall visual comparison of:

- CPU throughput
- Total events processed
- Average latency
- Minimum, average, and maximum latency

The visualizations are based directly on the Sysbench measurements recorded during the experiment.

## Technical Analysis

The benchmark results provide an experimental comparison of CPU performance under the same workload and equivalent virtual machine resources.

### 1. CPU Throughput

The **events per second** metric represents the number of benchmark operations completed per second.

| Hypervisor | Events/sec |
|---|---:|
| Proxmox VE | 1,453.98 |
| VMware Workstation | 707.43 |

For this experiment, Proxmox VE recorded approximately **2.05× the CPU throughput** measured on VMware Workstation.

### 2. Total Events

The total number of completed benchmark events was:

- **Proxmox VE:** 14,548 events
- **VMware Workstation:** 7,077 events

The higher event count corresponds to the higher measured throughput during the benchmark execution.

### 3. Average Latency

Average latency represents the typical time taken to process a benchmark event.

| Hypervisor | Average Latency |
|---|---:|
| Proxmox VE | 0.69 ms |
| VMware Workstation | 1.41 ms |

The Proxmox VE measurement was lower in this test.

### 4. Maximum Latency

Maximum latency represents the highest latency observed during the benchmark.

| Hypervisor | Maximum Latency |
|---|---:|
| Proxmox VE | 1.24 ms |
| VMware Workstation | 9.00 ms |

The VMware Workstation test recorded a higher maximum latency, indicating a larger observed latency spike during the benchmark.

### 5. Overall Observation

Under the tested configuration and Sysbench CPU workload:

- Proxmox VE recorded higher measured CPU throughput.
- Proxmox VE recorded lower minimum, average, and maximum latency.
- Both environments completed the benchmark in approximately 10 seconds.
- The results demonstrate how virtualization architecture and the specific execution environment can affect measured CPU performance.

These observations are specific to the experimental setup and should not be generalized to every workload or hardware configuration.

## Conclusion

This experiment compared the CPU performance of a **Type-1 hypervisor (Proxmox VE)** and a **Type-2 hypervisor (VMware Workstation)** using the Sysbench CPU benchmark.

Both virtual machines were configured with equivalent resources:

- Ubuntu Linux
- 2 vCPU
- 2 GB RAM
- 20 GB virtual disk

The same Sysbench workload was executed in both environments:

```bash
sysbench cpu --cpu-max-prime=20000 run
```
Under the tested configuration and workload, the measured results showed:

Proxmox VE achieved 1,453.98 events/sec.
VMware Workstation achieved 707.43 events/sec.
Proxmox VE recorded an average latency of 0.69 ms.
VMware Workstation recorded an average latency of 1.41 ms.
The maximum observed latency was 1.24 ms for Proxmox VE and 9.00 ms for VMware Workstation.
Both benchmark executions completed in approximately 10 seconds.

The experiment demonstrates how virtualization architecture can influence CPU performance measurements. However, the results are specific to the hardware, VM configuration, host environment, and benchmark workload used in this experiment.

The experiment also provides practical experience with:

      Type-1 and Type-2 virtualization
      
      Virtual machine configuration
      
      Ubuntu VM administration
      
      Sysbench benchmarking
      
      CPU performance measurement
      
      Experimental comparison and analysis.


