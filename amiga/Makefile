#CC=i386-pc-linux-gcc
CC=m68k-amigaos-gcc
AS=m68k-amigaos-as

CFLAGS= -Os -I . -I .. -nostdlib -I ../fat32 -DFATFS_IS_LITTLE_ENDIAN=0 -m68000 -Wall -msoft-float -fno-builtin -fomit-frame-pointer #-DDEBUG
LDFLAGS=-s  -Wl,-Map,foo.map -nostdlib -m68000

EXEC=HXCFEMNG


all: AUTOBOOT.ADF
	#./upx  -9 $(EXEC)
	#./Shrinkler $(EXEC) $(EXEC).PACK
	#mv $(EXEC).PACK "H:\SDHxCFloppySelector.amigados\$(EXEC)"

AUTOBOOT.ADF: HXCFEMNG bootblock/bootblock.c bootblock/ncrt0.S bootblock/make_bootblock.c
	# Generate ADF with the new exe, Build the bootblock, and patch the adf with the new bootblock
	cp $(EXEC) ./HxCFESelector.amigados/$(EXEC)
	hxcfe  -conv:AMIGA_ADF -finput:HxCFESelector.amigados -foutput:$@
	cd bootblock && $(MAKE) $@
	hxcfe  -conv:HXC_HFE -finput:$@ -foutput:AUTOBOOT.HFE

HXCFEMNG: libc/ncrt0.o libc/amiga_api.o libc/mStartupAsmC.o libc/mStartup.o libc/snprintf/snprintf.o libc/ncrt0.o fectrl.o gui_utils.o amiga.o crc.o fat_access.o fat_filelib.o fat_misc.o fat_string.o fat_table.o fat_write.o fat_cache.o reboot.o msg_txt.o
	$(CC) -o $@    $^ $(LDFLAGS)
	m68k-amigaos-strip -s $@

reboot.o: reboot.S
	$(AS) -o $@ $<

fectrl.o: ../fectrl.c
	$(CC) -o $@ -c $< $(CFLAGS)

gui_utils.o: ../gui_utils.c
	$(CC) -o $@ -c $< $(CFLAGS)

msg_txt.o: ../msg_txt.c
	$(CC) -o $@ -c $< $(CFLAGS)

amiga.o: amiga.c
	$(CC) -o $@ -c $< $(CFLAGS)

crc.o: crc.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_access.o: ../fat32/fat_access.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_filelib.o: ../fat32/fat_filelib.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_misc.o: ../fat32/fat_misc.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_string.o: ../fat32/fat_string.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_table.o: ../fat32/fat_table.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_write.o: ../fat32/fat_write.c
	$(CC) -o $@ -c $< $(CFLAGS)

fat_cache.o: ../fat32/fat_cache.c
	$(CC) -o $@ -c $< $(CFLAGS)

libc/mStartup.o: libc/mStartup.c
	$(CC) $(CFLAGS) -c $< -o $@

libc/snprintf/snprintf.o: libc/snprintf/snprintf.c libc/snprintf/snprintf.h
	$(CC) $(CFLAGS) -c $< -o $@

libc/mStartupAsmC.o: libc/mStartupAsmC.s
	$(AS) -o $@ $< -m68000 -pcrel

libc/amiga_api.o: libc/amiga_api.S
	$(AS) -o $@ $< -m68000 -pcrel

libc/ncrt0.o: libc/ncrt0.S
	$(AS) -o $@ $< -m68000 -pcrel

clean:
	rm -rf *.o *.bin *.map *.ADF *.HFE make_bootblock.exe
	cd bootblock && $(MAKE) clean

mrproper: clean
	rm -rf $(EXEC)

.PHONY: clean mrproper
