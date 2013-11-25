CXX = g++
CFLAGS = -mtune=native -Wall -Wextra -std=c++0x -O3 -fomit-frame-pointer

OSVERSION := $(shell uname -s)
LIBS = -lgmp -lgmpxx -lcrypto -lssl -pthread

ifeq ($(OSVERSION),Linux)
	LIBS += -lrt
endif

# You might need to edit these paths too
LIBPATHS = -L/usr/local/lib -L/usr/lib
INCLUDEPATHS = -I/usr/local/include -I/usr/include -IprimecoinMiner/includes/

ifeq ($(OSVERSION),Darwin)
	GOT_MACPORTS := $(shell which port)
ifdef GOT_MACPORTS
	LIBPATHS += -L/opt/local/lib
	INCLUDEPATHS += -I/opt/local/include
endif
endif

JHLIB = primecoinMiner/jhlib/customBuffer.o \
	primecoinMiner/jhlib/fastString_eprintf.o \
	primecoinMiner/jhlib/packetBuffer.o \
	primecoinMiner/jhlib/fastString.o \
	primecoinMiner/jhlib/hashTable_uint32.o \
	primecoinMiner/jhlib/simpleList.o \
	primecoinMiner/jhlib/simpleHTTP.o

OBJS = \
	primecoinMiner/bn2.o \
	primecoinMiner/bn2_div.o \
        primecoinMiner/ticker.o \
	primecoinMiner/jsonBuilder.o \
	primecoinMiner/jsonClient.o \
	primecoinMiner/jsonObject.o \
	primecoinMiner/jsonParser.o \
	primecoinMiner/jsonrpc.o \
	primecoinMiner/prime.o \
	primecoinMiner/main.o \
	primecoinMiner/miner.o \
	primecoinMiner/ripemd160.o \
	primecoinMiner/sha256.o \
	primecoinMiner/xptClient.o \
	primecoinMiner/xptClientPacketHandler.o \
	primecoinMiner/xptPacketbuffer.o \
	primecoinMiner/xptServer.o \
	primecoinMiner/xptServerPacketHandler.o

all: jhprimeminer
  
primecoinMiner/jhlib/%.o: primecoinMiner/jhlib/%.cpp
	$(CXX) -c $(CFLAGS) -I./primecoinMiner/jhlib $< -o $@

primecoinMiner/%.o: primecoinMiner/%.cpp
	$(CXX) -c $(CFLAGS) $(INCLUDEPATHS) $< -o $@ 

jhprimeminer: $(OBJS:primecoinMiner/%=primecoinMiner/%) $(JHLIB:primecoinMiner/jhlib/%=primecoinMiner/jhlib/%)
	$(CXX) $(CFLAGS) $(LIBPATHS) $(INCLUDEPATHS) -o $@ $^ $(LIBS)

clean:
	-rm -f jhprimeminer
	-rm -f primecoinMiner/*.o
	-rm -f primecoinMiner/jhlib/*.o
