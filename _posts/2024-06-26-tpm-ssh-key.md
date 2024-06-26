---
layout: single
title: Install Hydra with PostgreSQL
tags:
  - fedora
  - ssh
  - tpm2
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to SSH  
---
# Setup

```bash
sudo dnf install tpm2-pkcs11 tpm2-pkcs11-tools -y
sudo usermod -a -G tss "$(id -nu)"
newgrp tss
```

# Create Key

```bash
unset HISTFILE
tpm2_ptool init
tpm2_ptool init
tpm2_ptool addtoken --pid=1 --label=ssh --userpin=MySecretPassword --sopin=MyRecoveryPassword
tpm2_ptool addkey --label=ssh --userpin=MySecretPassword --algorithm=ecc256
```

# Export Public Key

```bash
ssh-keygen -D /usr/lib64/pkcs11/libtpm2_pkcs11.so > ~/.ssh/my-ssh-key_using-tpm2.pub
cat > ~/.ssh/config << EOF
Host server
    PKCS11Provider /usr/lib64/pkcs11/libtpm2_pkcs11.so
    PasswordAuthentication no
EOF
```

# Put Public Key 

```bash 
cat ~/.ssh/my-ssh-key_using-tpm2.pub | ssh -i ~/.ssh/my-identity-key user@server "cat >> ~/.ssh/authorized_keys"

```