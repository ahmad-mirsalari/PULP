
# **Table of Contents**
1. [RISC-V](#risc_v)
   
3. [PULP](#pulp)
   
   2.1 [RISC-V Cores](#riscv_cores)
   
   2.2 [Peripherals](#Peripherals)
   
   2.3. [Interconnects](#Interconnects)

   2.4. [Platforms](#Platforms)

      2.4.1. [Single core](#Single)

      2.4.2. [Multi-core (Cluster-based)](#Multi-core)

      2.4.3. [Multi-Cluster](#Multi-Cluster)

   2.5. [Accelerator (HWPEs)](#Accelerator)

   2.6. [Silicon Proven designs](#Silicon_Proven_designs)

   2.7. [Software](#Software)

   2.8. [Useful libraries](#libraries)
      
      2.8.1. [Deploying DNNs on PULP](#Deploying_DNNs_on_PULP)
      
      2.8.2. [Digital Signal Processing](#DSP)

      2.8.3. [PULP-DroNet](#Dronet)

   2.9. [How we can develop an application on PULP](#application_pulp)
      
      2.9.1. [PULP SDK](#pulp_sdk)
      
      2.9.2. [PULP FreeRTOS](#pulp_freertos)
      
      2.9.3. [PULP Runtime](#pulp_runtime)
      
      2.9.4. [Snitch Runtime](#snitch_runtime)
      
      2.9.5. [CVA6](#cva6)

   2.9.10. [Some Case Studies](#case_studies)
      
      2.10.1. [GAP8](#gap8)
      
      2.10.2. [GAP9](#gap9)

This report will look at the PULP platform, designed to enable energy-efficient computing systems based on the RISC-V instruction set architecture. The report will cover various topics, including the RISC-V cores that can be used with PULP and the types of peripherals and interconnect available. The report will also explore the different platforms that PULP supports, including single-core and multi-core systems, and examine how accelerator modules can optimize performance. Additionally, the report will cover the software tools and libraries available for PULP, including those designed for deploying deep neural networks and for digital signal processing. The report will also provide a detailed overview of the PULP software development kit and the various runtime environments available for PULP-based systems. Finally, the report will include several case studies showcasing the real-world applications of PULP, including an examination of the GAP8 and GAP9 systems.

1. <a name="risc_v"></a>**RISC-V**

RISC-V is an open-source instruction set architecture (ISA) designed to be simple, modular, and extensible. It is based on the Reduced Instruction Set Computer (RISC) philosophy, which emphasizes using a small set of simple instructions that can be executed quickly and efficiently.

The RISC-V ISA is designed to be highly flexible and customizable, allowing for a wide range of implementations to meet specific application requirements. It supports various data types and memory models and can be extended to include custom instructions or additional features as needed.

One of the critical advantages of RISC-V is its open-source nature, which allows anyone to use, modify, or distribute the ISA without any licensing fees or restrictions. It is possible to customize the implementations using the free opcodes in RISC-V. This makes it a highly accessible platform for academic research, prototyping, and low-volume production.


<p align="center">
  <a href="https://youtu.be/RklEOl3xAdQ?t=14">
    <img src="http://img.youtube.com/vi/RklEOl3xAdQ/0.jpg" alt="Video Thumbnail">
  </a>
</p>

In this video, Frank Gürkaynak presented a short overview of the basics of RISC-V.

Given the increasing popularity of the RISC-V architecture, it is not surprising that several open-source platforms have emerged to support it. One such platform is PULP, which provides developers with tools and resources to create energy-efficient computing systems based on the RISC-V architecture.



2. <a name="pulp"></a>**PULP**

[PULP (Parallel Ultra Low Power)](https://pulp-platform.org/) is an [open-source platform](https://github.com/pulp-platform/) for designing energy-efficient, parallel computing systems. It is based on the RISC-V instruction set architecture (ISA) and includes various hardware and software components to support various parallel processing applications.

In addition to the processor cores, the PULP platform includes a range of other hardware components, such as memory controllers, communication interfaces, and accelerators, designed to support efficient parallel processing. It also includes various software tools and libraries, including compilers, debuggers, and performance analysis tools, designed to facilitate development on the platform.

The PULP platform is designed to be highly flexible and customizable, allowing it to be adapted to a wide range of application domains, including machine learning, signal processing, and Internet of Things (IoT) applications. It is also designed to be energy-efficient, focusing on minimizing power consumption while maintaining high performance.

This tutorial covers the following subjects about PULP:

- RISC-V Cores
- Peripherals
- Interconnects
- Platforms
  - Single Core
  - Multi-core (Cluster-based)
    - GAP8
    - GAP9
  - Multi-Cluster
- Accelerators (HWPEs)


<p align="center">
  <img src="/src/pulp_overview.png" alt="Image Description" width="500">
</p>

<p align="center"><em>Figure 1: An overview of PULP</em></p>

- Silicon Proven Designs
- Software
- Useful libraries
   - PULP-NN
   - Dory
   - QuantLab
   - PULP-TraniLib
   - PULP-DSP
   - PULP-Dronet
- How can we develop an application on PULP

One of the critical components of the PULP platform is the range of available RISC-V cores, which provide the foundation for energy-efficient computing systems.

2.1. <a name="riscv_cores"></a>**RISC_V CORES**


As shown in [Table 1](#table1), The PULP platform includes several RISC-V processors, such as RI5CY, Ariane, Snitch, etc. Here is a comparison of different processors on the [PULP website](<https://pulp-platform.org/implementation.html>). Also, you can find more details by clicking on each processor’s name. 
<div align="center">

## <a name="table1"></a><span style="font-size: 16px;">Table 1: Processors available in PULP</span>

</div>

|**Processor**|**Bits/Stages**|**Description**|
| :-: | :-: | :-: |
|[CV32E40P<br>(RI5CY)](https://pulp-platform.org/docs/ri5cy_user_manual.pdf)|32bit /<br>4-stage|A 4-stage 32-bit core that implements RV32IMC, with an optional 32-bit FPU supporting the F extension and instruction set extensions for digital signal processing (DSP) operations, including hardware loops, SIMD extensions, bit manipulation and post-increment instructions.|
|[Ibex (Zero-riscy)](https://pulp-platform.org/docs/user_manual.pdf)|32bit /<br>2-stage|An area-optimized 2-stage 32-bit core for control applications implementing RV32-IMC.|
|Micro-riscy|32bit /<br>2-stage|A minimal area 2-stage 32-bit core with 16 registers and no hardware multiplier implementing RV32-EC.|
|[CVA6 (Ariane)](https://github.com/openhwgroup/cva6/tree/master/docs)|64bit /<br>6-stage|A 6-stage, single issue, in-order 64-bit CPU which fully implements I, M, C and D extensions as specified in Volume I: User-Level ISA V 2.1 as well as the draft privilege extension 1.10. It implements three privilege levels, M, S, and U, to fully support a Unix-like (Linux, BSD, etc.) operating system. It has a configurable size, separate TLBs, a hardware PTW and branch prediction (branch target buffer, branch history table and a return address stack). The primary design goal was to reduce critical path length to about 20 gate delays.|
|[Snitch](https://github.com/pulp-platform/snitch)|32bit /<br>1-stage|A single-stage, single-issue 32-bit RISC-V integer core tuned for high energy efficiency. It aims to maximise the compute/control ratio by making the FPU external to the core and the dominant part of the design and mitigating the effects of deep pipelines and dynamic scheduling.|

One of the most widely used RISC-V cores in the PULP platform is the RI5CY core, which is a 32-bit core that is optimized for energy efficiency and high performance. This core includes various extensions (Xpulp) to RISC-V for DSP applications.

- Post–incrementing load/store instructions. 
- Hardware Loops (lp.start, lp.end, lp.count)
- ALU instructions 
  - Bit manipulation (count, set, clear, leading bit detection)
  - Fused operations: (add/sub-shift)
  - Immediate branch instructions 
- Multiply Accumulate (32x32 bit and 16x16 bit)
- SIMD instructions (2x16 bit or 4x8 bit) with scalar replication option 
  - add, min/max, dot product, shuffle, pack (copy), vector comparison. 

Here you can find more details about DSP ISA Extensions for an Open-Source RISC V Implementation.

<p align="center">
  <a href="https://www.youtube.com/watch?v=nbCJOEyicSY&ab\_channel=RISC-VInternational">
    <img src="http://img.youtube.com/vi/nbCJOEyicSY/0.jpg" alt="Video Thumbnail">
  </a>
</p>

An overall summary of the cores is provided in [Figure 2](#cores).

<p align="center">
  <img name="cores" src="/src/cores.png" alt="Image Description" width="500">
</p>

<p align="center"><em>Figure 2: A summary of the available cores</em></p>


[Here](https://video.ethz.ch/events/2019/risc-v/8624b69b-83c4-495f-a748-e5b28f7cb55d.html) you can find more details about OpenPiton+Ariane which is the first Linux-Booting Open-Source RISC-V Manycore.

Also, Prof. Luca Benini talked about Ariane. 

<p align="center">
  <a href="https://youtu.be/RklEOl3xAdQ?t=3717">
    <img src="http://img.youtube.com/vi/RklEOl3xAdQ/0.jpg" alt="Video Thumbnail">
  </a>
</p>


In addition to the range of RISC-V cores available in PULP-based systems, various peripherals can be integrated into these systems to provide additional functionality and improve overall performance.

2.2. <a name="Peripherals"></a>**Peripherals**

The PULP team have developed customized accelerators, AXI-compatible interconnect solutions, DMA engines, and peripherals to communicate with the environment, including GPIO, SPI, I2S, JTAG, and many more. More details are available here:
<p align="center">
  <a href="https://youtu.be/w-24R_n2QFU?t=235">
    <img src="http://img.youtube.com/vi/w-24R_n2QFU/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.3. <a name="Interconnects"></a>**Interconnects**

The PULP project uses several types of interconnects to enable communication between the various processing elements on its system-on-chip (SoC) designs, such as Logarithmic interconnect, APB-Peripheral BUS, and AXI4-interconnect.

More information is available here.
<p align="center">
  <a href="https://youtu.be/Ipy71G8eSWY?t=1268">
    <img src="http://img.youtube.com/vi/Ipy71G8eSWY/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.4. <a name="Platforms"></a>**Platforms**

We can divide PULP platforms into three categories:

2.4.1  <a name="Single"></a>**Single core**

The simplest PULP-based systems are micro-controllers that can be configured to use any 32-bit RISC-V cores they have developed (RI5CY, Zero-riscy, Micro-riscy) to add memory and some peripherals, according to [Figure 3](#singlecore). Advanced versions also allow Accelerators to be added to the system.

<p align="center">
  <img name="singlecore" src="/src/singlecore.png" alt="Image Description" width="200">
</p>

<p align="center"><em>Figure 3: single core components</em></p>

M, R5, A, I, and O are Memory, RISC-V Core, Accelerator, Input, and Output. PULPissimo and PULPino are two Single core MCUs in the PULP project. Let’s compare these two MCUs due to the PULP website.

2.4.1.1  <a name="PULPino"></a>**PULPino**

A minimal single-core RISC-V SoC, the first open-source release that has attracted a lot of attention.

<p align="center">
  <img name="pulpino" src="/src/pulpino.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 4 PULPino architecture</em></p>


More details about PULPino are available here.

<p align="center">
  <a href="https://www.youtube.com/watch?v=Hkkb4IljTB8&ab\_channel=RISC-VInternational">
    <img src="http://img.youtube.com/vi/Hkkb4IljTB8/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.4.1.2  <a name="PULPissimo"></a>**PULPissimo**

An advanced version of their microcontroller. The main change is the presence of the logarithmic interconnect between the core and the memory subsystem allowing multiple access ports. These are then used by an integrated uDMA that can copy data directly between peripherals, memory, and optional accelerators called Hardware Processing Engines (HWPEs).

<p align="center">
  <img name="pulpissimo" src="/src/pulpissimo.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 5 PULPissimo architecture</em></p>

In this tutorial, Davide Schiavone explains the architecture of PULPissimo, the differences among individual PULP cores, Xpulp extensions, and much more.

<p align="center">
  <a href="https://youtu.be/27tndT6cBH0]">
    <img src="http://img.youtube.com/vi/27tndT6cBH0/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.4.2  <a name="Multi-core"></a>**Multi-core (Cluster-based)**


<p align="center">
  <img name="multicore" src="/src/multicore.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 6 cluster-based components</em></p>

The more advanced systems are based on clusters of 32-bit RISC-V cores with direct access to a small and fast scratchpad memory (Tightly Coupled Data Memory). The cluster is supported by an SoC that houses a larger second-level memory, peripherals for input and output, and a complete PULPissimo class microcontroller for power management and basic operations in later versions. 
<p align="center">
  <img name="multicore_arch" src="/src/multicore_arch.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 7 Multi-core architecture</em></p>

Most of their research is based on developing architectures based on these systems. [Mia Wallace](http://asic.ethz.ch/2015/Mia_Wallace.html), [Honey Bunny](http://asic.ethz.ch/2015/Honey_Bunny.html), [Fulmine](http://asic.ethz.ch/2015/Fulmine.html), [Mr. Wolf](http://asic.ethz.ch/2017/Mr.Wolf.html) and [Vega](http://asic.ethz.ch/2020/Vega.html) are all such systems, and the source code for the latest system has been released as OPENPULP on their [GitHub](https://github.com/pulp-platform/pulp) page.

During the ACACES20 summer school, Prof. Luca Benini talked about this multi-core platform and other concepts related to the cluster, such as barrier, DMA, Memory, and interconnects.
<p align="center">
  <a href="https://youtu.be/Ipy71G8eSWY?t=979">
    <img src="http://img.youtube.com/vi/Ipy71G8eSWY/0.jpg" alt="Video Thumbnail">
  </a>
</p>

Two prominent examples of PULP-based commercially available platforms are GAP8 and GAP9, which offer a range of features and benefits for specific design requirements and performance goals.


2.4.2.1 <a name="GAP8"></a>**GAP8**

GAP8 (Greenwaves Application Processor) is a low-power, high-performance application processor developed by Greenwaves Technologies. It is designed specifically for the efficient execution of machine learning and signal-processing algorithms in embedded systems. (More details are available [here](https://greenwaves-technologies.com/wp-content/uploads/2021/04/Product-Brief-GAP8-V1\_9.pdf))

GAP8 is based on the RISC-V open-source instruction set architecture and is optimized for low power consumption and high performance. It features a multi-core design with eight processing cores, each with its own local memory and shared L2 cache and includes hardware accelerators for commonly used signal processing and machine learning operations.

The processor is designed for many embedded systems, including sensor nodes, wearables, and other low-power IoT devices. Its low power consumption and high performance make it well-suited for applications such as image and audio processing, gesture recognition, and environmental monitoring.

Greenwaves Technologies also provides a comprehensive software development kit (SDK) for GAP8, including optimized machine learning and signal processing libraries, a C/C++ compiler, and tools for debugging and profiling applications. The SDK also includes support for the PULP operating system, allowing developers to leverage the full capabilities of the PULP ecosystem.

<p align="center">
  <img name="gap8" src="/src/gap8.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 8 GAP8</em></p>


2.4.2.2 <a name="GAP9"></a>**GAP9**

[GAP9](https://greenwaves-technologies.com/wp-content/uploads/2023/02/GAP9-Product-Brief-V1_14_non_NDA.pdf) is the latest version of the Greenwaves Application Processor (GAP) developed by Greenwaves Technologies. Like its predecessor, GAP8, GAP9 is a low-power, high-performance application processor optimized for the efficient execution of machine learning and signal processing algorithms in embedded systems.

GAP9 features a multi-core design with nine RISC-V cores, including an additional processing core compared to GAP8. Each core has its own local memory, shared L2 cache, and hardware accelerators for signal processing and machine learning operations. GAP9 also features new hardware modules for image processing, including a multi-channel, multi-resolution image sensor interface and hardware support for convolutional neural networks (CNNs).

<p align="center">
  <img name="gap9" src="/src/gap9.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 9 GAP9</em></p>

2.4.3 <a name="Multi-Cluster"></a>**Multi-Cluster**

<p align="center">
  <img name="figure10" src="/src/multi_cluster.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 10 Multi-Cluster components</em></p>

They have also expanded their work for larger workloads, where a PULP system that contains multiple clusters is connected to a regular computing node. In this scenario, the PULP cluster is used as an energy-efficient accelerator for DSP loads. Their [HERO platform](https://pulp-platform.org/hero.html) release is such a system.

<p align="center">
  <img name="multi_cluster_ach" src="/src/multi_cluster_ach.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 11 Multi-Cluster architecture</em></p>
More materials on different types of multi-cluster architectures are available on

- [Snitch core & FREP](<https://ieeexplore.ieee.org/abstract/document/9216552>)
- [HERO](<https://arxiv.org/abs/2201.03861>)
- [Occamy](github.com/pulp-platform/snitch)

Prof. Luca Benini shared insights gained in designing open-source RISC-V hardware and software for energy-efficient computing, moving from tiny, parallel ultra-low power chips to high-performance many-core chipsets.
<p align="center">
  <a href="https://www.youtube.com/watch?v=kMhdq7A3d3I&ab_channel=SCConferenceSeries">
    <img src="http://img.youtube.com/vi/kMhdq7A3d3I/0.jpg" alt="Video Thumbnail">
  </a>
</p>

For more information, you can watch this video  where Frank Gürkaynak provided more details about PULP-based chips.
<p align="center">
  <a href="https://youtu.be/Ipy71G8eSWY?t=5236">
    <img src="http://img.youtube.com/vi/Ipy71G8eSWY/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.5. <a name="Accelerator"></a>**Accelerator (HWPEs)**

*Hardware Processing Engines* (HWPEs) are special-purpose, memory-coupled accelerators that can be inserted in the SoC or cluster of a PULP system to amplify its performance and energy efficiency in particular tasks.

Unlike most accelerators in literature, HWPEs do not rely on an external DMA to feed them with input and extract output, and they are not (necessarily) tied to a single core. Instead, they operate directly on the same memory shared by other PULP system elements (e.g., the L1 TCDM in a PULP cluster or the shared L2 in PULPissimo). Their control is memory-mapped and accessed through a peripheral bus or interconnect. HW-based execution on an HWPE can be readily intermixed with software code because all that needs to be exchanged between the two is a set of pointers and, if necessary, a few parameters. (More details are available [here](<https://hwpe-doc.readthedocs.io/en/latest/modules.html>))

The following hardware accelerators are available (find the latest papers and accelerators on their [website](<https://hwpe-doc.readthedocs.io/en/latest/papers.html> )):

- HWCE: Convolution engine
- XNE: Binary Neural Network Inference
- [RBE: Convolutions, flexible precision for weights and activations](<https://github.com/pulp-platform/rbe>)
- [NE16: Convolutions, flexible precision for weights](<https://github.com/pulp-platform/ne16>)
- FFT Accelerator
- [RedMulE: floating-point GEMM accelerator](<https://github.com/pulp-platform/redmule>)
- IMA: In-Memory Computing
- SNE: Digital SNN Accelerator for Sparse Event-Based Convolutions (uses a modified HWPE infrastructure)

Prof. Benini provided an overview of what is written on PULP concepts in the previous sections
<p align="center">
  <a href="https://youtu.be/Ipy71G8eSWY">
    <img src="http://img.youtube.com/vi/Ipy71G8eSWY/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.6.  <a name="Silicon_Proven_designs"></a>**Silicon Proven designs**


They have a long tradition of taping out ASICs at ETH Zurich; check their [Chip Gallery](http://asic.ethz.ch/). They have designed and tested over 40 PULP-related designs in several technologies (more details are available [here](<https://pulp-platform.org/implementation.html>)). 

<p align="center">
  <img name="chips" src="/src/chips.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 12 chips</em></p>


2.7.  <a name="Software"></a>**Software**

<p align="center">
  <img name="pulp_software" src="/src/pulp_software.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 13 PULP software</em></p>

The PULP Microcontroller Software Interface Standard (PMSIS) provides the Board Support Package (BSP), the Application Programming Interface (API), and the drivers for running applications on PULP-based Microcontrollers (MCUs). It is developed and expanded based on the old pulp-rt used, e.g., for Mr. Wolf processor.

The GCC and the LLVM compilers used for PULP are respectively based on GNU GCC and LLVM supporting the PULP ISA based on RISC-V standard ISA and specific extensions such as Xpulpv0, Xpulpv1, Xpulpv2 and XpulpNN which have different features and application domains.

PULPOS is an optimized software library for operating system functionalities such as tasking, memory management and interrupts. Alternatively, FreeRTOS is also ported for PULP, including drivers. The Hardware Abstraction Layer (HAL) is a set of functions that hides the register level of the memory map, allowing common programming entry points for typical hardware modules.

2.8. <a name="libraries"></a>**Useful libraries**

   2.8.1.  <a name="Deploying_DNNs_on_PULP"></a>**Deploying DNNs on PULP**
   
   2.8.1.1.  **PULP NN**
      
[PULP NN](https://github.com/pulp-platform/pulp-nn) is a multicore computing library for Quantized Neural Network (QNN) inference on PULP clusters of RISC-V-based processors. It includes optimized kernels such as convolution, matrix multiplication, pooling, normalization and other common state-of-the-art QNN kernels. It fully exploits the Xpulp ISA extension and the cluster's parallelism to achieve high performance and energy efficiency on PULP-based devices. It has been tested on GWT GAP8.

- Work on L1 memory; data exchange with outer memory levels is managed at a higher level
- Exploit parallelism + vectorization capabilities of PULP RI5CY/CV32E40P cores
- Try to transform all linear operators into a GEMM (Generalized Matrix Multiplication) form

GEMM-based convolution is a technique for implementing convolutional neural networks (CNNs) using matrix multiplication operations. It is based on the observation that the computation performed by the convolution operation can be expressed as a matrix multiplication between the input data and a set of learnable weights, followed by a bias term and an activation function. "GEMM" stands for "general matrix multiplication", a fundamental operation in linear algebra. Expressing the convolution operation as a matrix multiplication can be efficiently implemented using hardware or software optimized for GEMM operations.

- Target Height/Width/Channel (HWC) data layout
- [Open-source code](https://github.com/pulp-platform/pulp-nn)

More details are available [here](https://video.ethz.ch/events/2019/risc-v/3e8fdd84-b4b5-493d-9b72-65f25b0852d6.html)

2.8.1.2. **DORY**

[DORY](https://github.com/pulp-platform/dory) (Deployment Oriented to memoRY) is an automatic tool to deploy DNNs on PULP platforms. DORY abstracts DNN tiling problem as a Constraint Programming (CP) problem: it maximizes the L1 memory utilization under the topological constraints imposed by each DNN layer. Then, it generates ANSI C code to orchestrate off- and on-chip transfers and computation phases. Furthermore, DORY augments the CP formulation with heuristics promoting performance-effective tile sizes based on the PULP-NN or other custom DNN backends to maximise speed. For more details, visit [here](https://github.com/pulp-platform/dory). Also, Alessio is talking about the PULP Virtual Platform and DORY, an automated tool to deploy DNNs on memory-constrained devices.

<p align="center">
  <a href="https://youtu.be/kTVUJyRyibU?t=3747">
    <img src="http://img.youtube.com/vi/kTVUJyRyibU/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.8.1.2. **QuantLab**

[QuantLab](https://github.com/pulp-platform/quantlab) (Deployment Oriented to memoRY) is a tool to train, compare and deploy quantized neural networks (QNNs). It was developed on top of the [PyTorch](https://pytorch.org/) deep learning framework and is a purely command-line-based tool. More details can be found [here](https://github.com/pulp-platform/quantlab).

You can find the QuantLab video here.
<p align="center">
  <a href="https://youtu.be/dgngk02eLYI">
    <img src="http://img.youtube.com/vi/dgngk02eLYI/0.jpg" alt="Video Thumbnail">
  </a>
</p>
In this tutorial, Matteo presents QuantLab, a PyTorch-based software tool designed to train quantized neural networks, optimize them, and prepare them for deployment on PULP platforms.

In this talk, Dr. Francesco Conti discusses An Open Source Flow for DNNs on Ultra Low Power RISC V Cores.
<p align="center">
  <a href="https://www.youtube.com/watch?v=5-lLRE99lBo&ab_channel=RISC-VInternational">
    <img src="http://img.youtube.com/vi/5-lLRE99lBo/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.8.1.4.  **[PULP-TrainLib](<https://github.com/pulp-platform/pulp-trainlib> )**

[PULP-TrainLib](<https://github.com/pulp-platform/pulp-trainlib>) is the first Deep Neural Network training library for the PULP Platform. PULP-TrainLib features a wide set of performance-tunable DNN layer primitives for training, together with optimizers, losses, and activation functions. To enable on-device training, PULP-TrainLib is equipped with AutoTuner, a pre-deployment tool to select the fastest configuration for each DNN layer, according to the training step to be performed and the shapes of the layer tensors. To facilitate the deployment of training tasks on the target PULP device, PULP-TrainLib is equipped with the TrainLib Deployer, a code generator capable of generating a project folder containing all the files and the code to run a DNN training task on PULP.


2.8.2. <a name="DSP"></a>**Digital Signal Processing**

   2.8.2.1.  **PULP DSP**
   
[PULP DSP](https://github.com/pulp-platform/pulp-dsp) provides optimized functions for digital signal processing, such as dot product, matrix multiplication, convolution, fast Fourier transform, etc., for various data types (8-, 16-, 32-bit integer and fixed-point, and single-precision floating-point). The optimized implementations exploit the SIMD instructions, hardware loop, parallel cluster, etc. It has been tested on Mr. Wolf, featuring Ibex and CV32E40P cores and pulp-open. It can also be run on GWT GAP8 featuring CV32E40P cores. For more details, please visit the [repository](https://github.com/pulp-platform/pulp-dsp) and refer to the [documentation](https://pulp-platform.github.io/pulp-dsp/), where you can also find a [documentation](https://pulp-platform.github.io/pulp-dsp/tutorial-index/) on how to use the library and advice on how to optimize codes on PULP.

2.8.3. <a name="Dronet"></a>**[PULP-DroNet](<https://github.com/pulp-platform/pulp-dronet/tree/master/pulp-dronet-v2>)**

**PULP-DroNet** is a deep learning-powered *visual navigation engine* that enables autonomous navigation of a pocket-size quadrotor in a previously unseen environment. Thanks to PULP-DroNet, the nano-drone can explore the environment, avoiding collisions with dynamic obstacles, in complete autonomy -- **no human operator, no ad-hoc external signals, and no remote laptop!** This means all complex computations are quickly done directly aboard the vehicle. The visual navigation engine is composed of both software and hardware part.

More details are available in these videos:
<p align="center">
  <a href="https://www.youtube.com/watch?v=57Vy5cSvnaA&ab\_channel=PULPPlatform">
    <img src="http://img.youtube.com/vi/57Vy5cSvnaA/0.jpg" alt="Video Thumbnail">
  </a>
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=JKY03NV3C2s&ab\_channel=PULPPlatform">
    <img src="http://img.youtube.com/vi/JKY03NV3C2s/0.jpg" alt="Video Thumbnail">
  </a>
</p>


<p align="center">
  <a href="https://www.youtube.com/watch?v=Cd9GyTl6tHI&ab\_channel=PULPPlatform">
    <img src="http://img.youtube.com/vi/Cd9GyTl6tHI/0.jpg" alt="Video Thumbnail">
  </a>
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=41IwjAXmFQ0&ab\_channel=PULPPlatform">
    <img src="http://img.youtube.com/vi/41IwjAXmFQ0/0.jpg" alt="Video Thumbnail">
  </a>
</p>

2.9.  <a name="application_pulp"></a>**How we can develop an application on PULP**

Let’s now take a user's point of view. If you’d like to develop an application using machine learning or digital signal processing algorithms, you can start with PULP SDK if you intend to use mostly integer operations or with Snitch Runtime for optimized floating-point operations. If you wish to use Linux, CVA6 will be your choice. If you intend to develop Hardware (HW), e.g., an HW accelerator and would like to test some simple software code quickly, then you can go with PULP Runtime. Finally, if you prefer FreeRTOS, you can go with the PULP FreeRTOS.

<p align="center">
  <img name="figure14" src="/src/pulp_usage.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 14 an overall flow of PULP usage</em></p>

Nazareno Bruschi introduces the Software Development Kit for PULP, while Giuseppe elaborates on the GCC Compilation Toolchain.


<p align="center">
  <a href="https://youtu.be/Ydd9TlKQiO4">
    <img src="http://img.youtube.com/vi/Ydd9TlKQiO4/0.jpg" alt="Video Thumbnail">
  </a>
</p>


2.9.1. <a name="pulp_sdk"></a>**PULP SDK**

[PULP SDK](https://github.com/pulp-platform/pulp-sdk) includes the fundamental libraries, tools, and scripts to develop applications for PULP chips, such as platform descriptions, operating system libraries, drivers, and simulators.

It includes the **GVSoC** virtual platform, which guarantees high accuracy, including all PULP hardware IP models, such as cores, cluster, interconnect, cache, and uDMA. It is an event-based simulator (cycle-accurate at a core level, with statistical approximations at interconnect level). It results in fast simulations and allows an agile reconfiguration thanks to JSON-based platform description files and Python generators.

Nazareno Bruschi is talking about GVSoC here.
<p align="center">
  <a href="https://youtu.be/kTVUJyRyibU">
    <img src="http://img.youtube.com/vi/kTVUJyRyibU/0.jpg" alt="Video Thumbnail">
  </a>
</p>

The virtual platform allows dumping architecture events to help developers debug their applications by better showing what is happening in the system. For example, it can show executed instructions, DMA transfers, events generated, memory accesses, etc. The generated traces can be visualized using [GTKWave](http://gtkwave.sourceforge.net/).

2.9.2. <a name="pulp_freertos"></a>**PULP FreeRTOS**

[PULP FreeRTOS](https://github.com/pulp-platform/pulp-freertos) provides FreeRTOS and drivers for developing real-time applications on PULP-based systems. Programs can be run using RTL simulation (simulating the hardware design), e.g., QuestaSim, or the GVSoC virtual platform (software emulation of the hardware design). A book about FreeRTOS can be found [here](https://www.freertos.org/Documentation/RTOS_book.html), and the official documentation is available on [this website](https://www.freertos.org/features.html). It has been tested on Pulpissimo, pulp-open, and ControlPULP.

2.9.3.  <a name="pulp_runtime"></a>**PULP Runtime**

[PULP Runtime](https://github.com/pulp-platform/pulp-runtime) provides a minimal way to run a barebone program on PULP architectures. Programs can be run using RTL simulation, e.g., QuestaSim. You can use it, e.g., when you develop a new piece of HW, such as HW accelerators. It has been tested on Pulpissimo, pulp-open, ControlPULP, and Marsellus.

2.9.4. <a name="snitch_runtime"></a>**Snitch Runtime**

Snitch Runtime provides a fundamental, bare-metal runtime for [Snitch](https://github.com/pulp-platform/snitch) systems. It exposes a minimal API to manage the execution of code across the available cores and clusters, query information about a thread's context, and coordinate and exchange data with other threads.

It includes an LLVM-based binary translation simulator for Snitch systems called Banshee that can specifically emulate the custom instruction set extensions (instruction-accurate).

For Snitch, the [Trace-viewer](https://github.com/pulp-platform/snitch/pull/236) or [Catapult](https://github.com/catapult-project/catapult/tree/master/tracing) is used to visualize traces.

The DSP, NN, DORY, and QuantLab workflow support are under development.

2.9.5.  <a name="cva6"></a>**CVA6**

CVA6 SDK is used for [CVA6](https://github.com/pulp-platform/cva6), which is a 6-stage, single-issue, in-order CPU which implements the 64-bit RISC-V instruction set. You can simulate CVA6 in QuestaSim, VCS, and Verilator (the Verilator output can be visualised with GTKWave) and emulate CVA6 on FPGAs.

2.10.  <a name="case_studies"></a>**Some Case Studies**

   2.10.1.  <a name="gap8"></a>**GAP8**
   
   - [Low-Power License Plate Detection and Recognition on a RISC-V Multi-Core MCU-based Vision System](https://github.com/GreenWaves-Technologies/licence_plate_recognition)

The project implements an image-based Deep Learning pipeline to detect license plates and read the registration number. The algorithm is based on a 2 steps inference model:

- Mobilenet SSD-Lite to detect License plates within greyscale 320x240 images.
- LPRNet to read the registration number from 94x24 License Plates crops.

Models are invoked in sequence to run the full pipeline.

<p align="center">
  <img name="figure15" src="/src/plate_detection.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 15 License Plate Detection</em></p>

More details are available in this talk.
<p align="center">
  <a href="https://www.youtube.com/watch?v=0nS7ZynRoIs&ab_channel=tinyML">
    <img src="http://img.youtube.com/vi/0nS7ZynRoIs/0.jpg" alt="Video Thumbnail">
  </a>
</p>


- [enabling perception on Nano-Robots](https://arxiv.org/abs/2301.12175)
   - [Paper](https://arxiv.org/abs/2301.12175)
   - [GitHub](<https://github.com/pulp-platform/pulp-dronet/tree/master/pulp-dronet-v2>)

<p align="center">
  <img name="nano_robots" src="/src/nano_robots.png" alt="Image Description" width="500">
</p>
<p align="center"><em>Figure 16 Nano-Robots</em></p>

2.10.2. <a name="gap9"></a>**GAP9**

- [Mixed-Precision Speech Enhancement on multi-core MCUs](<https://arxiv.org/abs/2210.07692>)

This paper presents an optimized methodology to design and deploy Speech Enhancement (SE) algorithms based on Recurrent Neural Networks (RNNs) on a state-of-the-art MicroController Unit (MCU) with 1+8 general-purpose RISC-V cores. To achieve low-latency execution, we propose an optimized software pipeline interleaving parallel computation of LSTM or GRU recurrent blocks, featuring vectorized 8-bit integer (INT8) and 16-bit floating-point (FP16) compute units, with manually-managed memory transfers of model parameters. To ensure minimal accuracy degradation with respect to the full-precision models, we propose a novel FP16-INT8 Mixed-Precision Post-Training Quantization (PTQ) scheme that compresses the recurrent layers to 8-bit while the bit precision of remaining layers is kept to FP16. Experiments are conducted on multiple LSTM and GRU-based SE models trained on the Valentini dataset, featuring up to 1.24M parameters. Thanks to the proposed approaches, we speed up the computation by up to 4x with respect to the lossless FP16 baselines. Differently from a uniform 8-bit quantization that degrades the PESQ score by 0.3 on average, the Mixed-Precision PTQ scheme leads to a low-degradation of only 0.06 while achieving a 1.4-1.7x memory saving. Thanks to this compression, we cut the power cost of the external memory by fitting the large models on the limited on-chip non-volatile memory, and we gain an MCU power saving of up to 2.5x by reducing the supply voltage from 0.8V to 0.65V while still matching the real-time constraints. Our design results are 10x more energy efficient than state-of-the-art SE solutions deployed on single-core MCUs that use smaller models and quantization-aware training.

- Continual On-device Learning on Multi-Core RISC-V MicroControllers

  More details are available here.

<p align="center">
  <a href="https://www.youtube.com/watch?v=FiP48Za9ElM&ab_channel=tinyML">
    <img src="http://img.youtube.com/vi/FiP48Za9ElM/0.jpg" alt="Video Thumbnail">
  </a>
</p>


**Transprecision Floating Point Unit on PULP**

[An Open-Source Transprecision FPU on PULP](<https://github.com/openhwgroup/cvfpu> ):

<p align="center">
  <a href="https://www.youtube.com/watch?v=X_UqAS33ev8&ab_channel=RISC-VInternational">
    <img src="http://img.youtube.com/vi/X_UqAS33ev8/0.jpg" alt="Video Thumbnail">
  </a>
</p>
