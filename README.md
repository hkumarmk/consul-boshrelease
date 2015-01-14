BOSH release for consul
=======================

Use this BOSH release to either:

- deploy a cluster of consul servers; OR
- upgrade an existing BOSH deployment to advertise or discover services

The [redis-boshrelease](https://github.com/cloudfoundry-community/redis-boshrelease) is an example BOSH release that can use this consul release to advertise itself to other consul consumers.

Installation
------------

This information was changed because of the switch to a new consul version.
Right now the consul tarball is downloaded via the update script.

```
$> git clone https://github.com/cloudfoundry-community/consul-boshrelease.git
$> cd consul-boshrelease
$> ./update
$> bosh create release --force
$> bosh upload release
```
Usage
-----

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a 3-node cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a 3-node cluster:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

```yaml
---
networks:
  - name: consul1
    type: dynamic
    cloud_properties:
      security_groups:
        - consul
```

Where `- consul` means you wish to use an existing security group called `consul`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```
