# Mike's tips
Various stuff I forget, sometimes.


### Turning off ASLR on Linux
Taken from [StackOverflow](http://askubuntu.com/questions/318315/how-can-i-temporarily-disable-aslr-address-space-layout-randomization).

We can see the ASLR status in the file
```
/proc/sys/kernel/randomize_va_space
```

The following values are supported:

```
0 – No randomization. Everything is static.
1 – Conservative randomization. Shared libraries, stack, mmap(), VDSO and heap are randomized.
2 – Full randomization. In addition to elements listed in the previous point,
    memory managed through brk() is also randomized.
```

So, to disable it:

```
echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
```

and to enable it again, run

```
echo 2 | sudo tee /proc/sys/kernel/randomize_va_space
```

This won't survive a reboot, so you'll have to configure this in sysctl.
Add a file /etc/sysctl.d/01-disable-aslr.conf containing:

```
kernel.randomize_va_space = 0
```
should permanently disable this.


### Various tips for diffing two files

Diffing files in two columns
```
diff -y -W 160 dqn.py others/VashishtMadhavan/dqn.py |less
```
where `-y` means line by line.
and `-W 160` means width (in chars of the output).

Good visual-style vim-related diff
```
vimdiff dqn.py others/dqn.py
```

### How to print all variables in a tensorflow graph

```python
tf.get_collection(tf.GraphKeys.GLOBAL_VARIABLES)
```


### How to run tensorflow computation only on one GPU

It is done by setting an environment variable `CUDA_VISIBLE_DEVICES` to a comma separated list of wanted GPUs.


```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = "1"
```

### How to run exploit so you can still type things into shell

if we finish the first process before the pipe, we won't be able to pass input to the second!

```bash
(python exploit.py; cat) | /opt/protostar/bin/stack5 
```


### How to screenshot part of screen in Ubuntu 16

```
gnome-screenshot -a
```


### How to quickly kill a process using port 8000 in Linux?

```
fuser -k 8000/udp
```




### Hot to stop displaymanager

`systemctl isolate multi-user.target`. # disable the graphical target
`sudo service lightdm stop`   # manual stop of window manager often still needed ¯\_(ツ)_/¯ 
`sudo service gdm3 stop`

`modprobe -r nvidia-drm`     #  Unload the drivers

`rmmod` also unloads the drivers

`systemctl start graphical.target`  restarts the graphical target


### Various other tricks for installing NVIDIA CUDA etc

`dpkg -l | grep nvidia` search 

```
sudo apt-get purge nvidia*
sudo apt-get autoremove
sudo sh cuda-*.run --silient --override
```

```
Driver:   Installed
Toolkit:  Installed in /usr/local/cuda-10.2/
Samples:  Installed in /home/mike/, but missing recommended libraries

Please make sure that
 -   PATH includes /usr/local/cuda-10.2/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-10.2/lib64, or, add /usr/local/cuda-10.2/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-10.2/bin
To uninstall the NVIDIA Driver, run nvidia-uninstall
```

### How to setup the standard python env
pipenv --python=python3.7


### Check what ubuntu / debian you have

`lsb_release -a`

### check what stuff you have connected HW-wise

```
lscpu. The lscpu command reports information about the cpu and processing units. ...
lshw - List Hardware. ...
hwinfo - Hardware Information. ...
lspci - List PCI. ...
lsscsi - List scsi devices. ...
lsusb - List usb buses and device details. ...
Inxi. ...
lsblk - List block devices
```


### What are the various keybindings that work both in Emacs and by default in Mac OS?

| Shortcut | Wat it does  |                                                
| ---------|------------- |                                                
| C-a      |  go to beginning of the line                                 | 
| C-e      |  go to end of the line                                       | 
| C-l      |  redisplay                                                   | 
| C-k      | Erase from cursor to end of line                             | 
| C-d      | Erase next character                                         | 
| C-h      | Erase last character                                         | 
| C-w      | Erase previous word                                          | 
| C-p      | Show previous command                                        | 
| C-n      | Show next command                                            | 
| C-b      | Move back one char                                           | 
| C-f      | Move forward one char                                        | 
| M-b      | Move back one word                                           | 
| M-f      | Move forward one word                                        | 
| C-j      | Execute command                                              | 
| C-t      | Transpose last two chars                                     | 
| M-t      |  transpose two last words                                    | 
| `<Del>`  | Delete the character just before the cursor                  | 
| C-h      | #SHELL ONLY#  Delete the character just before the cursor    | 
| C-d      | Delete the next character after the cursor                   | 
| M-`<Del>`| Kill the word immediately before the cursor                  | 
| C-w      | #SHELL ONLY# Kill the word immediately before the cursor     | 
| C-d      |  delete next character                                       | 
| M-d      | Kill the next word after the cursor                          | 
| C-k      | Kill from the cursor position to end of line                 | 
| M-k      | Kill to the end of the current sentence                      | 

| emacs    | Wat does it does xD                               |
| ---------|-------------|              
| C-x C-f  | Find a file                                       |
| C-g      | Cancel whatever you trying to do                  |
| C-x C-s  | Save a file                                       |
| C-x C-b  | List buffers                                      |
| C-x C-f  | Find file                                         |
| C-x C-s  | Save file                                         |
| C-x s    | Save some buffers                                 |
| C-x C-b  | List buffers                                      |
| C-x b    | Switch buffer                                     |
| C-x C-c  | Quit Emacs                                        |
| C-x 1    | Delete all but one window                         |
| C-x u    | Undo                                              |
| C-x 1    | kill all screens but this one                     |
| C-x 2    | make 2 windows                                    |
| C-x 3    | make 2 windows vert                               |
| C-x 0    | switch to the other window                        |
| C-s      | search                                            |


### Rarer vim trix

| Vim         |                           |
|-------------|---------------------------|
| C-e         | move screen up one line   |
| C-y         | move screen up one line   |
| C-u         | move screen up half page  |
| C-d         | move screen up half page  |
| C-b         | move screen up one page   |
| C-f         | move screen up one page   |
|`:%!xxd`     | to switch into hex mode   |
|`:%!xxd` -r  | switch back from hex mode |



### Compression, tar, etc

|general   |                  Wat it does |
| ---------|-------------|              
| tar cvf arch.tar file1 file2 ....              | creates an arch            |
| tar xvf arch.tar                               | extract an arch            |
| tar tvf arch.tar                               | table contents of an arch  |
| find /dir/to/search -name whatfile.txt -print | self-explainatory           |
| grep search-dis-pattern where (-A -B -C 10)    | grep for a pattern         |
| du -sh * \| gsort -h                           | display disk usage sorted |
| zip -r archive.zip *            | packup dir to zip |
