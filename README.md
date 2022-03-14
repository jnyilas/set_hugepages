# set_hugepages
It slices. It dices. It sets (and helps you properly size) a Linux huge pages memory reservation pool.

##Usage
    set_hugepages_sh [OPTIONS] -a[s SIZE]
    set_hugepages_sh [OPTIONS] -p PCT
      -a  Auto set huge pages reservation size (+5GB by default) above
          currently used huge pages allocation. Can be tuned with -s
      -s  Set huge pages reservation size in units of MB, GB, or Pages.
      -p  Set huge pages reservation size as a percentage of total RAM.
      -c  Commit changes to /etc/systctl.conf and attempt to modify live kernel.
