
Version | Comments
---|---
Ver 1.0 | last edit: 2018 Author: David Zhang

## tips

### cscope
find ./ -name "*.[ch]" > cscope.files
ctags -x cscope.files
> truss cscope.files for issues

###  enable kmdb during ak boot up
in 'platform/.../kernel' '-k'
if drops to shell, run "mdb -K", :c

### set breakpointer when boot
::bp zfs`spa_init
> NB: this is the very first function from zfs module, so that we can init
lots of flags when this bp is hit.

### debug akd startup by mdb
mdb /usr/sbin/akd
::bp break_func
::run -f
> p,p... press p to continue

### set bp in akd
mdb -p `pgrep -o akd`
libzfs.so.1`send_iterate_snap:b
:c

### where dbx
/net/tufnel.us.oracle.com/export/SUNWspro/i386/SUNWspro/developerstudio12.6/bin/dbx

### force core dump
reboot -d

### handy solaris cli
date 1053.00

### pause a kthread
int spin;
while (spin)
	    delay(hz);


### enable debug info
export STRIPSTABS_KEEP_STABS=1
dmake DEBUGFORMAT="-xdebugformat=dwarf" COPTFLAG="-g" COPTFLAG64="-g" install

### keep driver attached
modload /kernel/drv/smp

### unzip tar.gz
gunzip ak.27581125-05-target.tar.gz |tar -xf -

### mount another root to boot
mount -F zfs -o remount,rw system/root/ak-nas-xxxxxx

### du all files and dirs
du -dshL *
du -dshL .

### get a version associated with a tag
hg update 'changeset'
hg id

### disable autoindent in vi
set nosmartindent filetype=txt noautoindent

### shift group in vim
1) v
2) >

### sed,awk, etc string trimming
ggrep -A(after) -B(before)
diff -U2 ...
cut -f1 -d ":" result.txt |sed 's:.*/::'

### manually run stc test
export PATH=$PATH:/opt/SUNWstc-genutils/bin/
cd /opt/SUNWstc-fs-zfs/tests/functional/cli_root/zfs_receive
/opt/SUNWstc-stf/bin/i386/stf_execute -m i386 zfs_receive_013_pos

export PATH=$PATH:/opt/SUNWstc-stf/bin/amd64:/opt/SUNWstc-genutils/bin
stf_configure -c DISKS="c1t57d0 c1t58d0 c1t59d0"
if you want to add a crypto wrapper, Mark Musante told me how to do it and you can do it this way:
stf_configure -c WRAPPER=crypto -c DISKS="c0t1d0 c0t2d0 c0t3d0"
stf_execute -m `uname -p` resilver_006_neg

## mdb & dtrace

### sd mdb
*sd_state::walk softstate
*sd_state::walk softstate|::print struct sd_lun ! grep cmds

### zfs mdb
::taskq -n zio

### generic - thread running time
ffffa1c2193e4000::print kthread_t t_disp_time|::hrtime
::stacks -c func|::thread -d
::offsetof dsl_dataset_t 158

### dtrace
dtrace -n 'ERROR { stack() }'

dtrace -n 'pid$target:libc ...' -p 809

ERROR { @errstack[stack()] = count() }

END { printa(@errstack) }

### boot to kmdb
reboot -e raw_64a -- -kd

### enforce kmdb to ignore dtrace
kdi_dtrace_state/W 1

### truss
truss -u 'libzfs::zfs_unmount*' -u ...
truss -fT open zfs recv





---

### misc
savecore -vf /var/crash/testsystem/vmdump.0

cp /net/pietro/builds/ws/yidzhan/st-ws/on-server/usr/src/tools/scripts/onu.sh /root/
ksh /root/onu.sh -d /net/pietro/builds/ws/yidzhan/st-ws/on-server/data/server-pkg/i386-debug -t on12-sc-
ksh /root/onu.sh -d /ws/on-gate/packages/daily.2017-08-09/server-i386-debug/ -t on12-daily-

::ptree
fffff66fd5206030::walk thread|::threadlist -v

# revert a file
hg list
hg revert $file

# if the file is already checked in
hg reci
hg backout

usr/lib/fm/fmd/fmd -o fg=true -o client.debug=true
fmd -o debug=help
usr/lib/fm/fmd/fmd -o fg=true -o client.debug=true -o debug=topo

pkg set-publisher -G'*' -g http://ipkg.us.oracle.com/solaris12/dev/ solaris
pkg set-publisher -P -g /ws/on12-gate/snapshots/packages/latest/repo.osnet-internal/publisher/extra extra
pkg install system/dtrace/dtrace-ei
pkg update --accept --be-name=s12_20

pkg change-facet facet.version-lock.system/zones/brand/brand-sn1=false
pkg unfreeze system/zones/brand/brand-sn1
pkg install system/zones/brand/brand-sn1

webrev -o webrev_listfmd -w list_libfmd.txt
webrev -p /net/hyper/tank2/ws/yidzhan/on12-cd2/webrev -o webrev_listfmd_on -w list_libfmd.txt

webrev -o webrev_on -w list_on.txt
webrev -p /net/hyper/tank2/ws/yidzhan/on12-cd2/webrev_on -o webrev_whole_on -w list_on.txt

workspace parent -p /ws/events-gate

pkg install sg3_utils

/ws/on11update-tools/onbld/bin/wx
wx init 3 y
wx pb -n
. ./myenv.sh
make

===
sparc boot net
===
init 0
ok boot net:dhcp

===
pkgfmt -c file to check pkg format, do not use -d or others
===


