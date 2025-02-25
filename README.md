[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Kaihao CHEN
### Student Id: 21072068
### Email: kchenbw@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Tools selected:
    >
    > computing performance: Phoronix Test Suite
    >
    > - <u>phoronix-test-suite run pts/compress-7zip</u>: Use the 7-Zip compression algorithm to test the CPU multi-threaded compression performance, parameters include compression level (default 5), test file size (default 1GB)
    >   - The higher the value, the stronger the CPU's multithreading performance.
    > - <u>phoronix-test-suite run pts/ramspeed</u> 5 1: Test memory bandwidth, default parameters and <u>select operation type (Average test(copy, scale, add, triad) and integer)</u>
    >   - Reflect memory read and write efficiency
    >
    > Networking performance: iPerf & PING
    >
    > - iperf -c <IP> -w 256k: the TCP window size is set to 256k to reduce the impact of high latency
    > - iperf -s -w 256k: the TCP window size is set to 256k
    > - ping <IP> -c 100: Send 100 ICMP packets to calculate the average RTT

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    CPU performance: average compression rating / average decompression rating
    
    | Size        | CPU performance MIPS | Memory performance MB/s |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 3615 / 3075 | 10416.90 |
    | `t2.medium`  | 10200 / 6015 | 20014.98 |
    | `c5d.large` | 7338 / 4872 | 13740.35 |
    
    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Gbits) | RTT (ms) |
    | ------------------------- | --------------- | -------- |
    | `t3.medium` - `t3.medium` | 3.85            | 0.206    |
    | `m5.large` - `m5.large`   | 4.96            | 0.174    |
    | `c5n.large` - `c5n.large` | 4.94            | 0.177    |
    | `t3.medium` - `c5n.large` | 2.23            | 0.603    |
    | `m5.large` - `c5n.large`  | 2.84            | 0.535    |
    | `m5.large` - `t3.medium`  | 1.96            | 0.842    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use **private** IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w        | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 33.0 Mbits/sec | 60.0     |
    | N. Virginia - N. Virginia | 2.00 Gbits/sec | 0.895    |
    | Oregon - Oregon           | 4.63 Gbits/sec | 0.200    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use **public** IP address when using iPerf within the same region.
