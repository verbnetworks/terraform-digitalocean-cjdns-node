# Terraform + Digital Ocean + cjdns

Terraform module to create a cjdns node and push a locally managed `cjdroute.conf`
files once the digitalocean-droplet comes online.
 * [cjdns](https://github.com/cjdelisle/cjdns)
 * [hyperboria](https://hyperboria.net/)
 * [digital ocean](https://www.digitalocean.com/)

## Usage
Establishing up a cjdns node (running v20) on Digital Ocean can be made incredibly easy with this Terraform 
module as shown in the minimal example below:-
```hcl
module "cjdns-node" {
  source = "verbnetworks/cjdns-node/digitalocean"
  hostname = "node0-sfo2-digitalocean"
  region = "sfo2"
  user = "<username>"
  user_sshkey = "<ssh-public-key>"
  cjdroute_config = "${path.cwd}/../../data/cjdroute/node0-sfo2-digitalocean.json"
}
```

You can very easily start a cjdns-crashy release by specifying the git-commit as shown:-
```hcl
module "cjdns-node" {
  source = "verbnetworks/cjdns-node/digitalocean"
  hostname = "node0-sfo2-digitalocean"
  region = "sfo2"
  user = "<username>"
  user_sshkey = "<ssh-public-key>"
  cjdns_commit = "ec3c3126943d7a95d2b1c2fe95f047883e64214b"  # crashy @ 2018-02-03
  cjdroute_config = "${path.cwd}/../../data/cjdroute/node0-sfo2-digitalocean.json"
}
```

NB: the required build cycles can take ~10 minutes to complete, do a `tail -f /var/log/cloud-init-output.log` 
to watch the progress - you will recognise when everything is complete by the ascii-art hostname when ssh'ing 
into the host.

The module provides a range of other input-variables and useful outputs to tweak your digitalocean-droplet as required.

## Input Variables - Required

### hostname
The hostname applied to this cjdns-node droplet.

### region
The digitalocean region to start this cjdns-node within.

### user
The user initial login user to create with passwordless sudo access for this cjdns-node, if empty no account will be 
created. The root account is always disabled.

### user_sshkey
The sshkey to apply to the initial user account - password based auth is always disabled.

### cjdroute_config
The local `cjdroute.conf` file to push to this cjdns-node droplet.


## Input Variables - Required with defaults already set

### cjdns_commit
The git-commit sha1 to download from github.com/cjdelisle/cjdns then build and install on this cjdns-node. The default 
is currently [ec3c3126943d7a95d2b1c2fe95f047883e64214b](https://github.com/cjdelisle/cjdns/tree/ec3c3126943d7a95d2b1c2fe95f047883e64214b)
which represents v20, if you wish to use a more recent [crashy](https://github.com/cjdelisle/cjdns/tree/crashey) 
commit you can specify it here.
- Default: "efd7d7f82be405fe47f6806b6cc9c0043885bc2e"

Recent (as at 2018-02) commits and versions are listed here for reference:-
https://github.com/cjdelisle/cjdns/releases
 - crashy = ec3c3126943d7a95d2b1c2fe95f047883e64214b @ 2018-02-03
 - crashy = 0a08d0812976548ce12db9d80a9c0033fb8a14fc @ 2018-01-10
 - v20    = efd7d7f82be405fe47f6806b6cc9c0043885bc2e
 - v19.1  = f4f73cdc120907f9952f7c74abe68394fd2879a0
 - v19    = 63fdd890d7b9903e386ae094fe4ace548d3988f6
 - v18    = 6781eddb2b206da6d9e14fa79fab507c9f154acf

### image
The digitalocean image to use as the base for this cjdns-node.
 - Default: "ubuntu-16-04-x64"

### size
The digitalocean droplet size to use for this cjdns-node.
 - Default: "512mb"

### backups
Enable/disable droplet backup functionality on this cjdns-node.
 - Default: false

### monitoring
Enable/disable droplet monitoring functionality on this cjdns-node.
 - Default: true

### ipv6
Enable/disable getting a public IPv6 on this digitalocean-droplet.
 - Default: true

### private_networking
Enable/disable digitalocean private-networking functionality on this cjdns-node.
 - Default: true


## Input Variables - Optional

### ipfs_version
If set will additionally install IPFS on this cjdns-node, without any config!  This is provided as a convenience only 
since IPFS is generally a useful use-case for cjdns-nodes to participate in.  IPFS versions can be found 
[https://dist.ipfs.io/go-ipfs/versions](https://dist.ipfs.io/go-ipfs/versions)
 - Default: ""


## Outputs

### hostname
The hostname given to this cjdns-node.

### region
The digitalocean region this cjdns-node is within

### user
The user initial login user created with passwordless sudo access on this cjdns-node if set.

### cjdroute_config
The local cjdroute.conf file pused to this cjdns-node droplet.

### cjdns_commit
The cjdns git commit used to download, build and install on this cjdns-node.

### ipfs_version
The IPFS version used to download and install on this cjdns-node.

### ipv4_address
The public digitalocean-droplet IPv4 address of this cjdns-node.

### ipv6_address
The public digitalocean-droplet IPv6 address of this cjdns-node.


## Authors
Module managed by [Verb Networks](https://github.com/verbnetworks)

## License
Apache 2 Licensed. See LICENSE for full details.
