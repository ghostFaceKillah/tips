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
tf.get_collection(tf.GraphKeys.VARIABLES)
```
