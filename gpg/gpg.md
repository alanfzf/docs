# GPG YubiKey setup

## How to import a key

```bash
gpg --card-edit
fetch
quit

# once fetched list the keys
gpg --list-keys
gpg --edit-key <KEYHERE>
trust 5
```

## Create new keys
- [Full Tutorial](https://support.yubico.com/s/article/Using-Your-YubiKey-with-OpenPGP)
```bash
gpg --expert --full-gen-key
# select (1) RSA and RSA
gpg --expert --edit-key 1234ABC
# key type 8 RSA (set your own capabilities)
```

## Create new Encryption sub key

Create new encrypt sub key for the existing key:

```bash
gpg --batch --quick-add-key <KEYHERE> "rsa4096" encrypt 1y

gpg --edit-key 8F486C12805382BB
addkey
6
save
```

## Factory reset YubiKey

```bash
# 1. enter card edit mode
gpg --card-edit
# 2. enter admin mode
admin
# 3. delete all keys
factory-reset
# 4. get Key fingerprint
gpg --list-secret-keys --keyid-format LONG
# 5. delete local stubs
gpg --batch --yes --delete-secret-and-public-key <key-finger-print>
```
