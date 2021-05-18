#
# this file was created by a computer. trust it.
#

# compiler tools
XILINX_VITIS ?= /tools/Xilinx/Vitis/2020.1
XILINX_XRT ?= /opt/xilinx/xrt
XILINX_VIVADO ?= /tools/Xilinx/Vivado/2020.1
XILINX_VIVADO_HLS ?= $(XILINX_VITIS)/Vivado_HLS

HOST_CXX ?= g++
VPP ?= ${XILINX_VITIS}/bin/v++
EMCONFIGUTIL = ${XILINX_VITIS}/bin/emconfigutil --od .
RM = rm -f
RMDIR = rm -rf

VITIS_PLATFORM = xilinx_u200_xdma_201830_2
VITIS_PLATFORM_PATH = $(VITIS_PLATFORM)

# host compiler global settings
CXXFLAGS += -std=c++0x -DVITIS_PLATFORM=$(VITIS_PLATFORM) -D__USE_XOPEN2K8 -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/ext/xcl2 -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/ext/quantlib -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/ext/rng -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L3/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L3/include/models -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/ext/xcl2 -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L3/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L3/include/models -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L2/tests/CFBlackScholesMerton/src/kernel -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L1/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L2/include -I$(XILINX_XRT)/include/ -I$(XILINX_VIVADO)/include/ -O0 -g -Wall -O3  -c -fmessage-length=0
LDFLAGS += -lxilinxopencl -lpthread -lrt -lstdc++ -lmpfr -lgmp -lhlsmc++-GCC46 -lIp_floating_point_v7_0_bitacc_cmodel -lIp_xfft_v9_1_bitacc_cmodel -lIp_fir_compiler_v7_2_bitacc_cmodel -lIp_dds_compiler_v6_0_bitacc_cmodel -L$(XILINX_XRT)/lib/ -L$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -L$(XILINX_VIVADO)/lnx64/lib/csim -L$(XILINX_VIVADO)/lnx64/tools/dds_v6_0 -L$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -L$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath-link,$(XILINX_XRT)/lib -Wl,-rpath,$(XILINX_VIVADO)/lnx64/lib/csim -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fpo_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fft_v9_1 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/fir_v7_0 -Wl,-rpath,$(XILINX_VIVADO)/lnx64/tools/dds_v6_0

# hardware compiler shared settings
VPP_OPTS = --target sw_emu

#
# OpenCL kernel files
#

BINARY_CONTAINERS += bsm_kernel.xclbin

BUILD_SUBDIRS += bsm_kernel.build
BSM_KERNEL_OBJS += bsm_kernel.build/bsm_kernel.xo
ALL_KERNEL_OBJS += bsm_kernel.build/bsm_kernel.xo

ALL_MESSAGE_FILES = $(subst .xo,.mdb,$(ALL_KERNEL_OBJS)) $(subst .xclbin,.mdb,$(BINARY_CONTAINERS))

#
# host files
#

HOST_OBJECTS += libs/xf_fintech/L3/src/models/cf_black_scholes/src/xf_fintech_cf_black_scholes.o
HOST_OBJECTS += libs/xf_fintech/L3/src/models/cf_black_scholes_merton/src/xf_fintech_cf_black_scholes_merton.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_device.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_device_manager.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_internal.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_ocl_controller.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_timestamp.o
HOST_OBJECTS += libs/xf_fintech/L3/src/xf_fintech_trace.o
HOST_OBJECTS += libs/xf_fintech/ext/xcl2/xcl2.o
HOST_OBJECTS += src/xf_fintech_cf_BlackScholesMerton.o

HOST_EXE = test_alveo

BUILD_SUBDIRS += libs/xf_fintech/L3/src/models/cf_black_scholes/src/
BUILD_SUBDIRS += libs/xf_fintech/L3/src/models/cf_black_scholes_merton/src/
BUILD_SUBDIRS += libs/xf_fintech/L3/src/
BUILD_SUBDIRS += libs/xf_fintech/ext/xcl2/
BUILD_SUBDIRS += src/

EMCONFIG_FILE = emconfig.json

#
# primary build targets
#

.PHONY: all clean
all: $(BINARY_CONTAINERS) $(HOST_EXE) $(EMCONFIG_FILE)

.NOTPARALLEL: clean

clean-host:
	-$(RM) $(HOST_EXE) $(HOST_OBJECTS)

clean-accelerators:
	-$(RM) $(BINARY_CONTAINERS) $(ALL_KERNEL_OBJS) $(ALL_MESSAGE_FILES)  $(EMCONFIG_FILE)
	-$(RM) *.xclbin.sh *.xclbin.info *.xclbin.link_summary*
	-$(RMDIR) $(BUILD_SUBDIRS)
	-$(RMDIR) .Xil

clean-package:
	-${RMDIR} package
	-${RMDIR} package.build

clean: clean-host clean-accelerators clean-package

.PHONY: incremental
incremental: all

