TOP_SRCDIR = ../../
include $(TOP_SRCDIR)/Makefile.defs

INSTALLHEADERS = wlanif.h wlanifBSteerControl.h wlanifLinkEvents.h \
                 wlanifLinkEventsCmn.h wlanifBSteerControlCmn.h \
                 wlanifBSteerEvents.h driver_defs.h wlanifSocket.h \
                 wlanifCommandParser.h

# These are shared and exported for other daemons to use.
TARGET_LIB = libwlanif.a
TARGET_SOURCES = wlanif.c wlanifPrivate.c wlanifUtils.c wlanifBSteerEvents.c \
                 wlanifLinkEventsCmn.c wlanifBSteerControlCmn.c wlanifSocket.c \
		 wlanifCommandParser.c
TARGET_OBJECTS = $(patsubst %.c,%.$(OBJEXT),$(TARGET_SOURCES))

# These files are not shared/exported to other daemons.
BSA_TARGET_LIB = libwlanif_bsa.a
BSA_TARGET_SOURCES = wlanifBSteerControlBSA.c wlanifLinkEventsBSA.c \
                     wlanifBSteerEventsBSA.c
BSA_TARGET_OBJECTS = $(patsubst %.c,%.$(OBJEXT),$(BSA_TARGET_SOURCES))

all: $(TARGET_LIB) install

$(TARGET_LIB): $(TARGET_OBJECTS)
	@rm -f $(TARGET_LIB)
	$(AR) $(ARFLAGS) $(TARGET_LIB) $(TARGET_OBJECTS)
	$(RANLIB) $(TARGET_LIB)

$(BSA_TARGET_LIB): $(BSA_TARGET_OBJECTS)
	@rm -f $(BSA_TARGET_LIB)
	$(AR) $(ARFLAGS) $(BSA_TARGET_LIB) $(BSA_TARGET_OBJECTS)
	$(RANLIB) $(BSA_TARGET_LIB)

$(TARGET_OBJECTS): CFLAGS+=-fPIC

%.o: %.c
	$(COMPILE) $(CFLAGS) -c $?

install: $(TARGET_LIB) $(BSA_TARGET_LIB)
	cp -a -f $(TARGET_LIB) $(LBLIB_STATIC)/.
	cp -a -f $(BSA_TARGET_LIB) $(LBLIB_STATIC)/.
	for file in $(INSTALLHEADERS) ; do ln -s -f `pwd`/$$file $(LBINCLUDE)/. ; done

clean:
	@rm -f *.o $(TARGET_LIB) *~
	@rm -f *.o $(BSA_TARGET_LIB) *~

.PHONY: all clean

