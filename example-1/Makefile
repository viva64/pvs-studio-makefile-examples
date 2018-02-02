CXX=g++

PVS_CFG=./PVS-Studio.cfg
# csv, errorfile, fullhtml, html, tasklist, xml
LOG_FORMAT=tasklist
PVS_LOG=./project.tasks

CFLAGS=-c
OFLAGS=-O3
LDFLAGS=
DFLAGS=
INCLUDES=
SOURCES=main.cpp lib.cpp
OBJECTS=$(SOURCES:.cpp=.o)
IOBJECTS=$(SOURCES:.cpp=.o.PVS-Studio.i)
POBJECTS=$(SOURCES:.cpp=.o.PVS-Studio.log)
EXECUTABLE=example-1

all: $(SOURCES) $(EXECUTABLE)

$(EXECUTABLE): $(OBJECTS)
# Linking
	$(CXX) $(LDFLAGS) $(OBJECTS) -o $@
# Converting
	plog-converter -a 'GA:1,2' -t $(LOG_FORMAT) $(POBJECTS) -o $(PVS_LOG)

.cpp.o:
# Build
	$(CXX) $(CFLAGS) $< $(DFLAGS) $(INCLUDES) $(OFLAGS) -o $@
# Preprocessing
	$(CXX) $(CFLAGS) $< $(DFLAGS) $(INCLUDES) -E -o $@.PVS-Studio.i
# Analysis
	pvs-studio --cfg $(PVS_CFG) --source-file $< --i-file $@.PVS-Studio.i --output-file $@.PVS-Studio.log

clean:
	rm -f $(OBJECTS) $(IOBJECTS) $(POBJECTS) $(EXECUTABLE) $(PVS_LOG)
