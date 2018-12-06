# Controlling the FPGA from the HPS

YouTube user Bo Gao posted a useful series of tutorials for the DE10-Standard.
The tutorial was written for the Linux Console image, while I'm using Xfce.
This may be worth recording a video update (Xfce & DE10-Nano).

## 3. First HPS Project <https://youtu.be/D-Z2vLL5zT0>

### Manipulating FPGA LEDs

After a reboot, the HPS does not have access to the FPGA pins (i.e., LEDs).
Check /sys/class/leds to be sure. Then, to assume control, the DTBO file must
be loaded using a configfs.

1. Create the configfs:
   ```
   # mkdir /config
   # mount -t configfs configfs /mount
   # cd /config/devicetree/overlays
   # mkdir new-overlay
   ```
2. Find the appropriate DTBO in /lib/firmware. On my dev kit, it resides
   at `/lib/firmware/de10-nano.dtbo`:
   ```
   # file /lib/firmware/de10-nano.dtbo
   /lib/firmware/de10-nano.dtbo: Device Tree Blob version 17, size=13165, boot CPU=0,
                                 string block size=1033, DT structure block size=12076
   ```
3. There must be an identically-named RBF (raw binary file). On my Xfce image,
   this was stored under `/media/FAT` alongside several other useful-looking files.
   ```
   # file /media/FAT/de10-nano.rbf
   /lib/firmware/de10-nano.rbf: data
   # rsync -a /media/FAT/de10-nano.rbf /lib/firmware/
   ```
4. Load the FPGA overlay into the new overlay using the name of the device tree blob:
   ```
   # echo de10-nano.dtbo > new-overlay/path
   ```

If all went well, meaning that `de10-nano.{rbf,dtbo}` were recognized, there should
be a "heartbeat" blinking on the board, and some new LEDs will be available:
```
# cd /sys/class/leds
# ls
fpga_led0  fpga_led2  fpga_led4  fpga_led6  hps_led0
fpga_led1  fpga_led3  fpga_led5  fpga_led7
```

Now, the LEDs can be controlled through their `brightness` properties.
To turn the first LED on and off, execute
```
# echo 1 > fpga_led0/brightness
# echo 0 > fpga_led0/brightness
```

To turn all the LEDs on and off:
```
# for i in {0..7}; do echo 1 > fpga_led${i}/brightness; sleep 1; done
# for i in {7..0}; do echo 0 > fpga_led${i}/brightness; sleep 1; done
```

### HPS Code: Hello, World!

Let's write and execute a C program on the HPS.

```
$ nano hello.c
$ cat hello.c
#include <stdio.h>
int main()
{
    printf("Hello from DE10, World!\n");
    return 0;
}
```

```
$ nano Makefile
$ cat Makefile
hello: hello.c
	gcc -O2 -Wall -Dsoc_cv_av $< -o $@
```

```
$ make
$ ./hello
gcc -O2 -Wall -Dsoc_cv_av hello.c -o hello
Hello from DE10, World!
```

### Qsys Code: Hello, World!
