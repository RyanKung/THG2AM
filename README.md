# The Hitchhiker's Guide to the Actor Model in Rust

# Overview

* Introduction to Actor Model
	- History
	- Compare with Pi-Calcui
* Introduction to Actix & Actix-web
	- Actor
	- Arbiter
* More Actor Types
	- SyncArbiter
	- Supervisor
	- SystemServices
* Ghost in the Shell
	- Async
	- Tokio
	- Mio
* Issues & Solution
	- no panic!
	- Towel needed: wrap, wrap, and wrap
* Thanks to all the Fish
	- cite and ref


# Introduction

* Actor Model

A system is distributed is the message transmision delay is not negligible compared to the time between events in a single process.

- Leslie Lamport, Times ,Clocks, and the Ordering of Events in a Distributed System



```
https://lamport.azurewebsites.net/pubs/interprocess.pdf
On Interprocess Communication
Leslie Lamport
December 25, 1985
```
* origin

The Paper AI


* Actor Model and CSP

Contrast with other models of message-passing concurrency
Robin Milner's initial published work on concurrency[21] was also notable in that it was not based on composing sequential processes. His work differed from the actor model because it was based on a fixed number of processes of fixed topology communicating numbers and strings using synchronous communication. The original communicating sequential processes (CSP) model[22] published by Tony Hoare differed from the actor model because it was based on the parallel composition of a fixed number of sequential processes connected in a fixed topology, and communicating using synchronous message-passing based on process names (see Actor model and process calculi history). Later versions of CSP abandoned communication based on process names in favor of anonymous communication via channels, an approach also used in Milner's work on the calculus of communicating systems and the π-calculus.

https://en.wikipedia.org/wiki/Actor_model


* Actor model and process calculi history

https://en.wikipedia.org/wiki/Actor_model_and_process_calculi_history


# Actix

* System

- System is an actor which manages runtime.

1. Before starting any actix's actors, System actor has to be created and started with System::run() call.
2. This method creates new Arbiter in current thread and starts System actor.

```rust
extern crate actix;

use actix::prelude::*;
use std::time::Duration;

struct Timer {
    dur: Duration,
}

impl Actor for Timer {
    type Context = Context<Self>;

    // stop system after `self.dur` seconds
    fn started(&mut self, ctx: &mut Context<Self>) {
        ctx.run_later(self.dur, |act, ctx| {
            // Stop current running system.
            System::current().stop();
        });
    }
}

fn main() {
    // initialize system and run it.
    // This function blocks current thread
    let code = System::run(|| {
        // Start `Timer` actor
        Timer {
            dur: Duration::new(0, 1),
        }.start();
    });

    std::process::exit(code);
}
```

* Actor
	- state of Actor
	- Supervisor

* Arbiter

1. Arbiter controls event loop in its thread.
2. Each arbiter runs in separate thread.
3. Arbiter provides several api for event loop access.
4. Each arbiter can belongs to specific System actor.



* Sync Arbiter
	- with Postgres Arbbiter

# Deeper

* Tokio
* PThread

# Issues
## No Panic!
* Panic! like erlang
## Bring your Towel
* Abstract Message Handler

# Solution
