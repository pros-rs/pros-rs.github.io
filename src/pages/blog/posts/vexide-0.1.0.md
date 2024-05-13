---
layout: ../../../layouts/BlogPostLayout.astro
title: vexide v0.1.0
description: All about the first version of vexide
author: gavin-niederman
thumbnail: "https://images.unsplash.com/photo-1704895390342-b52a2f45786c?q=80&w=1932&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
thumbnail_alt: "Thumbnail Image"
tag: release
date: 2024-05-11
---

vexide has now officially been released! <br />
vexide is still relatively early in development and therefore has an unstable API surface,
but it is nearly feature complete.

If you end up using or contributing to vexide or just want to learn more,
please join [our discord server](https://discord.gg/y9mcGuQRYz) and tell us how it goes.

# What is vexide

vexide is a brand new library for programming V5 Brains.
Unlike the two major V5 Brain libraries (VEXCode and PROS), vexide is written in Rust instead of C and C++.
For this reason vexide has many advantages over PROS and VEXCode.
- vexide uses Rust async/await instead of an RTOS.
- vexide binaries are small, like, *really small* compared to PROS.
- All device errors need to be explicitly handled or ignored.
- vexide gives you complete control over what runs in your programs. Don't want a banner to print on startup? Disable the feature.
- vexide programs are built and linked with LLVM instead of the bulky ARM GNU Toolchain. 
- vexide is frequently updated. VEXCode will always get new features first, but vexide will almost certainly get them before PROS.

Every single line of code in vexide is open source and 100% Rust. 
vexide doesn't depend on any external C libraries (even libv5rt).
This means that anyone can build and contribute to vexide with ease.
It is impossible to contribute to VEXCode and you very rarely see anyone outside of Purdue contribute to PROS for a good reason.


# In practice

How does code written with vexide look?
Most APIs in vexide are asynchronous.
Here is a quick example showing a working program using the ``CompetitionRobot`` API.

```rust
#![no_main]
#![no_std]

use vexide::prelude::*;

struct Robot;

impl CompetitionRobot for Robot {
    type Error = ();
    async fn driver(&mut self) -> Result<(), ()> {
        println!("Driver");
        Ok(())
    }
}

#[vexide::main]
async fn main(_peripherals: Peripherals) {
    Robot.compete().await.unwrap();
}

```

Most device types can be constructed infallibly, but all methods called after the fact return ``Result``s.

```rust
// Infallibly create a motor
let mut motor = Motor::new(peripherals.port_1, Gearset::Green, Direction::Forward);
// Set the voltage or return early if there is an error.
motor.set_voltage(12.0)?;
// Set the voltage and ignore any error.
motor.set_voltage(0.0).ok();

```

# Should you use vexide

Like any library vexide has pros and cons.
I've gone over a lot of the pros already, but not any of the cons.
vexide has one major con: a lack of hardware testing.
vexide is very new and thus hasn't been thoroughly tested.

So should you use it?
For a competition, I do not advise using vexide ***yet***.
However, I encourage you to give vexide a try and help us get it to a state where we are confident in competition readiness.