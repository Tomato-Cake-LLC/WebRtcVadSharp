VERSION = 1.4.0

ARMCC = arm-none-eabi-gcc -D_POSIX_THREADS
CFLAGS = -I. -fPIC -DWEBRTC_POSIX -DNDEBUG
LDFLAGS = -shared

TARGET  = libwebrtcvad.so.$(VERSION)
MACOS_TARGET = libwebrtcvad.dylib
SOURCES = $(shell find . -type f -iname '*.c')
HEADERS = $(shell find . -type f -iname '*.h')
OBJECTS = $(SOURCES:.c=.o)

lib: lib-x64

lib-x64: CC=gcc
lib-x64: runtimes/linux-x64/native/$(TARGET)

lib-x86: CC=gcc -m32
lib-x86: runtimes/linux-x86/native/$(TARGET)

lib-arm64: CC=$(ARMCC)
lib-arm64: runtimes/linux-arm64/native/$(TARGET)

lib-macos-arm64: CC=clang -arch arm64
lib-macos-arm64: runtimes/macos-arm64/native/$(MACOS_TARGET)

runtimes/linux-x64/native/$(TARGET): $(OBJECTS)
	mkdir -p runtimes/linux-x64/native
	$(CC) $(LDFLAGS) -o $@ $(OBJECTS)
	
runtimes/linux-x86/native/$(TARGET): $(OBJECTS)
	mkdir -p runtimes/linux-x86/native
	$(CC) $(LDFLAGS) -o $@ $(OBJECTS)

runtimes/linux-arm64/native/$(TARGET): $(OBJECTS)
	mkdir -p runtimes/linux-arm64/native
	$(ARMCC) $(LDFLAGS) -o $@ $(OBJECTS)

runtimes/linux-arm/native/$(TARGET): $(OBJECTS)
	mkdir -p runtimes/linux-arm/native
	$(ARMCC) $(LDFLAGS) -m32 -o $@ $(OBJECTS)

runtimes/macos-arm64/native/$(MACOS_TARGET): $(OBJECTS)
	mkdir -p runtimes/macos-arm64/native
	$(CC) -dynamiclib -o $@ $(OBJECTS)

clean:
	rm -rf $(OBJECTS) $(TARGET) bin obj runtimes
