
# rt-tests snap

[![rt-tests](https://snapcraft.io/rt-tests/badge.svg)](https://snapcraft.io/rt-tests)

This is a snap packaging of
[rt-tests](https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/rt-tests),
a set of programs to test various real-time Linux features.

The programs include:

- [cyclicdeadline](https://manpages.ubuntu.com/manpages/noble/man8/cyclicdeadline.8.html)
- [cyclictest](https://manpages.ubuntu.com/manpages/noble/en/man8/cyclictest.8.html)
- [deadline_test](https://manpages.ubuntu.com/manpages/noble/man8/deadline_test.8.html) as `deadline-test` in this snap
- [hackbench](https://manpages.ubuntu.com/manpages/noble/man8/hackbench.8.html)
- [hwlatdetect](https://manpages.ubuntu.com/manpages/noble/en/man8/hwlatdetect.8.html)
- [oslat](https://manpages.ubuntu.com/manpages/noble/en/man8/oslat.8.html)
- [pi_stress](https://manpages.ubuntu.com/manpages/noble/en/man8/pi_stress.8.html) as `pi-stress` in this snap
- [pip_stress](https://manpages.ubuntu.com/manpages/noble/en/man8/pip_stress.8.html) as `pip-stress` in this snap
- [pmqtest](https://manpages.ubuntu.com/manpages/noble/en/man8/pmqtest.8.html)
- [ptsematest](https://manpages.ubuntu.com/manpages/noble/en/man8/ptsematest.8.html) - Works only when `--threads` is set to it's default value `1` or when not defined. For any other number of threads, install the snap in [devmode](https://snapcraft.io/docs/install-modes#heading--devmode); see [#46](https://github.com/canonical/rt-tests-snap/issues/46)
- [queuelat](https://manpages.ubuntu.com/manpages/noble/en/man8/queuelat.8.html)
- [rt-migrate-test](https://manpages.ubuntu.com/manpages/noble/en/man8/rt-migrate-test.8.html)
- [signaltest](https://manpages.ubuntu.com/manpages/noble/en/man8/signaltest.8.html)
- [sigwaittest](https://manpages.ubuntu.com/manpages/noble/en/man8/sigwaittest.8.html)
- [ssdd](https://manpages.ubuntu.com/manpages/noble/en/man8/ssdd.8.html) - Works only when the snap is installed in [devmode](https://snapcraft.io/docs/install-modes#heading--devmode); see [#12](https://github.com/canonical/rt-tests-snap/issues/12)
- [svsematest](https://manpages.ubuntu.com/manpages/noble/en/man8/svsematest.8.html)

## Known Issues

This snap is under development.
Refer [here](https://github.com/canonical/rt-tests-snap/issues?q=is%3Aissue+is%3Aopen+label%3Abug) for the list of known issues.

## Install

To install it:

```bash
sudo snap install rt-tests
```

## Configure

> [!NOTE]
> All needed interfaces are auto-connected when installing from store:
>
> - [cpu-control](https://snapcraft.io/docs/cpu-control-interface) interface
> - [mount-observe](https://snapcraft.io/docs/mount-observe-interface) interface;
> - The `pmqtest` app is granted access to use POSIX message queues via the [posix-mq](https://snapcraft.io/docs/posix-mq-interface)  interface.
> - [process-control](https://snapcraft.io/docs/process-control-interface) interface;
> - [shared-memory](https://snapcraft.io/docs/shared-memory-interface) interface;
> - `sys-kernel-debug-sched-features` plug into the [system-files](https://snapcraft.io/docs/system-files-interface) interface;
> - [system-trace](https://snapcraft.io/docs/system-trace-interface) interface;

If the snap is [built locally](#local-build), it's necessary to manually connect the following interfaces:

```bash
sudo snap connect rt-tests:process-control
sudo snap connect rt-tests:mount-observe
sudo snap connect rt-tests:system-trace
sudo snap connect rt-tests:sys-kernel-debug-sched-features
sudo snap connect rt-tests:cpu-control
```

## Use

The program commands are available within the snap's namespace.
For example, the command for `cyclictest` is `rt-tests.cyclictest`:

On a Raspberry Pi 5 running Real-time Ubuntu 24.04.1 LTS:

```console
$ sudo rt-tests.cyclictest --mlockall --smp --priority=80 --interval=200 --distance=0 --loops=100000
# /dev/cpu_dma_latency set to 0us
policy: fifo: loadavg: 0.69 0.24 0.09 1/203 1134

T: 0 ( 1128) P:80 I:200 C: 100000 Min:      2 Act:    6 Avg:    7 Max:      26
T: 1 ( 1129) P:80 I:200 C:  99995 Min:      3 Act:    6 Avg:    7 Max:      20
T: 2 ( 1130) P:80 I:200 C:  99951 Min:      3 Act:    6 Avg:    7 Max:      24
T: 3 ( 1131) P:80 I:200 C:  99906 Min:      3 Act:    6 Avg:    7 Max:      21
```

### Add aliases

You can add [aliases](https://snapcraft.io/docs/commands-and-aliases) to run the program commands without the namespace. For example:

```console
$ sudo snap alias rt-tests.cyclictest cyclictest
Added:
  - rt-tests.cyclictest as cyclictest

$ which cyclictest
/snap/bin/cyclictest
```

Run the following commands to add aliases for all the programs, effectively making them available under the same names as if they were installed using the Debian package:

```bash

sudo snap alias rt-tests.cyclictest cyclictest
sudo snap alias rt-tests.cyclicdeadline cyclicdeadline 
sudo snap alias rt-tests.deadline-test deadline_test
sudo snap alias rt-tests.get-cyclictest-snapshot get-cyclictest-snapshot
sudo snap alias rt-tests.hackbench hackbench
sudo snap alias rt-tests.hwlatdetect hwlatdetect
sudo snap alias rt-tests.oslat oslat
sudo snap alias rt-tests.pip-stress pip_stress
sudo snap alias rt-tests.pi-stress pi_stress
sudo snap alias rt-tests.pmqtest pmqtest
sudo snap alias rt-tests.ptsematest ptsematest
sudo snap alias rt-tests.queuelat queuelat
sudo snap alias rt-tests.rt-migrate-test rt-migrate-test
sudo snap alias rt-tests.signaltest signaltest
sudo snap alias rt-tests.sigwaittest sigwaittest
sudo snap alias rt-tests.ssdd ssdd
sudo snap alias rt-tests.svsematest svsematest
```

### Local Build

Firstly, build it using [Snapcraft](https://snapcraft.io/snapcraft):

```bash
snapcraft -v
```

Then, install it in [dangerous mode](https://snapcraft.io/docs/install-modes#heading--dangerous):

```bash
sudo snap install --dangerous *.snap
```
