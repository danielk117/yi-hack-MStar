OBJECTS_1 = imggrabber.o add_water.o water_mark.o
OBJECTS = $(OBJECTS_1)
JPEGLIB = jpeg-9c
JPEGLIB_DIR = ./$(JPEGLIB)
INC_J = -I$(JPEGLIB_DIR)
LIB_J = $(JPEGLIB_DIR)/.libs/libjpeg.a

all: imggrabber

imggrabber.o: imggrabber.c $(HEADERS)
	@$(build_jpeglib)
	$(CC) -c $< $(INC_J) -fPIC -O2 -o $@

add_water.o: add_water.c $(HEADERS)
	$(CC) -c $< $(INC_J) -fPIC -O2 -o $@

water_mark.o: water_mark.c $(HEADERS)
	$(CC) -c $< $(INC_J) -fPIC -O2 -o $@

imggrabber: $(OBJECTS_1)
	$(CC) $(OBJECTS_1) $(LIB_J) -fPIC -O2 -o $@
	$(STRIP) $@

.PHONY: clean

clean:
	rm -f imggrabber
	rm -f $(OBJECTS)

distclean: clean
	rm -rf SDK
	rm -rf jpeg-9c

define build_jpeglib

    # get archive
    if [ ! -f SDK/jpeg.tar.gz ]; then \
        mkdir -p SDK; \
        wget -O ./SDK/jpeg.tar.gz.tmp "https://www.ijg.org/files/jpegsrc.v9c.tar.gz"; \
        mv ./SDK/jpeg.tar.gz.tmp ./SDK/jpeg.tar.gz; \
    fi

    # untar
    if [ ! -f $(JPEGLIB)/README ]; then \
         tar zxvf ./SDK/jpeg.tar.gz; \
    fi

   # build
    if [ ! -f $(JPEGLIB)/.libs/libjpeg.a ]; then \
         cd $(JPEGLIB); \
        ./configure --host=$(CROSS) --enable-shared=yes && \
         make; \
         cd ..;\
    fi
endef
