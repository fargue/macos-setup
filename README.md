# macos-setup

## Display Link Drivers if you need them

[Download link](https://www.synaptics.com/products/displaylink-graphics/downloads)

## Xcode

```
xcode-select --install
```

## Install brew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## iTerm2

`brew install iterm2 --cask`

## clone local setup for mac

Setup PAT in github via User -> Settings -> DeveloperSettings (at the bottom)

```
mkdir -p $HOME/project/personal
cd $HOME/project/personal
git clone https://github.com/fargue/macos-setup.git
# use fargue/[new pat]

mkdir ~/bin
ln -s HOME/project/personal/macos-setup/bin/mac-netstat ~/bin
ln -s HOME/project/personal/macos-setup/bin/rds-events ~/bin
```


## Install zsh and starship

```
brew install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

curl -sS https://starship.rs/install.sh | sh


ln -s $HOME/project/personal/macos-setup/zsh-environment-variables.mac-001 ~/.local-environment-variables

ln -s $HOME/project/personal/macos-setup/starship.toml ~/.config/starship.toml

ln -sf $HOME/project/personal/macos-setup/zshrc.mac-001  ~/.zshrc

source ~/.zshrc

# Note you'll see warnings about pyenv. That's ok, we'll install that later
```

## setup git

```
git config --global user.name "Jerry Arnold"
git config --global user.email "jrarnold@trevipay.com"
git config --global core.editor vim
git config --global core.ui true
```

## Starship

```
curl -sS https://starship.rs/install.sh | sh

```

[config](https://starship.rs/config/)


## Joplin

```
brew install joplin
```

Sync with DropBox via jerryarn@gmail.com login

## Setup Keys

[Reference](https://phoenixnap.com/kb/generate-setup-ssh-key-ubuntu)

```
mkdir -p $HOME/.ssh
chmod 0700 $HOME/.ssh
ssh-keygen
```

## Aliases (must have above starship setup done)

```sh
ln -sf $HOME/project/personal/macos-setup/zsh-alias ${ZSH_CUSTOM}/aliases.zsh
```

## Brew prereqs

```
brew install \
    awscli \
    aws-iam-authenticator \
    docker-credential-helper-ecr \
    eksctl \
    fzf \
    git \
    helm \
    java \
    jenv \
    jq \
    kubernetes-cli \
    pluto \
    rbenv \
    readline \
    stern \
    tfenv \
    watch \
    wget \
    xz

brew install --cask dbeaver-community flux macdown notunes
```

## Pyenv

```
brew install pyenv
```

### Install some versions

```
pyenv install --list
pyenv install 3.13.1
pyenv global 3.13.1
```

## Pip installs

```
pip install aws-mfa
```

## Setup AWS cli

```
mkdir ~/.aws
chmod 700 ~/.aws
echo "[default-long-term]" > ~/.aws/credentials
echo "aws_access_key_id = AKIAWXXXXXXX" >> ~/.aws/credentials
echo "aws_secret_access_key = XXXXXXXXX" >> ~/.aws/credentials
echo "aws_mfa_device = arn:aws:iam::434875166128:mfa/jrarnold@multiservice.com" >> ~/.aws/credentials
 
chmod 600 ~/.aws/credentials
```

## Rancher Desktop

```
mkdir -p $HOME/project/msts/devops/
cd $HOME/project/msts/devops
git clone https://gitlab.com/msts-enterprise/devops/local-development.git
cd local-development
bin/reset-rancher.sh

# follow instructions to download. Install the downloaded dmg file
# Run Rancher Desktop from /Applications folder
# Popup Welcome to Rancher Desktop window should appear
#   -- Pick needed kubernetes version
#   -- Select Container Engine
#   -- Set Configure Path to Automatic
# Click OK

# If you get startup errors like:
# 
# Error: /Applications/Rancher Desktop.app/Contents/Resources/resources/darwin/lima/bin/limactl.ventura exited with code 1

# To into your Preferences and set the Virtual Machine -> Emulation from QEMU to VZ then restart.


# Once Rancher is up and running:
bin/reset-rancher.sh
```

## SQLCi

[Download Link](https://www.oracle.com/database/sqldeveloper/technologies/sqlcl/download/)

```
mkdir ~/software
mv ~/Downloads/sqlcl* ~/software
cd ~/software
unzip sqlcl-*.zip
ln -sf ~/software/sqlcl/bin/sql ~/bin/sql
ln -sf ~/software/sqlcl/bin/sql ~/bin/sqlplus
```

## KubeContexts

```
aws eks --region us-east-1 \
    update-kubeconfig --name staging-blue \
    --alias staging-blue.us-east-1 \
    --role-arn arn:aws:iam::434875166128:role/MstsDevopsEKSClusterAdmin

aws eks --region us-east-1 \
    update-kubeconfig --name staging-green \
    --alias staging-green.us-east-1 \
    --role-arn arn:aws:iam::434875166128:role/MstsDevopsEKSClusterAdmin

aws eks --region us-east-1 \
    update-kubeconfig --name production-green \
    --alias production-green.us-east-1 \
    --role-arn arn:aws:iam::434875166128:role/MstsDevopsEKSClusterAdmin

aws eks --region eu-west-1 \
    update-kubeconfig --name production-green \
    --alias production-green.eu-west-1 \
    --role-arn arn:aws:iam::434875166128:role/MstsDevopsEKSClusterAdmin

aws eks --profile=MSTS-Core-Services-CDE-Admin --region us-east-1 \
    update-kubeconfig --name cde-green \
    --alias cde-green.us-east-1 \
    --role-arn arn:aws:iam::612430976833:role/MstsDevopsEKSClusterAdmin
```