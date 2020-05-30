# DroidCam & DroidCamX (C) 2010
# https://github.com/aramg
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
# Use at your own risk. See README file for more details.

CC    = -no-pie
GTK   = `pkg-config --libs --cflags gtk+-3.0 glib-2.0` `pkg-config --libs x11`
LIBAV = `pkg-config --libs --cflags libswscale libavutil`
LIBS  =  -lspeex -lasound -lpthread -lm
JPEG  = `pkg-config --libs --cflags libturbojpeg`
SRC      = src/connection.c src/settings.c src/decoder*.c src/av.c src/adb.c

all: gresource droidcam-cli droidcam

ifeq "$(RELEASE)" "1"
LIBAV = /usr/lib/x86_64-linux-gnu/libswscale.a /usr/lib/x86_64-linux-gnu/libavutil.a
package: clean all
	@rm droidam_*.zip || true
	zip -x *.png src/* -r droidcam_`date +%s`.zip ./*
endif

gresource: .gresource.xml icon.png
	glib-compile-resources .gresource.xml --generate-source --target=src/resources.c

droidcam-cli: src/droidcam-cli.c $(SRC)
	gcc -Wall $(CC) $^ $(JPEG) $(LIBAV) $(LIBS) -o droidcam-cli

droidcam: src/droidcam.c src/resources.c $(SRC)
	gcc -Wall $(CC) $^ $(GTK) $(JPEG) $(LIBAV) $(LIBS) -o droidcam

clean:
	rm droidcam || true
	rm droidcam-cli || true
	make -C v4l2loopback clean
