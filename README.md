# preseed-ptables

Debian Preseed partition layouts for use with Foreman

## LVM with custom layout

```
<%#
kind: ptable
name: Preseed LVM Enhanced
oses:
- Debian
- Ubuntu
%>

# Erste Disk wird als Installdisk benutzt
d-i partman/early_command string debconf-set partman-auto/disk "$(list-devices disk | head -n1)"

# LVM verwenden
d-i partman-auto/method string lvm
d-i partman-auto-lvm/guided_size string max
d-i partman-auto-lvm/new_vg_name string rootvg

# Standard Filesystem
d-i partman/default_filesystem string xfs

# Name der Volume Group
d-i partman-auto-lvm/new_vg_name string rootvg

# Custom Recipie
d-i partman-auto/choose_recipe select custom

# Standard Partionierung
d-i partman-auto/expert_recipe string               \
      custom ::                                     \
              12288 12388 12288 $default_filesystem \
                      $lvmok{ }                     \
                      lv_name{ root }               \
                      method{ format }              \
                      format{ }                     \
                      use_filesystem{ }             \
                      $default_filesystem{ }        \
                      mountpoint{ / }               \
              .                                     \
              2048 2148 2048 $default_filesystem{ } \
                      $lvmok{ }                     \
                      lv_name{ home }               \
                      method{ format }              \
                      format{ }                     \
                      use_filesystem{ }             \
                      $default_filesystem{ }        \
                      mountpoint{ /home }           \
              .                                     \
              8192 8292 8192 $default_filesystem{ } \
                      $lvmok{ }                     \
                      lv_name{ opt }                \
                      method{ format }              \
                      format{ }                     \
                      use_filesystem{ }             \
                      $default_filesystem{ }        \
                      mountpoint{ /opt }            \
              .                                     \
              8192 8292 8192 $default_filesystem{ } \
                      $lvmok{ }                     \
                      lv_name{ var }                \
                      method{ format }              \
                      format{ }                     \
                      use_filesystem{ }             \
                      $default_filesystem{ }        \
                      mountpoint{ /var }            \
              .                                     \
              4096 4196 4096 $default_filesystem{ } \
                      $lvmok{ }                     \
                      lv_name{ tmp }                \
                      method{ format }              \
                      format{ }                     \
                      use_filesystem{ }             \
                      $default_filesystem{ }        \
                      mountpoint{ /tmp }            \
              .                                     \
              4096 0% 4096 linux-swap{ }          \
                      $lvmok{ }                     \
                      lv_name{ swap }               \
                      method{ swap }                \
                      format{ }                     \
              .

# Alle Installer Meldungen unterdruecken
d-i partman/confirm_write_new_label     boolean true
d-i partman/choose_partition            select  finish
d-i partman/confirm_nooverwrite         boolean true
d-i partman/confirm                     boolean true
d-i partman-auto/purge_lvm_from_device  boolean true
d-i partman-lvm/device_remove_lvm       boolean true
d-i partman-lvm/confirm                 boolean true
d-i partman-lvm/confirm_nooverwrite     boolean true
d-i partman-auto-lvm/no_boot            boolean true
d-i partman-md/device_remove_md         boolean true
d-i partman-md/confirm                  boolean true
d-i partman-md/confirm_nooverwrite      boolean true
```
