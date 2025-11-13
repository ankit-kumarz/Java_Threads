# Java Threads — Virtual Threads vs ForkJoin (Research)
                                                                                                                                                   
Author : Ankit Kumar  
Email: ankitrajj1068@gmail.com                                                                                                                                                                                      
Under the Supervision: Mrs Dimpy Singh (Assistant Professor), Department of CSE, JECRC University

## Abstract

This repository accompanies a research paper comparing Java virtual threads (Project Loom) and traditional ForkJoin-based concurrency models. The experiments implement both CPU-bound and I/O-bound workloads using two approaches:

- ForkJoin (work-stealing pool / structured parallelism)
- Virtual threads (lightweight threads from Project Loom)

The goal is to compare throughput, latency, scalability and resource utilization between these two approaches on representative CPU-bound and I/O-bound workloads.

## Repository structure

Files in the root directory:

- `CpuBoundForkJoin.java` — CPU-bound workload implemented using ForkJoin / ForkJoinPool or structured parallelism.
- `CpuBoundVirtualThreads.java` — CPU-bound workload implemented using virtual threads (JDK 21+).
- `IoBoundForkJoin.java` — I/O-bound workload implemented using ForkJoin or thread pool.
- `IoBoundVirtualThreads.java` — I/O-bound workload implemented using virtual threads.
- `LICENSE` — MIT license.
- `README.md` — this file.

Each Java file contains a self-contained `main` method. The programs are intended to be small, reproducible benchmarks to demonstrate differences between the two concurrency approaches.

## Requirements

- JDK 21 or later (Project Loom virtual threads are stable starting JDK 21). If you use a different JDK or vendor, ensure virtual threads are available.
- Windows PowerShell (examples below use PowerShell syntax).

## How to compile

Open a PowerShell terminal in the repository folder and run:

```powershell
javac *.java
```

This compiles all four example programs into `.class` files.

## How to run

Run any program with `java` from PowerShell. Example:

```powershell
# Run the CPU-bound version using ForkJoin
java CpuBoundForkJoin

# Run the CPU-bound version using virtual threads
java CpuBoundVirtualThreads

# Run the I/O-bound version using ForkJoin
java IoBoundForkJoin

# Run the I/O-bound version using virtual threads
java IoBoundVirtualThreads
```

If the programs accept command-line arguments (thread counts, task sizes, iteration counts), pass them after the class name. Check the top of each `.java` file for any usage notes or default parameters.

### Measuring time in PowerShell

Use `Measure-Command` to get wall-clock time for a run. Example:

```powershell
Measure-Command { java CpuBoundVirtualThreads }
```

For repeated runs and more rigorous benchmarking, run each experiment multiple times and discard the first (warmup) run. Capture CPU and memory usage using your OS tools (Task Manager, Performance Monitor) or add JVM flags like `-XshowSettings:vm` and `-Xmx` to control memory.

## Recommended JVM flags for reproducible results

- Use a fixed heap size when comparing runs: `-Xms2G -Xmx2G` (adjust to your machine).  
- Disable background services or other noisy processes while benchmarking.  
- For more detailed JVM-level metrics consider using `java -XX:+UnlockDiagnosticVMOptions -XX:+LogCompilation` or JFR recordings (Java Flight Recorder).

Example run with fixed heap:

```powershell
java -Xms2G -Xmx2G CpuBoundVirtualThreads
```

## Reproducing the experiments (suggested protocol)

1. Compile with `javac *.java`.
2. Run each test variant (ForkJoin vs VirtualThreads) N times (N >= 5).  
3. For each run record: wall-clock time, CPU utilization, peak memory.  
4. Compute mean and standard deviation, and plot results (throughput vs concurrency level, latency percentiles for IO-bound tests).

Notes: for CPU-bound tests scale the number of worker tasks from 1 up to 2× number of physical cores to observe saturation. For I/O-bound tests use large numbers of concurrent tasks (hundreds or thousands) to show where virtual threads reduce blocking overhead.

## Expected observations (discussion points)

- Virtual threads typically reduce complexity of code that blocks on I/O and scale to large numbers of concurrent tasks with low memory footprint.  
- For CPU-bound workloads, structured parallelism (ForkJoin) that matches available cores often delivers the best CPU utilization; oversubscribing with many threads may hurt performance.  
- The exact crossover point depends on hardware, workload characteristics and JVM settings — reproducibility is key.

## Citation

If you use this repository in your research, please cite:  
Ankit Kumar, Department of Computer Science & Engineering, JECRC University, Jaipur, India. Contact: ankit.22bcon1068@jecrcu.edu.in

## License

See the `LICENSE` file in this repository for license terms.

## Contact

For questions about the code or experiments, email Ankit Kumar at ankit.22bcon1068@jecrcu.edu.in.

---

