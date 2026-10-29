<div align="center">

# BinRacer

**Security research on memory allocators.**

The glibc heap and the Linux kernel, studied through reproducible labs and GDB
tooling for inspecting a live kernel.

[![Blog](https://img.shields.io/badge/blog-binracer.github.io-181717.svg)](https://binracer.github.io/)
[![KernLens](https://img.shields.io/badge/KernLens-docs-4a9eff.svg)](https://binracer.github.io/KernLens/)

</div>

The work here studies how two allocators behave — `malloc` in userland and SLUB in
the kernel — and publishes each finding as a lab that reproduces it: a target, a
launcher, and an expected result, so a claim can be checked rather than accepted.

| | Allocator | A wrong read shows up as |
| --- | --- | --- |
| **glibc** | `malloc`, in userland | a crash, at the point of the error |
| **SLUB** | the kernel's allocator | plausible data — the error stays hidden |

The second row is the reason for the kernel tooling. Without a way to inspect the
object itself, an incorrect offset returns a value that looks valid, and nothing
downstream reveals the mistake.

## Projects

| Project | Scope | Scale |
| --- | --- | --- |
| [**pwn4heap**](https://github.com/BinRacer/pwn4heap) | glibc allocator behaviour as Python labs, one per branch a technique applies to | 69 techniques · 236 labs · glibc 2.23–2.39 · MIT |
| [**pwn4kernel**](https://github.com/BinRacer/pwn4kernel) | kernel memory behaviour as standalone QEMU labs, with mitigations enabled | 24 techniques · 66 labs · 26 builds · kernels 4.4–6.14 · GPL-3.0 |
| [**KernLens**](https://github.com/BinRacer/KernLens) | 30 GDB commands for page tables, SLUB caches, `struct page` and KASLR | Python · x86-64 · GPL-3.0 |

The 236 `pwn4heap` labs share 78 target binaries: the technique is the subject, and
each target ships the `ld` and `libc` of the branch it was built for. Every
`pwn4kernel` launcher boots with SMEP, SMAP, KPTI and KASLR enabled, so a lab
describes behaviour under the mitigations a current kernel actually has.

## See it run

<p align="center">
  <img src="https://binracer.github.io/KernLens/assets/demo.svg"
       alt="kl-walk decoding a 2 MiB page-table mapping, then kl-slab details decoding a SLUB freelist pointer on a 6.1.134 kernel"
       width="100%">
</p>

`kl-walk` decoding a 2 MiB page-table mapping, then `kl-slab details` decoding a
freelist pointer out of a real object chain on a 6.1.134 kernel — verbatim from a
recorded session.

KernLens reports the entry each answer came from, so an incorrect read is visible
rather than plausible, and resolves structure layouts from DWARF, then the image's
own BTF, then a versioned registry. That ordering is what lets it work on a
distribution's stripped `vmlinux` and not only on a debug package.

[Source](https://github.com/BinRacer/KernLens) ·
[Docs](https://binracer.github.io/KernLens/) ·
[PyPI](https://pypi.org/project/kernlens/)

## Background

Systems programming in C, C++, Rust, Go and Python, with a focus on x86-64. Prior
work: pwnable.kr and pwnable.tw, completed.

---

<div align="center">

*"First, solve the problem. Then, write the code." — John Johnson*

[blog](https://binracer.github.io/) · [KernLens docs](https://binracer.github.io/KernLens/) · [PyPI](https://pypi.org/project/kernlens/)

</div>