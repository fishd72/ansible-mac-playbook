<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac deployment Ansible Playbook


This playbook installs and configures most of the software I use on my Mac and is mostly code re-used from Jeff Geerling's [mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook). My modified version does most of the heavy lifting automatically but I'll be adding more things as I can think of them.

As such, this is very much a work in progress, and is mostly a means for me to document my current Mac's setup as well as learn Ansible. I'll be evolving this set of playbooks over time.

*See also*:

  - [mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) (the original inspiration for this project)
  - [Boxen](https://github.com/boxen)
  - [Battleschool](http://spencer.gibb.us/blog/2014/02/03/introducing-battleschool)
  - [osxc](https://github.com/osxc)
  - [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. [Install Ansible](http://docs.ansible.com/intro_installation.html).
  3. Sign into (at least) the Apple App Store (issue#)
  4. Clone this repository to your local drive.
  5. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  6. Run `ansible-playbook main.yml -i inventory -K` inside this directory. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's environment and preferred software configuration is the same. This playbook has been fully customised to set up my Mac the way *I* like it, you may differ, and that's ok.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - cowsay
      - git
      - go
    
    mas_installed_apps:
      - { id: 443987910, name: "1Password" }
      - { id: 498486288, name: "Quick Resizer" }
      - { id: 557168941, name: "Tweetbot" }
      - { id: 497799835, name: "Xcode" }
    
    composer_packages:
      - name: hirak/prestissimo
      - name: drush/drush
        version: '^8.1'
    
    gem_packages:
      - name: bundler
        state: latest
    
    npm_packages:
      - name: webpack
    
    pip_packages:
      - name: mkdocs

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

  - appcleaner
  - autodmg
  - balenaetcher
  - caffeine
  - cheatsheet
  - coconutbattery
  - cyberduck
  - Dosbox
  - evernote
  - firefox
  - handbrake
  - launchcontrol
  - Microsoft-teams
  - OpenEmu
  - ripit
  - Signal
  - slack
  - Steam
  - suspicious-package
  - transmission
  - twitch
  - vagrant
  - visual-studio-code
  - vmware-fusion
  - Vuescan
  - 1password

Packages (installed with Homebrew):

  - httpie
  - mas
  - packer
  - qemu
  - http://git.io/sshpass.rb
  - zsh-history-substring-search
  
More Packages (installed via the App Store):

- Apple Configurator 2
- DaisyDisk
- Disk Speed Test
- Evernote Web Clipper
- Geekbench 4
- iFlicks 2
- Keynote
- Microsoft Remote Desktop
- MindNode
- Numbers
- Pages
- Paprika Recipe Manager
- PiPifier
- Pixelmator
- The Unarchiver
- Trello
- Tweetbot
- WiFi Explorer

VSCode Extensions installed:

 - bbenoist.vagrant
 - DavidAnson.vscode-markdownlint
 - esbenp.prettier-vscode
 - marcostazi.VS-code-vagrantfile
 - mrmlnc.vscode-duplicate
 - ms-python.python
 - vscoss.vscode-ansible

I also apply some custom config to VSCode using a J2 template, it's quite basic as I've only recently transitioned from Atom but I'll be extending it as I go.

My [dotfiles](https://github.com/fishd72/dotfiles) are also installed into the current user's home directory, including custom *gitignore*, *gitconfig* as well as configuration for both bash and zsh shells. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, I tweak a fair few macOS settings (using [code from a PR](https://github.com/geerlingguy/mac-dev-playbook/pull/79) [CherryKitten](https://github.com/CherryKitten) raised on the original playbook), do some Dock reconfiguration, clone some git repositories that I'm currently working on as well as add an extension for zsh, and finally customise Terminal with a custom theme.


## Future additions

### Things that still need to be done manually

I'm still testing, but I want to look further into the config of some of the apps I install, if I can figure out what default config I need, I'll try to include it.

### Applications/packages to be added:

  - Synology Drive - I did have this working via homebrew cask, however it seems they've changed the file permissions as downloads now fail.
  - tbc

### Configuration to be added:

  - tbc

## Testing the Playbook

Like Jeff, I've tested this mainly in a VMware Fusion virtual machine. I'll look into getting some documentation up here, but it's not terribly complicated.

Automated testing is next on the to-do list... I've been using this playbook, along with Jeff's excellent [youtube series](https://www.youtube.com/playlist?list=PL2_OBreMn7FplshFCWYlaN2uS8et9RjNG) to learn Ansible, so next I need to learn how to automate the testing of playbooks.

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

[Dave Fisher](https://github.com/fishd72), 2020 (originally inspired by [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)).
