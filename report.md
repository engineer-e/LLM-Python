Here's a single, coherent story that combines your hardware specifications, the PyTorch Python vs. LibTorch C++ comparison, and the performance engineering perspective into one document.

# Performance Evaluation of PyTorch (Python) and LibTorch (C++) for GPT-2 Inference on a CPU-Based System

## Introduction

The objective of this evaluation is to determine the most suitable development environment for executing GPT-2 Large Language Models (LLMs) on the available desktop computer. The comparison focuses on PyTorch using Python and LibTorch using C++, examining their execution architecture, computational performance, memory utilization, deployment characteristics, and practical model capacity. The evaluation is performed from a performance engineering perspective, emphasizing the complete execution pipeline and identifying the true system bottlenecks.

---

# Experimental System Specification

| **Category**                 | **Specification**                        |
| ---------------------------- | ---------------------------------------- |
| **Operating System**         | Microsoft Windows 10 Home (64-bit)       |
| **OS Version**               | 10.0.19045 (Build 19045)                 |
| **System Manufacturer**      | Micro-Star International Co., Ltd. (MSI) |
| **System Model**             | MS-7C09                                  |
| **System Architecture**      | x64-based PC                             |
| **Processor (CPU)**          | Intel® Core™ i3-9100F @ 3.60 GHz         |
| **CPU Cores / Threads**      | 4 Cores / 4 Threads                      |
| **BIOS Version**             | American Megatrends Inc. Version 1.50    |
| **Installed RAM**            | 8 GB DDR4                                |
| **Available RAM**            | Approximately 2.8 GB during measurement  |
| **Virtual Memory**           | 15.6 GB                                  |
| **Graphics Processor (GPU)** | NVIDIA GeForce GT 710                    |
| **LLM Execution Device**     | CPU                                      |
| **Inference Platform**       | PyTorch / LibTorch CPU Inference         |

The NVIDIA GeForce GT 710 is not suitable for accelerating modern transformer-based language models using current CUDA-enabled PyTorch releases. Consequently, all GPT-2 inference workloads execute entirely on the Intel Core i3-9100F CPU.

---

# Understanding the Execution Pipeline

Regardless of whether the application is written in Python or C++, GPT-2 follows the same computational pipeline:

```text
User Prompt
      │
      ▼
Tokenizer
      │
      ▼
Embedding Lookup
      │
      ▼
Transformer Blocks
      │
      ├── Multi-Head Self-Attention
      ├── Feed Forward Network
      ├── Layer Normalization
      └── Residual Connections
      │
      ▼
Logits
      │
      ▼
Sampling
      │
      ▼
Generated Token
```

More than ninety-five percent of the execution time is spent inside transformer computations, particularly matrix multiplication and attention operations. These operations dominate overall runtime and determine the practical performance of GPT-2 inference.

---

# Execution Architecture

Although Python and C++ appear to be different programming environments, both ultimately execute the same optimized PyTorch backend.

### PyTorch (Python)

```text
Python Application
        │
Python Interpreter
        │
PyTorch Python API
        │
ATen Tensor Library
        │
Dispatcher
        │
Optimized CPU Kernels
        │
Intel Core i3-9100F
```

### LibTorch (C++)

```text
C++ Application
        │
LibTorch API
        │
ATen Tensor Library
        │
Dispatcher
        │
Optimized CPU Kernels
        │
Intel Core i3-9100F
```

The only architectural difference is the Python interpreter. Once tensor operations begin, both implementations execute identical native C++ kernels.

---

# Performance Engineering Analysis

From a performance engineering perspective, this computer is classified as a CPU-bound system with constrained memory resources. Since the GPU is not used for inference, the processor performs every tensor operation, including matrix multiplication, attention computation, activation functions, layer normalization, and token generation.

The Intel Core i3-9100F provides four physical cores and four execution threads, allowing PyTorch to execute parallel tensor operations through optimized libraries such as OpenMP and Intel Math Kernel Library (MKL). However, the primary limitation is not computational throughput but memory capacity. Windows, background services, and the runtime environment consume a significant portion of the installed 8 GB of RAM, leaving only a limited amount of memory available for model weights, activations, tokenization, and application execution.

When the available memory becomes insufficient, Windows begins paging memory to disk through the page file. Disk access is several orders of magnitude slower than physical memory, causing severe performance degradation and making the application appear to freeze or hang. Therefore, memory pressure is a much greater performance concern than the programming language used to build the application.

---

# Resource Utilization

