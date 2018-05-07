CXX=cl.exe
LINKER=link.exe

PVS_STUDIO="C:\Program Files (x86)\PVS-Studio\x64\PVS-Studio.exe"
PLOG_CONVERTER="C:\Program Files (x86)\PVS-Studio\PlogConverter.exe"
PVS_CFG=.\PVS-Studio.cfg
LOG_FORMAT=Html,Csv,Plog
#
NAME_TEMPLATE=project
PVS_LOG_DIR=.

CFLAGS=/c
OFLAGS=/Ox
INCLUDES=
SOURCES=main.cpp lib.cpp
OBJECTS=$(SOURCES:.cpp=.o)
IOBJECTS=$(SOURCES:.cpp=.o.PVS-Studio.i)
POBJECTS=$(SOURCES:.cpp=.o.PVS-Studio.log)
EXECUTABLE=example-cl.exe

all: $(SOURCES) $(EXECUTABLE)

$(EXECUTABLE): $(OBJECTS)
# Linking
	$(LINKER) $(OBJECTS) /OUT:$(EXECUTABLE)
# Converting
	$(PLOG_CONVERTER) -a "GA:1,2" -t $(LOG_FORMAT) $(POBJECTS) -o $(PVS_LOG_DIR) -n $(NAME_TEMPLATE)

.cpp.o:
# Build
	$(CXX) $(CFLAGS) $(INCLUDES) $(OFLAGS) $< /Fo$@
# Preprocessing
	$(CXX) $(CFLAGS) $< $(INCLUDES) /P /Fi$@.PVS-Studio.i
# Analysis
	$(PVS_STUDIO) --cfg $(PVS_CFG) --source-file $< --i-file $@.PVS-Studio.i --output-file $@.PVS-Studio.log

clean:
	del $(OBJECTS) $(IOBJECTS) $(POBJECTS) $(EXECUTABLE) $(NAME_TEMPLATE).????
