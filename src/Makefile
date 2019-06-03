prefix = /usr
libdir = $(prefix)/lib/x86_64-linux-gnu

CFLAGS 		= -std=c99 -pedantic -Wall -g -Wextra -Wno-unused-parameter -Wno-unused-function -Wno-overlength-strings
INCLUDES 	= -I$(prefix)/include
LIBS 		= $(libdir)/libssl.a $(libdir)/libcrypto.a -pthread -lm -ldl

#CFLAGS		+= -fsanitize=address -fno-omit-frame-pointer -g
#CFLAGS		+= -fsanitize=memory -fno-omit-frame-pointer -g -fsanitize-memory-track-origins

all		: udp
udp		: dtls_udp_chargen dtls_udp_discard dtls_udp_echo

dtls_udp_chargen: dtls_udp_chargen.c
	$(CC) $(CFLAGS) $(INCLUDES) -o dtls_udp_chargen dtls_udp_chargen.c $(LFLAGS) $(LIBS)

dtls_udp_discard: dtls_udp_discard.c
	$(CC) $(CFLAGS) $(INCLUDES) -o dtls_udp_discard dtls_udp_discard.c $(LFLAGS) $(LIBS)

dtls_udp_echo: dtls_udp_echo.c
	$(CC) $(CFLAGS) $(INCLUDES) -o dtls_udp_echo dtls_udp_echo.c $(LFLAGS) $(LIBS)

clean:
	rm -f dtls_udp_chargen
	rm -f dtls_udp_discard
	rm -f dtls_udp_echo
