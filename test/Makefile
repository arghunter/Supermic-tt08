# Makefile
# See https://docs.cocotb.org/en/stable/quickstart.html for more info

# defaults
SIM ?= icarus
TOPLEVEL_LANG ?= verilog
SRC_DIR = $(PWD)/../src
PROJECT_SOURCES = project.v
PROJECT_SOURCES += ddr_to_sdr_dual.v
PROJECT_SOURCES += multi_ddr_to_sdr_dual.v
PROJECT_SOURCES += adder_23bit.v
PROJECT_SOURCES += adder_16x23bi.v
PROJECT_SOURCES += integrator1.v
PROJECT_SOURCES += integrator2.v
PROJECT_SOURCES += integrator3.v
PROJECT_SOURCES += integrator_top_module.v
PROJECT_SOURCES += comb.v
PROJECT_SOURCES += cic.v
PROJECT_SOURCES += delay_line.v
PROJECT_SOURCES += delay_module.v
PROJECT_SOURCES += mux_shift.v
PROJECT_SOURCES += i2s_bus.v
PROJECT_SOURCES += supermic_top_module.v
PROJECT_SOURCES += full_adder.v

ifneq ($(GATES),yes)

# RTL simulation:
SIM_BUILD				= sim_build/rtl
VERILOG_SOURCES += $(addprefix $(SRC_DIR)/,$(PROJECT_SOURCES))
COMPILE_ARGS 		+= -I$(SRC_DIR)

else

# Gate level simulation:
SIM_BUILD				= sim_build/gl
COMPILE_ARGS    += -DGL_TEST
COMPILE_ARGS    += -DFUNCTIONAL
COMPILE_ARGS    += -DUSE_POWER_PINS
COMPILE_ARGS    += -DSIM
COMPILE_ARGS    += -DUNIT_DELAY=\#1
VERILOG_SOURCES += $(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/verilog/primitives.v
VERILOG_SOURCES += $(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/verilog/sky130_fd_sc_hd.v

# this gets copied in by the GDS action workflow
VERILOG_SOURCES += $(PWD)/gate_level_netlist.v

endif

# Include the testbench sources:
VERILOG_SOURCES += $(PWD)/tb.v
TOPLEVEL = tb

# MODULE is the basename of the Python test file
MODULE = test

# include cocotb's make rules to take care of the simulator setup
include $(shell cocotb-config --makefiles)/Makefile.sim
