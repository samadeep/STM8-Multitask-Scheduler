**STM8-Multi-Tasker: A Cooperative/Preemptive Round Robin Scheduler**

This project provides a lightweight scheduler for the STM8 microprocessor, enabling threading with stack-based context switching. 
It's implemented in mixed C and assembler using SDCC, and leverages CMake for build management, ideally within the JetBrains CLion IDE (setup instructions to be added).

The core is optimized in assembler to minimize footprint, addressing SDCC's less-than-ideal code generation for STM8. This allows for more efficient use of C in specific projects, reducing concerns about code size.

**Resource Footprint**

**Minimal cooperative multi-tasking**: ~500 bytes code, 8 bytes data, plus 6 bytes + task stack size per task.
With tick timer (timed suspensions): ~700 bytes code, 16 bytes data, plus 8 bytes + task stack size per task.
All features enabled: ~1k code, 34 bytes data, plus 8 bytes + task stack size per task.

**Multi-Threading Considerations**
Non-reentrant functions called with interrupts enabled require protection to prevent concurrent access from different tasks. This can be achieved through:

**Mutexes**: Guard shared functions to ensure exclusive access.

**Global State Preservation (if applicable)**: If the function/library's storage location is known and stack space allows, use the OPT_PRESERVE_GLOBAL_STATE option along with custom save/restore functions. Use sparingly due to the per-task stack overhead and execution time on each switch.

**Hybrid Approach**: Combine the above methods as needed.

**Cooperative Scheduling Only**: Avoid shared, unlocked resources from ISRs.

**Library Limitations**
Assembly routines assume SDCC's default caller-saved register convention.
Interrupts are disabled during library calls, potentially increasing interrupt latency.
Despite these limitations, the benefits of a context switcher for simplified logic and implementation often outweigh the overhead, even on resource-constrained systems.
