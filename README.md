# set_hugepages
It slices. It dices. It sets (and helps you properly size) a Linux huge pages memory reservation pool.

## Usage
    set_hugepages_sh {a[s]|s|p} ARG [cCqv]
    set_hugepages_sh [-c|C|L] -p PCT
    set_hugepages_sh [-c|C|L] -s SIZE
    set_hugepages_sh [-c|C|L] -as SIZE
       -a  Auto set the huge pages reservation size (+5GB by default) above
           the currently used huge pages allocation. Can be tuned with -s

       -s  Set huge pages reservation size in units of (M)B, (G)B, or (P)ages.
           If used without -a, it sets an absolute huge pages reservation size.

       -p  Set huge pages reservation size as a percentage of total RAM.
           Expressed as a decimal value. E.G., argument .50 indicates 50 percent of RAM.

       -c  Commit changes to /etc/systctl.conf only.

       -C  Commit changes to /etc/systctl.conf and attempt to modify live kernel with 'sysctl(8)'.

       -L  Just  attempt to modify live kernel with 'sysctl(8)' without making the change persistent.
       
       -q  Be quieter.
        
       -v  Explore additional modeling around the proposed changes.
## Reporting
With no options, it defaults to modeling a 50% RAM huge pages reservation scenario:

    # ./set_hugepages_sh 
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on 50.000% of RAM proposed reservation:
        Proposed Huge Pages: 37663 (73.6 GB)
 
 ### Modeling with AutoSizing       
Let's auto size the huge pages pool to have +5GB free (default autosize):
    
    # ./set_hugepages_sh -a
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on automatically sizing (currently_used_huge_pages + 5G):
        Proposed Huge Pages: 21891  (42.8 GB)
        Proposed is equivalent to 29% RAM

Let's make it +7GB free instead:
    
    # ./set_hugepages_sh -a -s 7G
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on automatically sizing (currently_used_huge_pages + 7G):
        Proposed Huge Pages: 22915  (44.8 GB)
        Proposed is equivalent to 30% RAM
        
Another idea -- lets autosize it to have +2000 huge pages free:
    
    # ./set_hugepages_sh -a -s 2000P
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on automatically sizing (currently_used_huge_pages + 2000P):
        Proposed Huge Pages: 21331  (41.7 GB)
        Proposed is equivalent to 28% RAM

### Modeling with absolute sizing
Another plan, let's see how this looks with a 37% of RAM huge pages reservation:
    
    # ./set_hugepages_sh  -p .37
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on 37.000% of RAM proposed reservation:
        Proposed Huge Pages: 27871 (54.4 GB)
        
Hmmm, let's just model a 50G huge pages reservation and call it good:
    
    # ./set_hugepages_sh  -s 50G
    Setting absolute size
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on 33.985% of RAM proposed reservation:
        Proposed Huge Pages: 25600 (50.0 GB)
        
## Committing Changes        
Let's save this so it will be implemented on the next node reboot:
    
    # ./set_hugepages_sh  -s 50G -c
    Setting absolute size
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on 33.985% of RAM proposed reservation:
        Proposed Huge Pages: 25600 (50.0 GB)
    Committing configuration only to /etc/sysctl.conf ...
    
Oh wait! Let's also have 'sysctl' try and set it for us live and modify the running kernel without a reboot.  
When **growing** the huge pages reservation -- _Warning, this may run very slowly or not finish at all, depending on how fragmented and how many memory pages are truly "free"._  
However, when **shrinking** the huge pages reservation, this is not a problem, and the freeing of memory is a very quick operation.
    
    # ./set_hugepages_sh  -s 50G -C
    Setting absolute size
    Huge Page size is: 2048 kB
    Total Memory pages: 75326  (147.1 GB)

    Current Settings:
        Number of Huge Pages Reserved: 21408  (41.8 GB)
        Percent of RAM Huge Pages Reserved: 28%
        Number of Huge Pages Free: 2077  (4.1 GB)
        Number of Huge Pages *Actually* Used: 19331  (37.8 GB)
        Percent of RAM Huge Pages *Actually* Used: 26%
        Percent of Reserved Huge Pages *Actually* Used: 90%

    Proposed Settings:
        Based on 33.985% of RAM proposed reservation:
        Proposed Huge Pages: 25600 (50.0 GB)
    Committing configuration to /etc/sysctl.conf and modifying live kernel ...

        
