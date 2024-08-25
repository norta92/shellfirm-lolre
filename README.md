<p align="center">
<img src="https://github.com/kaplanelad/shellfirm/actions/workflows/ci.yaml/badge.svg"/>
<img src="https://github.com/kaplanelad/shellfirm/actions/workflows/release.yml/badge.svg"/>
</p>

<a href='https://ko-fi.com/eladkaplan' target='_blank'><img height='35' style='border:0px;height:34px;' src='https://az743702.vo.msecnd.net/cdn/kofi3.png?v=0' border='0' alt='Buy Me a Coffee at ko-fi.com' />
  
# shellfirm

<div align="center">
<h1>Opppppsss <b>lo</b>¿hiciste de nuevo? :scream: :scream: :cold_sweat:</h1>
</div>

¿Cómo me salvo de mí mismo?
* `rm -rf *`
* `git reset --hard` ¿Antes de pulsar la tecla Enter?
* `kubectl delete ns` ¡Alto! Vas a eliminar muchos recursos.
* ¡Y muchas más!

¿Quieres aprender de los errores de otras personas?

`shellfirm`interceptará cualquier patrón riesgoso e inmediatamente solicitará un pequeño desafío que verificará dos veces su acción, piense en ello como un captcha para su terminal.

```bash
rm -rf /
#######################
# RISKY COMMAND FOUND #
#######################
* You are going to delete everything in the path.

Solve the challenge: 8 + 0 = ? (^C to cancel)
```

## ¿Cómo funciona?
`shellfirm` evaluará todos los comandos de shell en segundo plano.
Si se detecta un patrón riesgoso, recibirás inmediatamente un mensaje con la advertencia correspondiente para verificar tu comando.

## Ejemplo
![](./docs/media/example.gif)


## Setup your shell  

### Install via brew
```bash
brew tap kaplanelad/tap && brew install shellfirm
```

Or download the binary file from [releases page](https://github.com/kaplanelad/shellfirm/releases), unzip the file and move to `/usr/local/bin` folder.

Validate shellfirm installation
```
shellfirm --version
```

## Verify installation
```
mkdir /tmp/shellfirm
cd /tmp/shellfirm
git reset --hard
```

## Select your shell
<details>
<summary>Oh My Zsh</summary>
Download zsh plugin:

```sh
curl https://raw.githubusercontent.com/kaplanelad/shellfirm/main/shell-plugins/shellfirm.plugin.oh-my-zsh.zsh --create-dirs -o ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/shellfirm/shellfirm.plugin.zsh
```

Add `shellfirm` to the list of Oh My Zsh plugins when Zsh is loaded(inside ~/.zshrc):

```bash
plugins=(... shellfirm)
```
</details>

<details>
<summary>Bash</summary>
Bash implementation is based on https://github.com/rcaloras/bash-preexec project, which adds a pre-exec hook to catch the command before executing.

```sh
# Download bash-preexec hook functions. 
curl https://raw.githubusercontent.com/rcaloras/bash-preexec/master/bash-preexec.sh -o ~/.bash-preexec.sh

# Source our file at the end of our bash profile (e.g. ~/.bashrc, ~/.profile, or ~/.bash_profile)
echo '[[ -f ~/.bash-preexec.sh ]] && source ~/.bash-preexec.sh' >> ~/.bashrc

# Download shellfirm pre-exec function
curl https://raw.githubusercontent.com/kaplanelad/shellfirm/main/shell-plugins/shellfirm.plugin.sh -o ~/.shellfirm-plugin.sh

# Load pre-exec command on shell initialized
echo 'source ~/.shellfirm-plugin.sh' >> ~/.bashrc
```
</details>

<details>

<summary>fish</summary>


```sh
curl https://raw.githubusercontent.com/kaplanelad/shellfirm/main/shell-plugins/shellfirm.plugin.fish -o ~/.config/fish/conf.d/shellfirm.plugin.fish
```
</details>

<details>
<summary>Zsh</summary>


```sh
# Add shellfirm to conf.d fishshell folder
curl https://raw.githubusercontent.com/kaplanelad/shellfirm/main/shell-plugins/shellfirm.plugin.zsh -o ~/.shellfirm-plugin.sh
echo 'source ~/.shellfirm-plugin.sh' >> ~/.zshrc
```
</details>

<details>
<summary>Docker</summary>

* [bash](./docs/docker/bash)
* [zsh](./docs/docker/zsh)
</details>

:information_source: Open a new shell session

:eyes: :eyes: [Verify installation](./README.md#verify-installation) :eyes: :eyes:

You should get a `shellfirm` prompt challenge. 

**If you didn't get the prompt challenge:**
1. Make sure the `shellfirm --version` returns a valid response.
2. Make sure that you downloaded the Zsh plugin and added it to the Oh My Zsh plugins in .zshrc.

## Risky commands
We have predefined a baseline of risky groups command that will be enabled by default, these are risky commands that might be destructive.

| Group |  Enabled By Default |
| --- | --- |
| [base](./docs/checks/base.md) | `true` |
| [git](./docs/checks/git.md) | `true` |
| [fs](./docs/checks/fs.md) | `true` |
| [fs-strict](./docs/checks/fs-strict.md) | `false` |
| [kubernetes](./docs/checks/kubernetes.md) | `false`  |
| [kubernetes-strict](./docs/checks/kubernetes-strict.md) | `false` |
| [heroku](./docs/checks/heroku.md) | `false` |
| [terraform](./docs/checks/terraform.md) | `false` |


### Add/Remove new group checks
```bash
shellfirm config update-groups
```

## Change challenge:

Currently we support 3 different challenges when a risky command is intercepted:
* `Math` - Default challenge which requires you to solve a math question.
* `Enter` - Required only to press `Enter` to continue.
* `Yes` - Required typing `yes` to continue.

You can change the default challenge by running the command:
```bash
shellfirm config challenge
```

*At any time you can cancel a risky command by hitting `^C`*

## Ignore pattern:

You can disable one or more patterns in a selected group by running the command:
```bash
shellfirm config ignore
```
## Deny pattern command:

Restrict user run command by select pattern id's that you not allow to run in the shell:
```bash
shellfirm config deny
```

## To Upgrade `shellfirm`
```bash
brew upgrade shellfirm
```

## Contributing
Thank you for your interest in contributing! Please refer to [contribution guidelines](./CONTRIBUTING.md) for guidance.

# Copyright
Copyright (c) 2022 [@kaplanelad](https://github.com/kaplanelad). See [LICENSE](LICENSE.txt) for further details.