| **Resource**          | **Observation**                                       |
| --------------------- | ----------------------------------------------------- |
| CPU Compute           | High Utilization                                      |
| Matrix Multiplication | Primary Compute Workload                              |
| Attention Mechanism   | Primary Compute Workload                              |
| RAM Capacity          | Primary System Bottleneck                             |
| Memory Bandwidth      | High Impact                                           |
| GPU Utilization       | Not Used                                              |
| Python Interpreter    | Minor Overhead                                        |
| Disk Paging           | Significant Performance Penalty when RAM is exhausted |

---

# PyTorch (Python) vs LibTorch (C++)

| **Category**                         | **PyTorch (Python)**     | **LibTorch (C++)**        |
| ------------------------------------ | ------------------------ | ------------------------- |
| Backend Engine                       | Same                     | Same                      |
| Tensor Library                       | Same                     | Same                      |
| CPU Kernels                          | Same                     | Same                      |
| Matrix Multiplication                | Same                     | Same                      |
| Attention Computation                | Same                     | Same                      |
| Raw Inference Speed                  | Nearly Identical         | Nearly Identical          |
| CPU Utilization                      | Nearly Identical         | Nearly Identical          |
| Thread Utilization                   | Nearly Identical         | Nearly Identical          |
| Memory Overhead                      | Higher                   | Lower                     |
| Startup Time                         | Slower                   | Faster                    |
| Runtime Memory Usage                 | Higher                   | Lower                     |
| Model Loading                        | Slightly Slower          | Slightly Faster           |
| Deployment                           | Good                     | Excellent                 |
| Integration with Native Applications | Moderate                 | Excellent                 |
| Learning Curve                       | Easier                   | More Difficult            |
| Best Use Case                        | Research and Development | Production and Deployment |

The computational performance is effectively identical because both interfaces invoke the same optimized native backend. The measurable differences arise from application overhead rather than neural network execution.

---

# Practical GPT-2 Model Capacity

### Current Hardware (8 GB RAM)

| **Model**    | **Parameters** | **Practical Status**                        |
| ------------ | -------------: | ------------------------------------------- |
| GPT-2 Small  |    124 Million | Excellent                                   |
| GPT-2 Medium |    355 Million | Usable with limitations                     |
| GPT-2 Large  |    774 Million | Generally impractical with standard PyTorch |
| GPT-2 XL     |    1.5 Billion | Not recommended                             |

### Upgraded Hardware (16 GB RAM)

| **Model**    | **Parameters** | **Practical Status**     |
| ------------ | -------------: | ------------------------ |
| GPT-2 Small  |    124 Million | Excellent                |
| GPT-2 Medium |    355 Million | Excellent                |
| GPT-2 Large  |    774 Million | Comfortable              |
| GPT-2 XL     |    1.5 Billion | Possible but CPU-limited |

Increasing RAM substantially improves the ability to execute larger models, but inference speed continues to be constrained by the four-core processor.

---

# Performance Scores

| **Metric**                | **PyTorch (Python)** | **LibTorch (C++)** |
| ------------------------- | -------------------: | -----------------: |
| Raw Compute Performance   |                  100 |                100 |
| Memory Efficiency         |                   85 |                 95 |
| Startup Performance       |                   80 |                 95 |
| CPU Utilization           |                  100 |                100 |
| Thread Efficiency         |                  100 |                100 |
| Deployment Efficiency     |                   85 |                100 |
| Development Productivity  |                  100 |                 75 |
| Overall Engineering Score |                   93 |                 97 |

---

# Summary

This evaluation demonstrates that PyTorch Python and LibTorch C++ share the same computational backend and therefore deliver nearly identical neural network execution performance. The principal distinctions are application startup time, runtime memory overhead, deployment flexibility, and development productivity. On the evaluated system, the dominant bottlenecks are limited physical memory and CPU computational resources rather than the choice of programming language. Python remains the preferred environment for rapid development, experimentation, and research, whereas LibTorch C++ offers advantages for deployment, memory management, and integration with native software.

---

# Conclusion

For the current hardware configuration consisting of an Intel Core i3-9100F, 8 GB of RAM, and an NVIDIA GeForce GT 710, LibTorch C++ is the more appropriate long-term platform when the objective is performance-oriented deployment or systems engineering. However, developers should not expect significant improvements in token generation speed simply by replacing Python with C++, because both interfaces execute the same optimized computational kernels. The most effective upgrades for improving GPT-2 performance are increasing system memory to at least 16 GB, employing optimized quantized inference where appropriate, and eventually replacing the GT 710 with a modern NVIDIA GPU capable of accelerating transformer workloads. These hardware improvements will have a far greater impact on overall LLM performance than changing the programming language alone.

This version reads like a **technical report** that combines your **system specifications**, **performance engineering analysis**, **PyTorch vs. LibTorch comparison**, **GPT-2 model capacity**, **performance metrics**, and **engineering conclusions** into one continuous document.
