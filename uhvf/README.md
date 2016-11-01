This is the beginning of a port of ukvm to Hypervisor.framework on
MacOSX, so that Solo5/Mirage-based unikernels can run natively on that
system.  The goal is for ukvm to end up with both a KVM-based bottom
half and Hypervisor.framework-based bottom half, whereas the top half
is shared.

Solo5 doesn't build properly in OSX yet (although @hannesm has done a
bunch of work to make it build on FreeBSD, so I don't think it's far
off), so I build using Docker for Mac with a simple build container. I
also use containers to build Mirage unikernels.  See
https://github.com/djwillia/dockerfiles.

At the moment, uhvf can do the Solo5 hello test and also run the Mirage
console test (from mirage-skeleton).

- need to implement modules: net, blk, gdb (is it finished?)

- KVM doesn't allow a trap on `rdtsc` but it should if we want to use
  the same interface for ukvm and uhvf (for e.g., det replay)

- `uhvf.c` and `ukvm-core.c` share a bunch of code; there should be a
  common part and a platform specific part at some point.


Older notes:

- It looks like the PVCLOCK can be completely removed from the ukvm
  parts of Solo5, as long as we change the poll hypercall to send the
  `until_nsecs` directly

- All interrupt handlers can be removed from the solo5 parts of ukvm
  because we get to see what exception happened in uhvf




