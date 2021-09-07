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

## Environment

The code ran in an virtual environment with 40 vCPUs cores, 180 GB RAM and with standard Ubuntu. It took about 30 mins to complete.

Smaller setups may also work.
