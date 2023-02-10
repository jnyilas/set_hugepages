# set_hugepages
It slices. It dices. It sets (and helps you properly size) a Linux huge pages memory reservation pool.

## Usage
    set_hugepages_sh {a[s]|s|p} ARG [cC]
    set_hugepages_sh [-c|C] -p PCT
    set_hugepages_sh [-c|C] -s SIZE
    set_hugepages_sh [-c|C] -as SIZE
       -a  Auto set the huge pages reservation size (+5GB by default) above
           the currently used huge pages allocation. Can be tuned with -s

       -s  Set huge pages reservation size in units of (M)B, (G)B, or (P)ages.
           If used without -a, it sets an absolute huge pages reservation size.

       -p  Set huge pages reservation size as a percentage of total RAM.
           Expressed as a decimal value. E.G., argument .50 indicates 50 percent of RAM.

       -c  Commit changes to /etc/systctl.conf only.

       -C  Commit changes to /etc/systctl.conf and attempt to modify live kernel with 'sysctl(8)'.