include makeemconfig.mk

nothing:

#
# binary container: bsm_kernel.xclbin
#

bsm_kernel.build/bsm_kernel.xo: ../libs/xf_fintech/L2/tests/CFBlackScholesMerton/src/kernel/bsm_kernel.cpp ../libs/xf_fintech/L2/tests/CFBlackScholesMerton/src/kernel/bus_interface.hpp ../../../../../Vivado/2020.1/include/hls_math.h ../libs/xf_fintech/L2/include/xf_fintech/cf_bsm.hpp bsm_kernel-bsm_kernel-compile.cfg common-config.cfg
	-@mkdir -p $(@D)
	-@$(RM) $@
	$(VPP) $(VPP_OPTS) --compile -I"$(<D)" --config common-config.cfg --config bsm_kernel-bsm_kernel-compile.cfg -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L1/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L2/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L1/include -I$(XILINX_VITIS)/workspace/test_alveo/libs/xf_fintech/L2/tests/CFBlackScholesMerton/src/kernel -o"$@" "$<"

bsm_kernel.xclbin: $(BSM_KERNEL_OBJS) bsm_kernel-link.cfg common-config.cfg
	-@echo $(VPP) $(VPP_OPTS) --link --config common-config.cfg --config bsm_kernel-link.cfg -o"$@" $(BSM_KERNEL_OBJS) > bsm_kernel.xclbin.sh
	$(VPP) $(VPP_OPTS) --link --config common-config.cfg --config bsm_kernel-link.cfg -o"$@" $(BSM_KERNEL_OBJS)

#
# host rules
#

libs/xf_fintech/L3/src/models/cf_black_scholes/src/xf_fintech_cf_black_scholes.o: ../libs/xf_fintech/L3/src/models/cf_black_scholes/src/xf_fintech_cf_black_scholes.cpp ../libs/xf_fintech/L3/include/xf_fintech_error_codes.hpp ../libs/xf_fintech/L3/include/xf_fintech_trace.hpp ../libs/xf_fintech/L3/include/models/xf_fintech_cf_black_scholes.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/models/cf_black_scholes_merton/src/xf_fintech_cf_black_scholes_merton.o: ../libs/xf_fintech/L3/src/models/cf_black_scholes_merton/src/xf_fintech_cf_black_scholes_merton.cpp ../libs/xf_fintech/L3/include/xf_fintech_error_codes.hpp ../libs/xf_fintech/L3/include/xf_fintech_trace.hpp ../libs/xf_fintech/L3/include/models/xf_fintech_cf_black_scholes_merton.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_device.o: ../libs/xf_fintech/L3/src/xf_fintech_device.cpp ../libs/xf_fintech/L3/include/xf_fintech_device.hpp ../libs/xf_fintech/L3/include/xf_fintech_error_codes.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_device_manager.o: ../libs/xf_fintech/L3/src/xf_fintech_device_manager.cpp ../libs/xf_fintech/L3/include/xf_fintech_device_manager.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_internal.o: ../libs/xf_fintech/L3/src/xf_fintech_internal.cpp ../libs/xf_fintech/L3/include/xf_fintech_internal.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_ocl_controller.o: ../libs/xf_fintech/L3/src/xf_fintech_ocl_controller.cpp ../libs/xf_fintech/L3/include/xf_fintech_error_codes.hpp ../libs/xf_fintech/L3/include/xf_fintech_ocl_controller.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_timestamp.o: ../libs/xf_fintech/L3/src/xf_fintech_timestamp.cpp ../libs/xf_fintech/L3/include/xf_fintech_timestamp.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/L3/src/xf_fintech_trace.o: ../libs/xf_fintech/L3/src/xf_fintech_trace.cpp ../libs/xf_fintech/L3/include/xf_fintech_trace.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

libs/xf_fintech/ext/xcl2/xcl2.o: ../libs/xf_fintech/ext/xcl2/xcl2.cpp ../libs/xf_fintech/ext/xcl2/xcl2.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

src/xf_fintech_cf_BlackScholesMerton.o: ../src/xf_fintech_cf_BlackScholesMerton.cpp ../libs/xf_fintech/L3/include/xf_fintech_api.hpp ../src/test_vectors_bsm.hpp
	-@mkdir -p $(@D)
	$(HOST_CXX) $(CXXFLAGS) -o "$@" "$<"

$(HOST_EXE): $(HOST_OBJECTS)
	$(HOST_CXX) -o "$@" $(+) $(LDFLAGS)
	-@echo $(VPP) --package --config package.cfg  bsm_kernel.xclbin
	-@$(VPP) --package --config package.cfg  bsm_kernel.xclbin


#
# emulation configuration file
#

$(EMCONFIG_FILE): makeemconfig.mk
	$(EMCONFIGUTIL) --nd $(NUMBER_OF_DEVICES)  --platform $(VITIS_PLATFORM_PATH)
	-@$(RMDIR) TempConfig .Xil/configutil*

