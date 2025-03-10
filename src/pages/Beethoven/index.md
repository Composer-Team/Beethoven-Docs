# Getting Started With Beethoven

Beethoven provides a software and hardware ecosystem for designing, assembling, and deploying hardware accelerators.
Below we show a high-level break-down of Beethoven into its hardware and software components.

<p align="center">
    <map name="GraffleExport">
        <area shape="rect" coords="1,43,115,65" href="https://google.com" />
    </map>
    <figure>
    <img src="img/figs/sitemap.jpg" usemap="#GraffleExport" />
    <figcaption> Each of these is clickable - click to learn more about a specific component!</figcaption>
    </figure>
</p>

At the center of it all is your hardware accelerator "core" implementation - a single functional unit.
Each core is controlled by the Host CPU through its [Host Interface](/Beethoven/host), and is able to
launch [memory transactions](/Beethoven/memory).


