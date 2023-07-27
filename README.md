# open5gs-bpf: Open5GS BPF applications

[![Github Actions](https://github.com/maikovisky/open5gs-bpf/actions/workflows/build.yml/badge.svg)](https://github.com/maikovisky/open5gs-bpf/actions/workflows/build.yml)

# Building

open5gs-bpf supports multiple build systems that do the same thing.
This serves as a cross reference for folks coming from different backgrounds.

## Install Dependencies

You will need `clang` (at least v11 or later), `libelf` and `zlib` to build
the examples, package names may vary across distros.

On Ubuntu/Debian, you need:
```shell
$ apt install clang cmake libelf1 libelf-dev zlib1g-dev llvm-dev libbfd-dev
```

On CentOS/Fedora, you need:
```shell
$ dnf install clang elfutils-libelf elfutils-libelf-devel zlib-devel
```
## Getting the source code

Download the git repository and check out submodules:

```shell
$ git clone --recurse-submodules https://github.com/maikovisky/open5gs-bpf.git
```

## Compile

```shell
$ cd src
$ make
```

## See log

```shell
$ cat /sys/kernel/debug/tracing/trace_pipe
```

# Troubleshooting

## Can't see log

```shell
$ mount -t debugfs none /sys/kernel/debug
```

Libbpf debug logs are quire helpful to pinpoint the exact source of problems,
so it's usually a good idea to look at them before starting to debug or
posting question online.

`./minimal` is always running with libbpf debug logs turned on.

For `./bootstrap`, run it in verbose mode (`-v`) to see libbpf debug logs:

```shell
$ sudo ./bootstrap -v
libbpf: loading object 'bootstrap_bpf' from buffer
libbpf: elf: section(2) tp/sched/sched_process_exec, size 384, link 0, flags 6, type=1
libbpf: sec 'tp/sched/sched_process_exec': found program 'handle_exec' at insn offset 0 (0 bytes), code size 48 insns (384 bytes)
libbpf: elf: section(3) tp/sched/sched_process_exit, size 432, link 0, flags 6, type=1
libbpf: sec 'tp/sched/sched_process_exit': found program 'handle_exit' at insn offset 0 (0 bytes), code size 54 insns (432 bytes)
libbpf: elf: section(4) license, size 13, link 0, flags 3, type=1
libbpf: license of bootstrap_bpf is Dual BSD/GPL
...
```
