# flow library loading sometimes take too long

flow constantly takes ~45sec to ~1min to load the following 3 line library:

```js
declare class AAA { aaa: Element }
declare class BBB { bbb: Element }
declare interface BUG extends BBB, AAA {}
```

This repo contains everything to reproduce the bug.

logs showing it takes ~45 sec :

```js
[2018-02-21 22:41:41] argv=/Users/rvion/dev/repro/node_modules/flow-bin/flow-osx-v0.66.0/flow start --temp-dir /tmp/flow /Users/rvion/dev/repro
[2018-02-21 22:41:41] Watching paths:
	/Users/rvion/dev/repro
[2018-02-21 22:41:41] Initializing Server (This might take some time)
[2018-02-21 22:41:41] executable=/Users/rvion/dev/repro/node_modules/flow-bin/flow-osx-v0.66.0/flow
[2018-02-21 22:41:41] version=0.66.0
[2018-02-21 22:41:41] Parsing
[2018-02-21 22:41:41] Building package heap
[2018-02-21 22:41:41] Loading libraries
[2018-02-21 22:42:27] Resolving dependencies
[2018-02-21 22:42:28] to_merge: Focused: 2, Dependents: 0, Dependencies: 0
[2018-02-21 22:42:28] Calculating dependencies
[2018-02-21 22:42:28] Merging
[2018-02-21 22:42:28] Done
[2018-02-21 22:42:28] Checked set: Focused: 2, Dependents: 0, Dependencies: 0
[2018-02-21 22:42:28] Server is READY
[2018-02-21 22:42:28] Took 46.600281 seconds to initialize.
[2018-02-21 22:42:28] Status: OK
```

## I

```
./node_modules/.bin/flow version
Flow, a static type checker for JavaScript, version 0.66.0
```

.flowconfig:

```ini
[ignore]
.*/node_modules/*

[libs]
./lib.js
```

```sh
./node_modules/.bin/flow ls --explain
ImplicitlyIncluded    /Users/rvion/dev/repro/package.json
ImplicitlyIncluded    /Users/rvion/dev/repro/package-lock.json
ExplicitLib           /Users/rvion/dev/repro/lib.js
ConfigFile            /Users/rvion/dev/repro/.flowconfig
```

```sh
killall flow
./node_modules/.bin/flow

No matching processes belonging to you were found
Launching Flow server for /Users/rvion/dev/repro
Spawned flow server (pid=87797)
Logs will go to /private/tmp/flow/zSUserszSrvionzSdevzSrepro.log
Monitor logs will go to /private/tmp/flow/zSUserszSrvionzSdevzSrepro.monitor_log
Please wait. Server is initializing (parsed files 3): -

# [... wait time ...]
```

---

:memo: I came across this bug while tesing my ts2flow converter that generates libdefs from \*.d.ts files.
