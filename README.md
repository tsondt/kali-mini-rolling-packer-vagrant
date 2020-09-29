# Packer Template and Vagrantfile for Kali Linux Rolling
Packer Template and Vagrantfile to spin up a minimal Kali Linux Rolling box (around 800MB) from the network install ISO.

## Usages
You will need [Packer](https://www.packer.io/docs/installation.html) and [Vagrant](https://www.vagrantup.com/docs/installation/) in your path.

Kali Rolling network install ISO: [ISO](https://http.kali.org/kali/dists/kali-rolling/main/installer-amd64/current/images/netboot/mini.iso) [SHA256SUMS](https://http.kali.org/kali/dists/kali-rolling/main/installer-amd64/current/images/netboot/SHA256SUMS)

(Notes: The ISO file and its SHA256SUM provided in this repo are for reference only, it is recommended to download the iso image directly from `http.kali.org` and [verify its authenticity](https://www.kali.org/docs/introduction/download-official-kali-linux-images/))

```
git clone https://github.com/tsondt/kali-mini-rolling-packer-vagrant
cd kali-mini-rolling-packer-vagrant
rm -f mini.iso
curl -sOL https://http.kali.org/kali/dists/kali-rolling/main/installer-amd64/current/images/netboot/mini.iso
curl -sOL https://http.kali.org/kali/dists/kali-rolling/main/installer-amd64/current/images/netboot/SHA256SUMS
sed -i.bak "s/830b876858d87690993567d12c4bf5cf5135ec2cc659ef141d0baaf64be1cc55/$(openssl sha1 -sha256 mini.iso | cut -d" " -f2)/" kali-mini-rolling.json
packer build kali-mini-rolling.json
vagrant box add --name kali-mini-rolling kali-mini-rolling_virtualbox.box
vagrant up
```

Default SSH username/password is `kali`/`kali`:

```
ssh -p 2222 kali@127.0.0.1
```

Install `i3`:

```
sudo apt update && sudo apt install -y lightdm kali-desktop-core kali-desktop-i3
sudo reboot
```

Notes:
- Add `display-setup-script=/usr/bin/VBoxClient-all` to `/etc/lightdm/lightdm.conf` for guest auto-resizing (if broken)
- Run `sudo VboxClient --vmsvga` to fix auto-resizing if using `VMSVGA` (if broken)
- https://www.virtualbox.org/ticket/11606#comment:79

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
4. https://www.packer.io/guides/automatic-operating-system-installs/preseed_ubuntu
5. https://tools.kali.org/kali-metapackages
