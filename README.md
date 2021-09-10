# IKEv2 Tamarin Model

The [Tamarin](https://tamarin-prover.github.io/) prover is a verification tool
for security protocols. This project contains a symbolic model of the Minimal IKEv2
protocol which can be used with Tamarin to verify the key exchanges' claimed
security properties.

There are two models:
IKEv2 in a standard setting and "PQ-IKEv2" in a quantum setting, that means [Minimal IKEv2](https://datatracker.ietf.org/doc/html/rfc7815) with the [IKE_INTERMEDIATE-extension](https://datatracker.ietf.org/doc/html/draft-ietf-ipsecme-ikev2-intermediate-03) and with an attacker who can break the Diffie-Hellman keys derived in the standard handshake.

## How do I run this?

First you need to obtain the tamarin-prover binary from the package manager
of your choice:
* Arch Linux: `pacman -S tamarin-prover`
* Nixpkgs: `nix-env -i tamarin-prover`

More infos [here](https://tamarin-prover.github.io/manual/book/002_installation.html)

Then run the model via the command line tool:

	tamarin-prover interactive ikev2.spthy
or:

	tamarin-prover interactive pq-ikev2.spthy

And point your browser to:

	http://localhost:3001

### Hints for installing Tamarin on Ubuntu
We used Ubuntu 20.04, as 18.04 did not support the glibc 2.29, which is required by Tamarin.
Tamarin das not provide a package for Ubuntu/Debian by using apt/dpkg, hence we installed tamrin with the homebrew package manager

#### Installing Hombrew
If the package manager includes homebrew:
	sudo apt install linuxbrew-wrapper
Otherwise manual installation is required (as it is currently the case in Ubuntu 20.04 LTS):
	sudo apt install build-essential curl file git
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
	sudo brew install tamarin-prover/tap/tamarin-prover
	test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
	test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
	test -r ~/.bash_profile && echo eval" ($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
	echo "eval $($(brew --prefix)/bin/brew shellenv)" >>~/.profile
	sudo brew install tamarin-prover/tap/tamarin-prover
	echo 'eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"' >> /home/ubuntu/.profile
	eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"
	brew install gcc

#### Installing Tamarin and its dependencies
	brew install tamarin-prover/tap/tamarin-prover
	brew install tamarin-prover/tap/maude graphviz haskell-stack
   	brew install font-util
	export PATH=$PATH:$HOME/.linuxbrew/var/homebrew/linked/tamarin-prover/bin
	tamarin-prover

## Environment

The code ran in an virtual environment with 40 vCPU cores, 180 GB RAM and with standard Ubuntu 20.04. It took about 30 mins to complete.

Smaller setups may work; the bottleneck is memory, not CPU power.

## Results

Depending on whether a lemma makes an "exists trace"-statement or an "all traces"-statements, different results are considered a success.
To prove an "exists trace"-statement, Tamarin will output a trace (as a picture) which shows a fulfilling trace for the lemma.
Tamarin succeeds at proving an "all traces"-statement if no trace can be found that contradicts the statement.
