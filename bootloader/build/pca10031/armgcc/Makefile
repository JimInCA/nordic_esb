PROJECT_NAME     := bootloader
TARGETS          := $(PROJECT_NAME)
OUTPUT_DIRECTORY := _build

SDK_ROOT := ../../../../../..
PROJ_DIR := ../../..

$(OUTPUT_DIRECTORY)/$(PROJECT_NAME).out: \
  LINKER_SCRIPT  := $(PROJECT_NAME)_gcc_nrf51.ld

# Source files common to all targets
SRC_FILES += \
  $(PROJ_DIR)/src/bootloader.c \
  $(PROJ_DIR)/src/bootloader_util_arm.c \
  $(PROJ_DIR)/src/dfu_dual_bank.c \
  $(PROJ_DIR)/src/dfu_transport_serial.c \
  $(PROJ_DIR)/src/interrupt_handler.c \
  $(PROJ_DIR)/src/main.c \
  $(SDK_ROOT)/components/boards/boards.c \
  $(SDK_ROOT)/integration/nrfx/legacy/nrf_drv_uart.c \
  $(SDK_ROOT)/components/libraries/util/app_error.c \
  $(SDK_ROOT)/components/libraries/util/app_error_weak.c \
  $(SDK_ROOT)/components/libraries/gpiote/app_gpiote.c \
  $(SDK_ROOT)/components/libraries/timer/app_timer.c \
  $(SDK_ROOT)/components/libraries/scheduler/app_scheduler.c \
  $(SDK_ROOT)/components/libraries/uart/app_uart.c \
  $(SDK_ROOT)/components/libraries/util/app_util_platform.c \
  $(SDK_ROOT)/components/libraries/crc16/crc16.c \
  $(SDK_ROOT)/components/libraries/hci/hci_mem_pool.c \
  $(SDK_ROOT)/components/libraries/hci/hci_slip.c \
  $(SDK_ROOT)/components/libraries/hci/hci_transport.c \
  $(SDK_ROOT)/modules/nrfx/drivers/src/nrfx_gpiote.c \
  $(SDK_ROOT)/modules/nrfx/drivers/src/nrfx_uart.c \
  $(SDK_ROOT)/modules/nrfx/mdk/gcc_startup_nrf51.S \
  $(SDK_ROOT)/modules/nrfx/mdk/system_nrf51.c \
  $(SDK_ROOT)/modules/nrfx/hal/nrf_nvmc.c \

# Include folders common to all targets
INC_FOLDERS += \
  $(PROJ_DIR)/include \
  $(SDK_ROOT)/components \
  $(SDK_ROOT)/components/boards \
  $(SDK_ROOT)/components/drivers_nrf/nrf_soc_nosd \
  $(SDK_ROOT)/components/libraries/atomic \
  $(SDK_ROOT)/components/libraries/balloc \
  $(SDK_ROOT)/components/libraries/bsp \
  $(SDK_ROOT)/components/libraries/crc16 \
  $(SDK_ROOT)/components/libraries/delay \
  $(SDK_ROOT)/components/libraries/experimental_section_vars \
  $(SDK_ROOT)/components/libraries/fifo \
  $(SDK_ROOT)/components/libraries/gpiote \
  $(SDK_ROOT)/components/libraries/hci \
  $(SDK_ROOT)/components/libraries/log \
  $(SDK_ROOT)/components/libraries/log/src \
  $(SDK_ROOT)/components/libraries/scheduler \
  $(SDK_ROOT)/components/libraries/strerror \
  $(SDK_ROOT)/components/libraries/timer \
  $(SDK_ROOT)/components/libraries/uart \
  $(SDK_ROOT)/components/libraries/util \
  $(SDK_ROOT)/components/proprietary_rf/esb \
  $(SDK_ROOT)/components/toolchain \
  $(SDK_ROOT)/components/toolchain/cmsis/include \
  $(SDK_ROOT)/integration/nrfx \
  $(SDK_ROOT)/integration/nrfx/legacy \
  $(SDK_ROOT)/modules/nrfx \
  $(SDK_ROOT)/modules/nrfx/drivers/include \
  $(SDK_ROOT)/modules/nrfx/hal \
  $(SDK_ROOT)/modules/nrfx/mdk \
  $(PROJ_DIR) \

