# Building Extra Software for the DE10-Nano

The Angstrom Linux package manager does not provide some common tools.
My installation notes for a few important tools are described here.

Before anything else, read Glenn K. Lockwood's "Getting started with
the Terasic DE10-Nano" to reconfigure the SD card and user accounts.

## ClamAV

Leaving a Linux server online all the time without protection seems
foolhardy. My board sits behind my router (firewall), and I want
antivirus running just in case.

1. Download the [ClamAV source
code](https://www.clamav.net/downloads).
2. Unpack it (`tar xvzf clamav-x.y.z.tar.gz`), then `cd clamav-x.y.z`.
3. Add the `clamav` user and build the package:
  ```
  $ sudo groupadd clamav
  $ sudo useradd clamav -g clamav
  $ ./configure
  $ make -j2 && sudo make install
  ```
4. Create and customize `freshclam.conf` and `clamd.conf`. On my
   board, ClamAV was installed to the `/usr/local` prefix.
   ```
   $ sudo cp /usr/local/etc/clamd.conf.sample /usr/local/etc/clamd.conf
   $ sudo cp /usr/local/etc/freshclam.conf.sample /usr/local/etc/freshclam.conf
   ```
   - In `/usr/local/etc/clamd.conf`, un-comment the line reading
     `LocalSocket /tmp/clamd.socket`.
   - In both files, remove or comment out the line reading `Example`.
5. Run `freshclam` to update definitions, then run a test scan:
   ```
   $ sudo freshclam
   $ sudo clamscan ~
   ```
6. Angstrom Linux uses `systemd` to manage daemon processes. Create
   `clamav-daemon.service`, `clamav-daemon.socket`, and
   `clamav-freshclam.service` in `/etc/systemd/system`. It's a good
   idea to base these off an existing trio, edited to set the right
   paths. Then start and enable the socket and both services.

## Emacs

Emacs is my preferred editor, but it is not packaged for Angstrom.
So, it must be built from source. For best results, disable X for
the duration of the build, since it takes a lot of RAM to build.

1. Drop to runlevel 3: `sudo init 3`
2. Download the [Emacs source code](http://ftpmirror.gnu.org/emacs/).
3. Unpack it (`tar xvzf emacs-x.y.tar.gz`), then `cd emacs-x.y`.
4. Build the package:
  ```
  $ sudo groupadd clamav
  $ sudo useradd clamav -g clamav
  $ ./configure --with-x=no
  $ make && sudo make install
  ```
  Note that this `make` is not parallelized: mine always hung when
  attempting to build in parallel.
