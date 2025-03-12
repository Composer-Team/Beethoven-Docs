# Getting Started With Beethoven

Beethoven provides a software and hardware ecosystem for designing, assembling, and deploying hardware accelerators.
Below we show a high-level break-down of Beethoven into its hardware and software components.

<p align="center">
    <map name="GraffleExport">
        <area shape="rect" coords="1,59,159,90" href="/Beethoven/sw/testbench"/>
    </map>
    <img src="/img/figs/sitemap.jpg" usemap="#GraffleExport"/>
</p>

At the center of it all is your hardware accelerator "core" implementation - a single functional unit.
The goal is to make it comfortable to
- **Manage host-accelerator communication from the HW**: Control signals from the host should align to your implemented algorithm and be otherwise straightforward to use.
- **Use this functional unit from the Host**: Software implementations should not need to massively complicate the codebase for utilizing this accelerated function.
- **Manage data**: Regardless of how the memory system is organized on your platform, it should be easy to access memory from your accelerator and read back results from the host. 
- **Scale your system**: TODO 

We will cover each of these points in this documentation. You can click on the blocks in the figure above to explore Beethoven's capabilities, or continue on to the first section: [Beethoven HW Stack](/Beethoven/HW). 
