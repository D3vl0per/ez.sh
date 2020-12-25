# ez.sh
bash(LUKS + detached header + GPG)


# Unseal base script
```
#!/bin/bash
gpg -d <gpg-encrypted-key-file> | sudo cryptsetup open --header <header-file> /dev/sdxX --key-file=- <mounted-mapper-name>
sudo mount <mounted-mapper-name> <mounting-point>
```

# Seal base script
```
#!/bin/bash

sync
fuser -km <mounting-point>
sudo umount <mounting-point>
sudo cryptsetup close <mounted-mapper-name>
```

# Manual 

## Generate encryption key
`dd if=/dev/urandom of=<key-file> bs=1024 count=2`

## Generate empty header file
`dd if=/dev/zero of=<header-file> bs=16M count=1`

## Format partition
`sudo cryptsetup luksFormat /dev/sdxX --key-file=<key-file-path> --offset 32768 --header <header-file-path>`

## Open luks
`sudo cryptsetup open /dev/sdxX --key-file=<key-file-path> --header <header-file-path>  <mounted-mapper-name>`

## Encrypt anything with gpg cli
`gpg --output <output> --encrypt --recipient <email> <file>`

## Open luks with GPG encrypted key file
`gpg -d <encrypted-key-file-path> | sudo cryptsetup open /dev/sdxX --key-file=- --header <header-file-path>  <mounted-mapper-name>`
