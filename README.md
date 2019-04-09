# IKEv2 Tamarin Model

The [Tamarin](https://tamarin-prover.github.io/) prover is a verification tool
for security protocols. This project contains a symbolic model of the IKEv2
protocol which can be used with Tamarin to verify the key exchanges claimed
security properties.

## How do I run this?

First you need to obtain the tamarin-prover binary from the package manager
of your choice:
* Arch Linux: ` pacman -S tamarin-prover`
* Nixpkgs: `nix-env -i tamarin-prover`

More infos [here](https://tamarin-prover.github.io/manual/book/002_installation.html)

Then run the model via the command line tool:

	tamarin-prover interactive ikev2.spthy

And point your browser to:

	http://localhost:3001

