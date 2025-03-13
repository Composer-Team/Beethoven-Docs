# Beethoven Hardware Stack

A design of an accelerator is broken down into two parts.
First, there is the functional unit implementation, the core.
You would typically implement this in your HDL of choice.
We are huge fans of [Chisel HDL](https://www.chisel-lang.org), but we do understand the necessity of [System]Verilog,
so we have added [various utilities](/Beethoven/HW/Verilog) to make it easier to integrate external Verilog modules into your design.

Second, there is the [accelerator configuration](/Beethoven/HW/Config).
The configuration informs Beethoven _how_ to build your accelerator:
- What cores do you want in your design?
- How many of each?
- How are they connected to memory?

Before we dive into the configuration, let's look at a simple accelerator core implementation.
[[Next]](/Beethoven/HW/Core)
