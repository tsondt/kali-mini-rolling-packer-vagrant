# Packer Template and Vagrantfile for Kali Linux Rolling
Packer Template and Vagrantfile to spin up a minimal Kali Linux Rolling box (around 450MB) from the network install ISO.

## Usages
You will need [Packer](https://www.packer.io/docs/installation.html) and [Vagrant](https://www.vagrantup.com/docs/installation/) in your path.

Kali Rolling network install ISO: [Download](http://http.kali.org/kali/dists/kali-rolling/main/installer-i386/current/images/netboot/mini.iso)

SHA256SUM `fd29c24c541cf22de9f2fdb2f8f2d44635862db54b3caf0e30ca4172107a80ec`

(Notes: The iso file and its SHA256SUM provided in this repo are for reference only, it is recommended to download the iso image directly from `http.kali.org` and [verify its authenticity](http://docs.kali.org/introduction/download-official-kali-linux-images#sha1sums))

```
git clone https://github.com/tsondt/kali-mini-rolling-packer-vagrant
cd kali-mini-rolling-packer-vagrant
rm -f mini.iso
curl -sOL http://http.kali.org/kali/dists/kali-rolling/main/installer-i386/current/images/netboot/mini.iso
sed -i.bak "s/fd29c24c541cf22de9f2fdb2f8f2d44635862db54b3caf0e30ca4172107a80ec/$(openssl sha -sha256 mini.iso | cut -d" " -f2)/" kali-mini-rolling.json
packer build kali-mini-rolling.json
vagrant box add --name kali-mini-rolling kali-mini-rolling_virtualbox.box
vagrant up
ssh -p 2222 root@127.0.0.1
```

Default SSH username/password is `root`/`t00r`.

Install `i3` and VirtualBox Guest Additions:

```
apt update && apt install -y i3 virtualbox-guest-x11
reboot
```

Clean up:

```
vagrant destroy
vagrant box remove kali
rm -f kali-mini-rolling_virtualbox.box
```

## References
1. https://github.com/Wh1t3Rh1n0/kali-unattended
2. https://github.com/nickryand/kali-packer
3. https://github.com/offensive-security/kali-linux-preseed

