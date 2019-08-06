# Getting Started example

This repository contains a single self-contained example with all files require to get a working solution that produces valid VHDL. The SME tools use C# as the programming language and runs with both Mono, Microsoft .Net and .Net Core compilers and runtimes. I recommend using .Net Core as it works the same across all operating systems.

# Prerequisites

Before you can run this example, make sure you have installed:
- [.Net Core SDK](https://dotnet.microsoft.com/download) with at least version 2.0

You can use any editor you like, but I recommend installing VS Code and the C# extensions as they work well on all operating systems:
- [VS Code](https://code.visualstudio.com/)
- [VS C# extensions](https://code.visualstudio.com/docs/languages/csharp)

You can test the generated VHDL in your favorite VHDL simulator, but I recommend using GHDL, as it is small, fast, standard compliant, and works on all operating systems:
- https://github.com/ghdl/ghdl/releases

# Running the example project

Once installed, you can [download this project and all files](https://github.com/kenkendk/sme-gettingstarted/archive/master.zip), and extract it to a fitting place.

Inside the folder where the files are extracted you can simply execute `dotnet run` and it will download all dependencies, compile the project, and run it.

You can perform these steps on the commandline on Linux/MacOS:
```bash
> curl -L "https://github.com/kenkendk/sme-gettingstarted/archive/master.zip" > gettingstarted.zip
> unzip gettingstarted.zip
> cd sme-gettingstarted-master
> dotnet run
Writing 648 pixels from image1.png
Still need to write 624 pixels
Still need to write 600 pixels
...
Still need to write 24 pixels
Still need to write 0 pixels
```

After the `run` command has completed you will have an `output/vhdl` folder which contains the generated VHDL code. 

# Testing with GHDL

Inside the `output/vhdl` folder you will find a `Makefile` that can compile and run the project using GHDL. Running the project with GHDL will use a trace from the C# execution to drive the simulation and verify that all signals in the VHDL code are clock-cycle accurate with the C# simulation. If you are not using GHDL, you can ignore the `Makefile` and instead use the VHDL files with your favorite tools.

An example run could look like this
```
> cd output/vhdl
> make
mkdir work
ghdl -a --std=93c --ieee=synopsys --workdir=work system_types.vhdl
ghdl -a --std=93c --ieee=synopsys --workdir=work Types_GettingStarted.vhdl
ghdl -a --std=93c --ieee=synopsys --workdir=work ColorBinCollector.vhdl
ghdl -a --std=93c --ieee=synopsys --workdir=work csv_util.vhdl
ghdl -a --std=93c --ieee=synopsys --workdir=work GettingStarted.vhdl
ghdl -a --std=93c --ieee=synopsys --workdir=work TestBench_GettingStarted.vhdl
ghdl -e --std=93c --ieee=synopsys --workdir=work GettingStarted_tb
cp "../trace.csv" .
ghdl -r --std=93c --ieee=synopsys --workdir=work GettingStarted_tb --vcd=trace.vcd
TestBench_GettingStarted.vhdl:250:9:@6515ns:(report note): completed successfully after 651 clockcycles
ghdl -a --std=93c --ieee=synopsys --workdir=work Export_GettingStarted.vhdl
```

# Building an FPGA bitstream

The `output/vhdl` folder also contains an `xpr` file which allows you to load the solution directly into [Xilinx Vivado](https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html). If you have a [Pynq](http://www.pynq.io/), [ZedBoard](http://zedboard.org/product/zedboard) or similar Zynq based device you can use the free Vivado WebPack version to generate a working FPGA bitstream.

*Note*: you need to configure pin mappings and clock contraints with the vendor tool before you can get the design to run on an actual FPGA device.

