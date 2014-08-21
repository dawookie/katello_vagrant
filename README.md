# katello_vagrant

 * https://fedorahosted.org/katello/wiki
 * Lets build a katello server under Vagrant.

1. Adjust your firewall to forward localhost:443 to localhost:9443
  1. (OSX) sudo ipfw add 100 fwd 127.0.0.1,9443 tcp from any to me 443
1. Run 'vagrant up'.

## Bug Reports
 - TBD

## Contributing
 - Fork the repository and email me a pull request.
