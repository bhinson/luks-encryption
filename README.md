Playbooks for LUKS-based encryption

encrypt-existing:
----------------

Encrypt the filesystem(s) on an existing system.  Afterwards, migrate the existing data to the
encrypted partition.  Since this is not an in-place migration, it uses equal-sized disks which
are added beforehand.

Assumptions / ToDo:

- Source disks are all different sizes (in GB)
- Equivalent sized disks have already been added
- LVM VG names with single dash are specified as double dash
- LVM can use full disk PV (i.e. no partition table), but non-LVM must have a partition
- There's a single partition on each disk. Multiple partitions are not yet taken into account
- Single PV per VG. Multiple PVs for a single VG are note yet taken into account
- One LV per VG (limitation from updating fstab with LVM setup)
- Root partition is non-LVM
- Using root=UUID=... in /boot/grub2/grub.cfg
- After encryption, the remaining unencrypted filesystems are not wiped, only the signature of the root filesystem

encrypt-new:
-----------

Create a new encrypted filesystem and mount to a specified directory.  The disk size is specified, and must
be the only disk of that size added to the system.

nbde-server:
-----------

Install and configure a RHEL 7 system as Network Bound Disk Encryption (NBDE) server.  This server is used to
automatically decrypt a LUKS-encrypted device at startup, without user intervention.  A single LUKS-encrypted disk
can have up to 8 NBDE servers associated, for failover purposes.
