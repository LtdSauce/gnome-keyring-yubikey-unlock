# Modified by LtdSauce to:
# - generate compilation database using bear
# - compile with sanitizers
CXX ?= g++
CXXFLAGS := -I ./lib -I . -std=c++17 -DKEYRING_IMPL_$(KEYRING_IMPL)

ifeq ($(KEYRING_IMPL),lib)
    CXXFLAGS += $(shell pkg-config --cflags --libs gnome-keyring-1)
else ifneq ($(KEYRING_IMPL),standalone)
$(error "KEYRING_IMPL must be set to 'lib' or 'standalone'. Example: 'make KEYRING_IMPL=standalone'")
endif

BUILD_COMMAND := $(CXX) unlock_keyrings.cc -o ../bin/unlock_keyrings $(CXXFLAGS)
secret: bin-dir
	$(BUILD_COMMAND)
bin-dir: ../bin/
	mkdir -p ../bin/
compile-database:
	bear -- $(BUILD_COMMAND)
debug: bin-dir
	$(BUILD_COMMAND) -fsanitize=address,leak,undefined
