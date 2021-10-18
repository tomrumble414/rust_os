# rust_os
An operating system written in Rust to develop Rust and OS skills
## Installation and features

## design decisions

## Key learning points

### Stack unwinding
  In essence, stack unwinding is just removing entries from the call stack.
  When no exceptions are thrown this process is rather straightforward, with each function termination, execution continues at the stored address where the function was called and the memory the called function was using is freed.
  
  The important implementation of stack unwinding is during an exception where the memory on the stack must be freed. Thus all the destructors of local variables previous to the exception throw need to be called and this is considered 'stack unwinding'

### 'The red zone' (128-byte red zone)
The idea behind the redzone is that it is an optimisation of stack allocation, where it allows functions to temporarily use the 128 bytes below its stack frame without adjusting the stack pointer. The benefits of this is that it allows for allocation additional stack memory without having to move the stack pointer and thus saves an instruction.

  However for the Rust kernel this red zone optimisation needs to be disabled as it causes issues with exceptions and hardware interupts. Since if one of those was to occur when a function currently has data in the red zone, that data would be overwritten by the interrupt function, resulting in bugs or stack corruption.
