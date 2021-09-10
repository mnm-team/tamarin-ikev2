# IKEv2 Tamarin Model

The [Tamarin](https://tamarin-prover.github.io/) prover is a verification tool
for security protocols. (Please also refer to the [Tamarin documentation](https://tamarin-prover.github.io/manual/tex/tamarin-manual.pdf) 
). 

This project contains a symbolic model of the Minimal IKEv2
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
	echo 'eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"' >> /home/ubuntu/.profile
	eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"
	
#### Installing Tamarin and its dependencies
	brew install gcc
	brew install font-util
	brew install tamarin-prover/tap/tamarin-prover
	brew install tamarin-prover/tap/maude graphviz haskell-stack
   	export PATH=$PATH:$HOME/.linuxbrew/var/homebrew/linked/tamarin-prover/bin
	tamarin-prover

## Environment

The code ran in an virtual environment with 40 vCPU cores, 180 GB RAM and with standard Ubuntu 20.04. It took about 30 mins to complete.

Smaller setups may work; the bottleneck is memory, not CPU power.

## Versions of the model

We model both standard IKEv2 and "PQ-IKEv2". PQ-IKEv2 here means IKEv2 with IKE-Intermediate in a model, where the attacker can break classical DH-keys.

The files that relate to the second kind have the prefix "pq-".

The "full model" versions contain models that consider the aliveness-, weak-agreement- and agreement-property not only from the initiator's point of view (as do the models in [ikev2.spthy](https://github.com/mnm-team/tamarin-ikev2/blob/master/ikev2.spthy) and in [pq-ikev2.spthy](https://github.com/mnm-team/tamarin-ikev2/blob/master/pq-ikev2.spthy)).

We provide the "running-neq-completed" versions to account for the ambiguity in the definition of aliveness, weak agreement and agreement, which require one of the parties to have "completed" the protocol and the other to have "run" the protocol. 
In the other versions, "run" is interpreted as "completed"; in the "running-neq-completed" versions, the properties are defined in such a way that aliveness and weak agreement only require the second party to reach any step of the protocol, not to complete it.

## Results

To check the model, choose the correct file in the Tamarin GUI. In the tab that opens, the model code is on the left. 

Click "sorry" at the foot of whichever lemma you want to prove. This opens the "Visualization display". All lemmas can be checked by clicking "a. autoprove".

Depending on whether a lemma makes an "exists trace"-statement or an "all traces"-statements, different results are considered a success.
To prove an "exists trace"-statement, Tamarin will output a trace (as a picture) which shows a fulfilling trace for the lemma.
Tamarin succeeds at proving an "all traces"-statement if no trace can be found that contradicts the statement.

Lemmas that can be proven yield a green trace on the left and, in the case of "exists-trace"-lemmas, a picture of a fulfilling trace.
Lemmas that can be disproven yield a red trace on the left and, in the case of "all-traces"-lemmas, a picture of a contradicting trace.
