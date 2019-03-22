# Laptop

Laptop is a guideline to help you setup an OS X laptop for development.

What it sets up
---------------

* [Homebrew] for package management
* [Homebrew Services] addon to manage services
* [Git] for version control
  * [Git Flow] to manage branch worflow
* [Zsh] a beautiful terminal
* [VirtualBox] to run VMs
* [Docker] to run containers
* [Minikube] to run Kubernetes
* [AWS CLI] to manage AWS
* [Google Cloud SDK] to manage Google Cloud
* [Kubernetic] to manage Kubernetes
* [Kubectx] to switch faster between clusters and namespaces
* [Kube-ps1] to add Kubernetes prompt info on Shell
* [Kubetail] to tail Kubernetes logs
* [Kops] to manage Kubernetes Production Clusters
* [Helm] to manage Kubernetes Charts
* [Z] to track into your most used directories
* [HTTPie] a command line HTTP client with an intuitive UI

It should take less than 15 minutes to install (depends on your machine).

[Homebrew]: https://brew.sh/
[Homebrew Services]: https://github.com/Homebrew/homebrew-services
[Git]: https://git-scm.com/
[Git Flow]: https://github.com/nvie/gitflow
[Zsh]: https://ohmyz.sh/
[VirtualBox]: https://www.virtualbox.org/wiki/Downloads
[Docker]: http://docker.com/
[Minikube]: https://kubernetes.io/docs/setup/minikube/
[AWS CLI]: https://aws.amazon.com/cli/
[Google Cloud SDK]: https://cloud.google.com/sdk/install
[Kubernetic]: https://www.kubernetic.com
[Kubectx]: https://github.com/ahmetb/kubectx
[Kube-ps1]: https://github.com/jonmosco/kube-ps1
[Kubetail]: https://github.com/johanhaleby/kubetail
[Kops]: https://github.com/kubernetes/kops
[Helm]: https://github.com/helm/helm
[Z]: https://github.com/rupa/z
[httpie]: https://httpie.org/

Requirements
------------

We support:

* macOS Mavericks (10.9)
* macOS Yosemite (10.10)
* macOS El Capitan (10.11)
* macOS Sierra (10.12)
* macOS High Sierra (10.13)
* macOS Mojave (10.14)

Older versions may work but aren't regularly tested. Bug reports for older versions are welcome.

Install
-------

Download, review, then execute the script:

```sh
curl --remote-name https://raw.githubusercontent.com/harbur/laptop/master/mac
less mac
sh mac 2>&1 | tee ~/laptop.log
```

Manual Setup
------------

After installing the script here is a list of followup manual installations:

### Git Commitizen

Install [Git Commitizen]:

```sh
npm install commitizen -g
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

[Git Commitizen]: https://github.com/commitizen/cz-cli

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/harbur/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

Inspired by
-----------

This project is inspired by [thoughtbot/laptop]!

[thoughtbot/laptop]: https://github.com/thoughtbot/laptop

Troubleshooting
---------------


If there is an error during Virtualbox installation, check this [blog post].

[blog post]: https://medium.com/@DMeechan/fixing-the-installation-failed-virtualbox-error-on-mac-high-sierra-7c421362b5b5
