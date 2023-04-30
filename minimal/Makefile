
PROJ      = blinky
BUILD     = ./build
DEVICE    = 1k
FOOTPRINT = tq144

FILES = top.v

.PHONY: all clean burn world timings

all: $(BUILD)/$(PROJ).bin

$(BUILD)/$(PROJ).bin: top.v pinmap.pcf
	mkdir -p $(BUILD)
	yosys -p "synth_ice40 -top top -json $(BUILD)/$(PROJ).json" $(FILES)
	nextpnr-ice40 --hx1k --package tq144 --json $(BUILD)/$(PROJ).json --asc $(BUILD)/$(PROJ).asc --pcf pinmap.pcf 
	icepack $(BUILD)/$(PROJ).asc $(BUILD)/$(PROJ).bin

burn: $(BUILD)/$(PROJ).bin
	iceprog -d i:0x0403:0x6010:0 $(BUILD)/$(PROJ).bin

timings:
	icetime -tmd hx$(DEVICE)  $(BUILD)/$(PROJ).asc


tb.vvp: tb.v top.v
	iverilog -o $@ $^

tb.vcd: tb.vvp
	vvp tb.vvp

plot: tb.vcd
	gtkwave tb.vcd

clean:
	rm -rf build obj_dir *.vvp *.vcd

world: clean all
