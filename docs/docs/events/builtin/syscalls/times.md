
# times

## Intro
times - Get process and child process times

## Description
The `times()` syscall retrieves the running times of the current process and of given child processes. Upon successful completion, the `buf` argument, if it is not NULL, is filled with an array of `struct tms` values.

A `struct tms` value has the following form:

```
struct tms {
	clock_t tms_utime; // user time
	clock_t tms_stime; // system time
	clock_t tms_cutime; // user time of process and child processes
	clock_t tms_cstime; // system time of process and child processes
};
```

The `clock_t` type is an arithmetic type which is capable of representing times.

This syscall is useful for profiling applications or to measure their running time. It can also be used to get an idea of the time a process and its children spent in user and system mode.

## Arguments
* `buf`:`struct tms*`[U] - Pointer to a `struct tms`, filled with an array of `struct tms` values upon successful completion.

### Available Tags
* U - Originated from user space (Pointer to user space memory used to get it).

## Hooks
### sys_times
#### Type
Kprobe
#### Purpose
Hooked to record timing information for the `times()` syscall.

## Example Use Case
The `times()` syscall can be used in the following example to get more insight into a process and child processes running times:

```
struct tms t;
if(times(&t) != -1){
	printf("User time of this process: %lld ms\n", t.tms_utime * MSEC_PER_SEC);
	printf("System time of this process: %lld ms\n", t.tms_stime * MSEC_PER_SEC);
	printf("User time of this process and its childrens: %lld ms\n", t.tms_cutime * MSEC_PER_SEC);
	printf("System time of this process and its childrens: %lld ms\n", t.tms_cstime * MSEC_PER_SEC);
}
```

## Issues
None.

## Related Events
* `execve()` - execute a program
* `clone()` - create a child process

> This document was automatically generated by OpenAI and needs review. It might
> not be accurate and might contain errors. The authors of Tracee recommend that
> the user reads the "events.go" source file to understand the events and their
> arguments better.