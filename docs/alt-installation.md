# Alternative Installation

This page outlines alternative installation instructions for the `earthly` build tool. The main instructions that most users need are available on the [installation intructions page](https://earthly.dev/get-earthly).

## Pre-requisites

* [Docker](https://docs.docker.com/install/)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* (*Windows only*) [Docker WSL 2 backend](https://docs.docker.com/docker-for-windows/wsl/)

## Install earthly

Download the binary relevant to your platform from [the releases page](https://github.com/earthly/earthly/releases), rename it to `earthly` and place it in your `bin`.

To initialize the installation, including adding auto-completion for your shell, run

```bash
sudo earthly bootstrap --with-autocomplete
```

and then restart your shell.

### CI

For instructions on how to install `earthly` for CI use, see the [CI integration guide](ci-integration/overview.md).

### Installing from Earthly repositories (**experimental**)

{% hint style='danger' %}
##### Important

Our rpm and deb repositories are currently in **Experimental** stage.

* The repository may break, be changed drastically with no warning, or be removed altogether.
* Check the [GitHub tracking issue](https://github.com/earthly/earthly/issues/986) for any known problems.
* Give us feedback on [Slack](https://earthly.dev/slack).
{% endhint %}

Earthly can be installed for Debian and RedHat based Linux distributions via the Earthly deb and rpm repositories.

All of our binaries are signed with our [PGP key](https://pkg.earthly.dev/earthly.pgp); which has the fingerprint:

    5816 B221 3DD1 CEB6 1FC9 52BA B118 5ECA 33F8 EB64

#### Debian-based repositories (including Ubuntu)

Debian-based Linux users (e.g. debian, ubuntu, mint, etc) can use our apt repo to install Earthly.

Before installing Earthly, you must first setup the Earthly apt repo.

1. Update apt and install required tools to support https-based apt repos:

   ```bash
   sudo apt-get update
   sudo apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg \
      lsb-release
   ```

2. Download Earthly's GPG key:

   ```bash
   curl -fsSL https://pkg.earthly.dev/earthly.pgp | sudo gpg --dearmor -o /usr/share/keyrings/earthly-archive-keyring.gpg
   ```

3. Setup the stable repo:

   ```bash
   echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/earthly-archive-keyring.gpg] https://pkg.earthly.dev/deb \
     stable main" | sudo tee /etc/apt/sources.list.d/earthly.list > /dev/null
   ```

4. Install Earthly:

   ```bash
   sudo apt-get update
   sudo apt-get install earthly
   ```


#### Fedora repositories

Fedora users can use our rpm repo to install Earthly.

1. Install plugins required to manage DNF repositories:

   ```bash
   sudo dnf -y install dnf-plugins-core
   ```

2. Add the Earthly repo to your system:

   ```bash
   sudo dnf config-manager \
       --add-repo \
       https://pkg.earthly.dev/earthly.repo
   ```

3. Install Earthly:

   ```bash
   sudo dnf install earthly
   ```

#### CentOS repositories

CentOS users can use our rpm repo to install Earthly.

1. Install utils required to manage yum repositories:

   ```bash
   sudo yum install -y yum-utils
   ```

2. Add the Earthly repo to your system:

   ```bash
   sudo yum-config-manager \
       --add-repo \
       https://pkg.earthly.dev/earthly.repo
   ```

3. Install Earthly:

   ```bash
   sudo yum install earthly
   ```

### Native Windows

{% hint style='danger' %}
##### Important

Our native Windows release is currently in the **Experimental** stage.

* The release ships with known issues. Many things work, but some don't.
* Check the [GitHub tracking issue](https://github.com/earthly/earthly/issues/1031) for any known problems.
* Give us feedback on [Slack](https://earthly.dev/slack).

{% endhint %}

To install the Windows release, simply download the binary [here](https://github.com/earthly/earthly/releases/latest/download/earthly-windows-amd64.exe) (or from our [release page](https://github.com/earthly/earthly/releases/latest/)); and ensure it is within your `PATH`.

To add `earthly.exe` to your `PATH` environment variable:

1. Search and select: System (Control Panel)
2. Click the Advanced system settings link.
3. Click Environment Variables. In the "System Variables" section, select the PATH environment variable and click Edit.
   * If the PATH environment variable does not exist, click New.
4. In the Edit window, specify the value of the PATH environment variable, and Click OK.
5. Close and reopen any existing terminal windows, so they will pick up the new `PATH`.

If you are going to mostly be working from a WSL2 prompt in Windows, you might want to consider following the Linux instructions for installation. This will help prevent any cross-subsystem file transfers and keep your builds fast. Note that the "original" WSL is unsupported.

### Installing from source

To install from source, see the [contributing page](https://github.com/earthly/earthly/blob/main/CONTRIBUTING.md).

## Configuration

If you use SSH-based git authentication, then your git credentials will just work with Earthly. Read more about [git auth](./guides/auth.md).

For a full list of configuration options, see the [Configuration reference](./earthly-config/earthly-config.md)

## Verify installation

To verify that the installation works correctly, you can issue a simple build of an existing hello-world project

```bash
earthly github.com/earthly/hello-world:main+hello
```

You should see the output

```
github.com/earthly/hello-world:main+hello | --> RUN [echo 'Hello, world!']
github.com/earthly/hello-world:main+hello | Hello, world!
github.com/earthly/hello-world:main+hello | Target github.com/earthly/hello-world:main+hello built successfully
=========================== SUCCESS ===========================
```

# Uninstall

To remove earthly, run the following commands:

## macOS users

```bash
brew uninstall earthly
rm -rf ~/.earthly
docker rm --force earthly-buildkitd
docker volume rm --force earthly-cache
```

## Linux and WSL2 users

```bash
rm /usr/local/bin/earthly
rm /usr/share/bash-completion/completions/earthly
rm /usr/local/share/zsh/site-functions/_earthly
rm -rf ~/.earthly
docker rm --force earthly-buildkitd
docker volume rm --force earthly-cache
```
