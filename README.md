# Fast `cargo install` action

> **Note**
> This is a fork of https://github.com/dtolnay/install to include packages
>

This GitHub Action installs a Rust crate using precompiled signed binaries built
on GitHub and hosted as GitHub release artifacts.

## Example workflow

```yaml
name: test suite
on: [push, pull_request]

jobs:
  expand:
    name: cargo expand
    runs-on: ubuntu-latest
    steps:
      - uses: kaankoken/install@master
        with:
          crate: cargo-expand
      - run: cargo expand --help
```

<img src="https://user-images.githubusercontent.com/1940490/136493915-2c3c6a6b-620c-46e1-be4b-3c96856ccd12.png">

## Inputs

| Name    | Required | Description                                  |
| ------- | :------: | -------------------------------------------- |
| `crate` | ✓        | Name of crate as published to crates.io      |
| `bin`   |          | Name of binary; default = same as crate name |

## GPG Key

### Install GPG

```bash
# Install GPG
brew install gpg

# Create config folders & files
mkdir ~/.gnupg
touch ~/.gnupg/gpg.conf ~/.gnupg/gpg-agent.conf

# Add this line to ~/.gnupg/gpg.conf
use-agent

# Add those line to ~/.gnupg/gpg-agent.conf
default-cache-ttl 34560000
max-cache-ttl 34560000

# Add those to ~/.zshrc
export GPG_TTY=$(tty)
gpgconf --launch gpg-agent

# Run
source ~/.zshrc
```

### Create a Key

- When `gpg` asks your `name` & `e-mail` enter:
```bash
name -> github.com/user-name/repo-name
email -> emailed used in github account
```

```bash
# Generate key with passphrase
gpg --full-generate-key

# Example
/home/username/.gnupg/secring.gpg
----------------------------------
sec   4096R/XXXX <creation date>
uid                  name <email.address>
ssb   4096R/YYYY <creation date>

# Get secret key (the XXXX part)
gpg --list-secret-keys --keyid-format=long

# Edit the key to remove passphrase
gpg --edit-key XXXX
```

- Type `passwd`
- Enter your existing passphrase.
- Enter the new passphrase for this secret key. (Leave this blank and press Enter)
- Press `Enter` twice and consider the warnings from the tool and its implications before proceeding

### Get output & Replace signing-key in the repo

```bash
gpg --output private.gpg --armor --export-secret-key github.com/user-name/repo-name
gpg --output signing-key.asc --armor --export github.com/user-name/repo-name
```

## License

The scripts and documentation in this project are released under the [MIT
License].

[MIT License]: LICENSE
