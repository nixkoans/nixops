# NixOS on linode

Implementing with GPT, BIOS, Grub2 boot loader in linode's KVM environment, direct disk boot

## Prepare the disk with appropriate partitions

```
gdisk /dev/sda
```

Here's the partitions I created:

```
Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048            4095   1024.0 KiB  EF02  BIOS boot partition
   2            4096         1028095   500.0 MiB   8300  Linux filesystem
   3         1028096         3125247   1024.0 MiB  8200  Linux swap
   4         3125248        50331614   22.5 GiB    8300  Linux filesystem
```

After creating the partitions with gdisk, let's format them:

```
mkfs.ext4 -L nixos /dev/sda4
mkfs.ext4 -L boot /dev/sda2
mkswap -L swap /dev/sda3
swapon /dev/sda3  # switch on the swap partition so nixos-install doesn't fail with out of memory error
```

## Boot into finnix rescue disk on linode

```
mount /dev/sda4 /mnt
mkdir -p /mnt/boot
mount /dev/sda2 /mnt/boot
swapon /dev/sda3
```

## Bootstrap nix installers

```
adduser nix

groupadd -r nixbld # we need the Nix build users
for n in $(seq 1 10); do useradd -c "Nix build user $n" \
-d /var/empty -g nixbld -G nixbld -M -N -r -s "$(which nologin)" \
nixbld$n; done

mkdir /nix
chown -R nix /nix
su - nix
bash <(curl https://nixos.org/nix/install)
exit # exit back as root user in finnix

# We can continue doing this as root in finnix
. ~nix/.nix-profile/etc/profile.d/nix.sh
nix-channel --remove nixpkgs
nix-channel --add http://nixos.org/channels/nixos-14.12 nixos
nix-channel --update

# Install vim so we can use it for editing the configuration.nix later
nix-env -i vim

# temporary configuration.nix
cat <<EOF > configuration.nix
{ fileSystems."/" = {};
boot.loader.grub.enable = false;
}
EOF

# Now we have the nixos-install, nixos-generate-config etc tools for us to bootstrap our disks
export NIX_PATH=nixpkgs=/root/.nix-defexpr/channels/nixos:nixos=/root/.nix-defexpr/channels/nixos/nixos
export NIXOS_CONFIG=/root/configuration.nix
nix-env -i -A config.system.build.nixos-install -A config.system.build.nixos-option -A config.system.build.nixos-generate-config -f "<nixos>"

# Generate configuration.nix on our target disks
export NIX_PATH=nixpkgs=/root/.nix-defexpr/channels/nixos:nixos=/root/.nix-defexpr/channels/nixos/nixos
nixos-generate-config --root /mnt
```

## Install!

```
unset NIXOS_CONFIG
nixos-install
```
