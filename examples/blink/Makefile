TARGET:=blink

all : flash

PREFIX:=riscv64-unknown-elf

GPIO_Toggle:=EXAM/GPIO/GPIO_Toggle/User

EVT:=../../ch32v003evt
MINICHLINK:=../../minichlink

CFLAGS:= \
	-g -Os -flto -ffunction-sections \
	-static-libgcc -lgcc \
	-march=rv32ec \
	-mabi=ilp32e \
	-I/usr/include/newlib \
	-I$(EVT) \
	-nostdlib \
	-I.

LDFLAGS:=-T $(EVT)/ch32v003.ld -Wl,--gc-sections

SYSTEM_C:=$(EVT)/startup_ch32v003.c $(EVT)/embedlibc.c

$(TARGET).elf : $(SYSTEM_C) $(TARGET).c
	$(PREFIX)-gcc -o $@ $^ $(CFLAGS) $(LDFLAGS)

$(TARGET).bin : $(TARGET).elf
	$(PREFIX)-size $^
	$(PREFIX)-objdump -S $^ > $(TARGET).lst
	$(PREFIX)-objdump -t $^ > $(TARGET).map
	$(PREFIX)-objcopy -O binary $< $(TARGET).bin
	$(PREFIX)-objcopy -O ihex $< $(TARGET).hex

flash : $(TARGET).bin
	make -C $(MINICHLINK) all
	$(MINICHLINK)/minichlink -w $< -r

clean :
	rm -rf $(TARGET).elf $(TARGET).bin $(TARGET).hex $(TARGET).lst $(TARGET).map $(TARGET).hex