# Libraries common to all targets
LIB_FILES += \

# Optimization flags
OPT  = -O3 -g3
# Save preprocessed file (.i), assemby file (.s), and object file (.o)
#OPT += -save-temps
# Uncomment the line below to enable link time optimization
#OPT += -flto

# C flags common to all targets
CFLAGS += $(OPT)
CFLAGS += -DBOARD_PCA10031
CFLAGS += -DBSP_DEFINES_ONLY
CFLAGS += -DCONFIG_GPIO_AS_PINRESET
CFLAGS += -DESB_PRESENT
CFLAGS += -DFLOAT_ABI_HARD
CFLAGS += -DNRF51
CFLAGS += -DNRF51422
CFLAGS += -DBOOTLOADER_BUILD
CFLAGS += -mcpu=cortex-m0
CFLAGS += -mthumb -mabi=aapcs
CFLAGS += -Wall -Werror
CFLAGS += -mfloat-abi=soft
# keep every function in a separate section, this allows linker to discard unused ones
CFLAGS += -ffunction-sections -fdata-sections -fno-strict-aliasing
CFLAGS += -fno-builtin -fshort-enums 

# C++ flags common to all targets
CXXFLAGS += $(OPT)

# Assembler flags common to all targets
ASMFLAGS += -g3
ASMFLAGS += -mcpu=cortex-m0
ASMFLAGS += -mthumb -mabi=aapcs
ASMFLAGS += -mfloat-abi=soft
ASMFLAGS += -DBOARD_PCA10031
ASMFLAGS += -DBSP_DEFINES_ONLY
ASMFLAGS += -DCONFIG_GPIO_AS_PINRESET
ASMFLAGS += -DESB_PRESENT
ASMFLAGS += -DFLOAT_ABI_HARD
ASMFLAGS += -DNRF51
ASMFLAGS += -DNRF51422
ASMFLAGS += -DBOOTLOADER_BUILD

# Linker flags
LDFLAGS += $(OPT)
LDFLAGS += -mthumb -mabi=aapcs -L$(SDK_ROOT)/modules/nrfx/mdk -T$(LINKER_SCRIPT)
LDFLAGS += -mcpu=cortex-m0
LDFLAGS += -mfloat-abi=soft
# let linker dump unused sections
LDFLAGS += -Wl,--gc-sections
# use newlib in nano version
LDFLAGS += --specs=nano.specs


# Add standard libraries at the very end of the linker input, after all objects
# that may need symbols provided by these libraries.
LIB_FILES += -lc -lnosys -lm


.PHONY: default help

# Default target - first one defined
default: $(PROJECT_NAME)

# Print all targets that can be built
help:
	@echo following targets are available:
	@echo		$(PROJECT_NAME)
	@echo		sdk_config - starting external tool for editing sdk_config.h
	@echo		flash      - flashing binary

TEMPLATE_PATH := $(SDK_ROOT)/components/toolchain/gcc


include $(TEMPLATE_PATH)/Makefile.common

$(foreach target, $(TARGETS), $(call define_target, $(target)))

.PHONY: flash erase

# Flash the program
flash: $(OUTPUT_DIRECTORY)/$(PROJECT_NAME).hex
	@echo Flashing: $<
	nrfjprog -f nrf51 --program $< --sectorerase
	nrfjprog -f nrf51 --reset

erase:
	nrfjprog -f nrf51 --eraseall

SDK_CONFIG_FILE :=  $(PROJ_DIR)/include/sdk_config.h
CMSIS_CONFIG_TOOL := $(SDK_ROOT)/external_tools/cmsisconfig/CMSIS_Configuration_Wizard.jar
sdk_config:
	java -jar $(CMSIS_CONFIG_TOOL) $(SDK_CONFIG_FILE)
