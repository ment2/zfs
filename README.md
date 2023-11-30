```bash
[vagrant@zfs ~]$ sudo -i
[root@zfs ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  512M  0 disk 
sdb      8:16   0  512M  0 disk 
sdc      8:32   0  512M  0 disk 
sdd      8:48   0  512M  0 disk 
sde      8:64   0  512M  0 disk 
sdf      8:80   0  512M  0 disk 
sdg      8:96   0  512M  0 disk 
sdh      8:112  0  512M  0 disk 
sdi      8:128  0   40G  0 disk 
`-sdi1   8:129  0   40G  0 part /
[root@zfs ~]# zpool create otus1 mirror /dev/sdb /dev/sdc
[root@zfs ~]# zpool create otus1 mirror /dev/sdb /dev/sde
/dev/sdb is in use and contains a unknown filesystem.
[root@zfs ~]# zpool create otus1 mirror /dev/sdb /dev/sde
/dev/sdb is in use and contains a unknown filesystem.
[root@zfs ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  512M  0 disk 
sdb      8:16   0  512M  0 disk 
|-sdb1   8:17   0  502M  0 part 
`-sdb9   8:25   0    8M  0 part 
sdc      8:32   0  512M  0 disk 
|-sdc1   8:33   0  502M  0 part 
`-sdc9   8:41   0    8M  0 part 
sdd      8:48   0  512M  0 disk 
sde      8:64   0  512M  0 disk 
sdf      8:80   0  512M  0 disk 
sdg      8:96   0  512M  0 disk 
sdh      8:112  0  512M  0 disk 
sdi      8:128  0   40G  0 disk 
`-sdi1   8:129  0   40G  0 part /
[root@zfs ~]# zpool create otus1 mirror /dev/sdb /dev/sde
/dev/sdb is in use and contains a unknown filesystem.
[root@zfs ~]# zpool create otus1 mirror /dev/sdd /dev/sde
cannot create 'otus1': pool already exists
[root@zfs ~]# zpool create otus2 mirror /dev/sdd /dev/sde
[root@zfs ~]# zpool create otus3 mirror /dev/sdf /dev/sdg
[root@zfs ~]# zpool create otus4 mirror /dev/sdh /dev/sdi
/dev/sdi is in use and contains a unknown filesystem.
[root@zfs ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  512M  0 disk 
sdb      8:16   0  512M  0 disk 
|-sdb1   8:17   0  502M  0 part 
`-sdb9   8:25   0    8M  0 part 
sdc      8:32   0  512M  0 disk 
|-sdc1   8:33   0  502M  0 part 
`-sdc9   8:41   0    8M  0 part 
sdd      8:48   0  512M  0 disk 
|-sdd1   8:49   0  502M  0 part 
`-sdd9   8:57   0    8M  0 part 
sde      8:64   0  512M  0 disk 
|-sde1   8:65   0  502M  0 part 
`-sde9   8:73   0    8M  0 part 
sdf      8:80   0  512M  0 disk 
|-sdf1   8:81   0  502M  0 part 
`-sdf9   8:89   0    8M  0 part 
sdg      8:96   0  512M  0 disk 
|-sdg1   8:97   0  502M  0 part 
`-sdg9   8:105  0    8M  0 part 
sdh      8:112  0  512M  0 disk 
sdi      8:128  0   40G  0 disk 
`-sdi1   8:129  0   40G  0 part /
[root@zfs ~]# zpool create otus4 mirror /dev/sda /dev/sdh
[root@zfs ~]# zpool list
NAME    SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
otus1   480M   100K   480M        -         -     0%     0%  1.00x    ONLINE  -
otus2   480M  91.5K   480M        -         -     0%     0%  1.00x    ONLINE  -
otus3   480M   106K   480M        -         -     0%     0%  1.00x    ONLINE  -
otus4   480M  91.5K   480M        -         -     0%     0%  1.00x    ONLINE  -
[root@zfs ~]# zfs set compression=lzjb otus1
[root@zfs ~]# zfs set compression=lz4 otus2
[root@zfs ~]# zfs set compression=gzip-9 otus3
[root@zfs ~]# zfs set compression=zle otus4
[root@zfs ~]# grep compression
^C
[root@zfs ~]# zfs get all | grep compression
otus1  compression           lzjb                   local
otus2  compression           lz4                    local
otus3  compression           gzip-9                 local
otus4  compression           zle                    local
[root@zfs ~]# for i in {1..4}; do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
--2023-11-30 14:55:09--  https://gutenberg.org/cache/epub/2600/pg2600.converter.log
Resolving gutenberg.org (gutenberg.org)... 152.19.134.47, 2610:28:3090:3000:0:bad:cafe:47
Connecting to gutenberg.org (gutenberg.org)|152.19.134.47|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 40988863 (39M) [text/plain]
Saving to: '/otus1/pg2600.converter.log'

100%[============================================================================================================>] 40,988,863  5.53MB/s   in 9.1s   

2023-11-30 14:55:20 (4.31 MB/s) - '/otus1/pg2600.converter.log' saved [40988863/40988863]

--2023-11-30 14:55:20--  https://gutenberg.org/cache/epub/2600/pg2600.converter.log
Resolving gutenberg.org (gutenberg.org)... 152.19.134.47, 2610:28:3090:3000:0:bad:cafe:47
Connecting to gutenberg.org (gutenberg.org)|152.19.134.47|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 40988863 (39M) [text/plain]
Saving to: '/otus2/pg2600.converter.log'

100%[============================================================================================================>] 40,988,863   335KB/s   in 1m 57s 

2023-11-30 14:57:19 (342 KB/s) - '/otus2/pg2600.converter.log' saved [40988863/40988863]

--2023-11-30 14:57:19--  https://gutenberg.org/cache/epub/2600/pg2600.converter.log
Resolving gutenberg.org (gutenberg.org)... 152.19.134.47, 2610:28:3090:3000:0:bad:cafe:47
Connecting to gutenberg.org (gutenberg.org)|152.19.134.47|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 40988863 (39M) [text/plain]
Saving to: '/otus3/pg2600.converter.log'

100%[============================================================================================================>] 40,988,863   393KB/s   in 96s    

2023-11-30 14:58:57 (417 KB/s) - '/otus3/pg2600.converter.log' saved [40988863/40988863]

--2023-11-30 14:58:57--  https://gutenberg.org/cache/epub/2600/pg2600.converter.log
Resolving gutenberg.org (gutenberg.org)... 152.19.134.47, 2610:28:3090:3000:0:bad:cafe:47
Connecting to gutenberg.org (gutenberg.org)|152.19.134.47|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 40988863 (39M) [text/plain]
Saving to: '/otus4/pg2600.converter.log'

100%[============================================================================================================>] 40,988,863  3.43MB/s   in 13s    

2023-11-30 14:59:11 (3.07 MB/s) - '/otus4/pg2600.converter.log' saved [40988863/40988863]

[root@zfs ~]# ls -l /otus*
/otus1:
total 22061
-rw-r--r--. 1 root root 40988863 Nov  2 08:10 pg2600.converter.log

/otus2:
total 17992
-rw-r--r--. 1 root root 40988863 Nov  2 08:10 pg2600.converter.log

/otus3:
total 10958
-rw-r--r--. 1 root root 40988863 Nov  2 08:10 pg2600.converter.log

/otus4:
total 40057
-rw-r--r--. 1 root root 40988863 Nov  2 08:10 pg2600.converter.log
[root@zfs ~]# zfs list
NAME    USED  AVAIL     REFER  MOUNTPOINT
otus1  21.6M   330M     21.6M  /otus1
otus2  17.7M   334M     17.6M  /otus2
otus3  10.8M   341M     10.7M  /otus3
otus4  39.2M   313M     39.1M  /otus4
[root@zfs ~]# wget -O archive.tar.gz --no-check-certificate 'https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download'
--2023-11-30 15:01:32--  https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download
Resolving drive.google.com (drive.google.com)... 142.250.185.110, 2a00:1450:4001:80b::200e
Connecting to drive.google.com (drive.google.com)|142.250.185.110|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://drive.google.com/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download [following]
--2023-11-30 15:01:33--  https://drive.google.com/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download
Reusing existing connection to drive.google.com:443.
HTTP request sent, awaiting response... 303 See Other
Location: https://doc-0c-bo-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/si7s668gvt0ace08h27bcuj4685k37nq/1701356475000/16189157874053420687/*/1KRBNW33QWqbvbVHa3hLJivOAt60yukkg?e=download&uuid=bc74e3e8-a657-49ab-8d4e-6bf22ccbb57c [following]
Warning: wildcards not supported in HTTP.
--2023-11-30 15:01:39--  https://doc-0c-bo-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/si7s668gvt0ace08h27bcuj4685k37nq/1701356475000/16189157874053420687/*/1KRBNW33QWqbvbVHa3hLJivOAt60yukkg?e=download&uuid=bc74e3e8-a657-49ab-8d4e-6bf22ccbb57c
Resolving doc-0c-bo-docs.googleusercontent.com (doc-0c-bo-docs.googleusercontent.com)... 142.250.185.97, 2a00:1450:4001:80f::2001
Connecting to doc-0c-bo-docs.googleusercontent.com (doc-0c-bo-docs.googleusercontent.com)|142.250.185.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7275140 (6.9M) [application/x-gzip]
Saving to: 'archive.tar.gz'

100%[============================================================================================================>] 7,275,140   4.76MB/s   in 1.5s   

2023-11-30 15:01:41 (4.76 MB/s) - 'archive.tar.gz' saved [7275140/7275140]

[root@zfs ~]# ls
anaconda-ks.cfg  archive.tar.gz  original-ks.cfg
[root@zfs ~]# tar -xzvf archive.tar.gz
zpoolexport/
zpoolexport/filea
zpoolexport/fileb
[root@zfs ~]# ls
anaconda-ks.cfg  archive.tar.gz  original-ks.cfg  zpoolexport
[root@zfs ~]# zpool import -d zpoolexport/
   pool: otus
     id: 6554193320433390805
  state: ONLINE
 action: The pool can be imported using its name or numeric identifier.
 config:

	otus                         ONLINE
	  mirror-0                   ONLINE
	    /root/zpoolexport/filea  ONLINE
	    /root/zpoolexport/fileb  ONLINE
[root@zfs ~]# zpool import -d zpoolexport/ otus
[root@zfs ~]# zpool status
  pool: otus
 state: ONLINE
  scan: none requested
config:

	NAME                         STATE     READ WRITE CKSUM
	otus                         ONLINE       0     0     0
	  mirror-0                   ONLINE       0     0     0
	    /root/zpoolexport/filea  ONLINE       0     0     0
	    /root/zpoolexport/fileb  ONLINE       0     0     0

errors: No known data errors

  pool: otus1
 state: ONLINE
  scan: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	otus1       ONLINE       0     0     0
	  mirror-0  ONLINE       0     0     0
	    sdb     ONLINE       0     0     0
	    sdc     ONLINE       0     0     0

errors: No known data errors

  pool: otus2
 state: ONLINE
  scan: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	otus2       ONLINE       0     0     0
	  mirror-0  ONLINE       0     0     0
	    sdd     ONLINE       0     0     0
	    sde     ONLINE       0     0     0

errors: No known data errors

  pool: otus3
 state: ONLINE
  scan: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	otus3       ONLINE       0     0     0
	  mirror-0  ONLINE       0     0     0
	    sdf     ONLINE       0     0     0
	    sdg     ONLINE       0     0     0

errors: No known data errors

  pool: otus4
 state: ONLINE
  scan: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	otus4       ONLINE       0     0     0
	  mirror-0  ONLINE       0     0     0
	    sda     ONLINE       0     0     0
	    sdh     ONLINE       0     0     0

errors: No known data errors
[root@zfs ~]# zpool get all otus
NAME  PROPERTY                       VALUE                          SOURCE
otus  size                           480M                           -
otus  capacity                       0%                             -
otus  altroot                        -                              default
otus  health                         ONLINE                         -
otus  guid                           6554193320433390805            -
otus  version                        -                              default
otus  bootfs                         -                              default
otus  delegation                     on                             default
otus  autoreplace                    off                            default
otus  cachefile                      -                              default
otus  failmode                       wait                           default
otus  listsnapshots                  off                            default
otus  autoexpand                     off                            default
otus  dedupditto                     0                              default
otus  dedupratio                     1.00x                          -
otus  free                           478M                           -
otus  allocated                      2.09M                          -
otus  readonly                       off                            -
otus  ashift                         0                              default
otus  comment                        -                              default
otus  expandsize                     -                              -
otus  freeing                        0                              -
otus  fragmentation                  0%                             -
otus  leaked                         0                              -
otus  multihost                      off                            default
otus  checkpoint                     -                              -
otus  load_guid                      11716735900226463286           -
otus  autotrim                       off                            default
otus  feature@async_destroy          enabled                        local
otus  feature@empty_bpobj            active                         local
otus  feature@lz4_compress           active                         local
otus  feature@multi_vdev_crash_dump  enabled                        local
otus  feature@spacemap_histogram     active                         local
otus  feature@enabled_txg            active                         local
otus  feature@hole_birth             active                         local
otus  feature@extensible_dataset     active                         local
otus  feature@embedded_data          active                         local
otus  feature@bookmarks              enabled                        local
otus  feature@filesystem_limits      enabled                        local
otus  feature@large_blocks           enabled                        local
otus  feature@large_dnode            enabled                        local
otus  feature@sha512                 enabled                        local
otus  feature@skein                  enabled                        local
otus  feature@edonr                  enabled                        local
otus  feature@userobj_accounting     active                         local
otus  feature@encryption             enabled                        local
otus  feature@project_quota          active                         local
otus  feature@device_removal         enabled                        local
otus  feature@obsolete_counts        enabled                        local
otus  feature@zpool_checkpoint       enabled                        local
otus  feature@spacemap_v2            active                         local
otus  feature@allocation_classes     enabled                        local
otus  feature@resilver_defer         enabled                        local
otus  feature@bookmark_v2            enabled                        local
[root@zfs ~]# zfs get all otus
NAME  PROPERTY              VALUE                  SOURCE
otus  type                  filesystem             -
otus  creation              Fri May 15  4:00 2020  -
otus  used                  2.04M                  -
otus  available             350M                   -
otus  referenced            24K                    -
otus  compressratio         1.00x                  -
otus  mounted               yes                    -
otus  quota                 none                   default
otus  reservation           none                   default
otus  recordsize            128K                   local
otus  mountpoint            /otus                  default
otus  sharenfs              off                    default
otus  checksum              sha256                 local
otus  compression           zle                    local
otus  atime                 on                     default
otus  devices               on                     default
otus  exec                  on                     default
otus  setuid                on                     default
otus  readonly              off                    default
otus  zoned                 off                    default
otus  snapdir               hidden                 default
otus  aclinherit            restricted             default
otus  createtxg             1                      -
otus  canmount              on                     default
otus  xattr                 on                     default
otus  copies                1                      default
otus  version               5                      -
otus  utf8only              off                    -
otus  normalization         none                   -
otus  casesensitivity       sensitive              -
otus  vscan                 off                    default
otus  nbmand                off                    default
otus  sharesmb              off                    default
otus  refquota              none                   default
otus  refreservation        none                   default
otus  guid                  14592242904030363272   -
otus  primarycache          all                    default
otus  secondarycache        all                    default
otus  usedbysnapshots       0B                     -
otus  usedbydataset         24K                    -
otus  usedbychildren        2.01M                  -
otus  usedbyrefreservation  0B                     -
otus  logbias               latency                default
otus  objsetid              54                     -
otus  dedup                 off                    default
otus  mlslabel              none                   default
otus  sync                  standard               default
otus  dnodesize             legacy                 default
otus  refcompressratio      1.00x                  -
otus  written               24K                    -
otus  logicalused           1020K                  -
otus  logicalreferenced     12K                    -
otus  volmode               default                default
otus  filesystem_limit      none                   default
otus  snapshot_limit        none                   default
otus  filesystem_count      none                   default
otus  snapshot_count        none                   default
otus  snapdev               hidden                 default
otus  acltype               off                    default
otus  context               none                   default
otus  fscontext             none                   default
otus  defcontext            none                   default
otus  rootcontext           none                   default
otus  relatime              off                    default
otus  redundant_metadata    all                    default
otus  overlay               off                    default
otus  encryption            off                    default
otus  keylocation           none                   default
otus  keyformat             none                   default
otus  pbkdf2iters           0                      default
otus  special_small_blocks  0                      default

[root@zfs ~]# zfs get available otus
NAME  PROPERTY   VALUE  SOURCE
otus  available  350M   -
[root@zfs ~]# zfs get readonly otus
NAME  PROPERTY  VALUE   SOURCE
otus  readonly  off     default
[root@zfs ~]# zfs get recordsize otus
NAME  PROPERTY    VALUE    SOURCE
otus  recordsize  128K     local

[root@zfs ~]# zfs get compression otus
NAME  PROPERTY     VALUE     SOURCE
otus  compression  zle       local
[root@zfs ~]# zfs get compression otus*
cannot open 'otus*': invalid character '*' in name
[root@zfs ~]# zfs get compression otus1
NAME   PROPERTY     VALUE     SOURCE
otus1  compression  lzjb      local
[root@zfs ~]# fs get checksum otus
-bash: fs: command not found
[root@zfs ~]# zfs get checksum otus
NAME  PROPERTY  VALUE      SOURCE
otus  checksum  sha256     local

[root@zfs ~]# wget -O otus_task2.file --no-check-certificate "https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download"
--2023-11-30 15:12:10--  https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download
Resolving drive.google.com (drive.google.com)... 142.250.184.206, 2a00:1450:4001:802::200e
Connecting to drive.google.com (drive.google.com)|142.250.184.206|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://drive.google.com/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download [following]
--2023-11-30 15:12:11--  https://drive.google.com/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download
Reusing existing connection to drive.google.com:443.
HTTP request sent, awaiting response... 303 See Other
Location: https://doc-00-bo-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/e2s65t3pajn5757r9tfmsl72d0ekrqug/1701357075000/16189157874053420687/*/1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG?e=download&uuid=f0e4fa45-20d4-4335-abc9-10636f2ca403 [following]
Warning: wildcards not supported in HTTP.
--2023-11-30 15:12:17--  https://doc-00-bo-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/e2s65t3pajn5757r9tfmsl72d0ekrqug/1701357075000/16189157874053420687/*/1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG?e=download&uuid=f0e4fa45-20d4-4335-abc9-10636f2ca403
Resolving doc-00-bo-docs.googleusercontent.com (doc-00-bo-docs.googleusercontent.com)... 142.250.186.129, 2a00:1450:4001:80f::2001
Connecting to doc-00-bo-docs.googleusercontent.com (doc-00-bo-docs.googleusercontent.com)|142.250.186.129|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5432736 (5.2M) [application/octet-stream]
Saving to: 'otus_task2.file'

100%[============================================================================================================>] 5,432,736   5.31MB/s   in 1.0s   

2023-11-30 15:12:19 (5.31 MB/s) - 'otus_task2.file' saved [5432736/5432736]

[root@zfs ~]# zfs receive otus/test@today < otus_task2.file
[root@zfs ~]# find /otus/test -name "secret_message"
/otus/test/task1/file_mess/secret_message
[root@zfs ~]# cat /otus/test/task1/file_mess/secret_message
https://github.com/sindresorhus/awesome
```

