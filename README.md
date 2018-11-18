# ansible-provisioned-ubuntu16

## 1. Install role

```sh
$ ansible-galaxy install --roles-path=.galaxy_roles geerlingguy.docker
$ ansible-galaxy install --roles-path=.galaxy_roles geerlingguy.pip
$ ansible-galaxy install --roles-path=.galaxy_roles geerlingguy.ruby
$ ansible-galaxy install --roles-path=.galaxy_roles Stouts.python 
```

## 2. hosts.yml example

```yml
all:
  hosts:
    pc1:
      ansible_port: 22
      ansible_host: 10.251.12.101
      ansible_user: user1
    pc2:
      ansible_port: 22
      ansible_host: 10.251.12.102
      ansible_user: user2
```

### 3. Extra variable
To parameterize some configure, you can provide extra variables as
`--extra-vars` option.

```sh
$ ansible-playbook site.yml -i hosts.yml -k -K --extra-vars=@.extraValue.yml
```

All extra variable examples are below:
```yml
# .extraValue.yml (is gitignored)
EXTRA_VAR_UPDATE_VIM_PLUGIN: true                         # default: false
EXTRA_VAR_PROXY_SERVER_URL: http://123.123.123.123:8080/
EXTRA_VAR_NO_PROXY_SERVER_URL: https://private.company.gitlab.com/
EXTRA_VAR_CA_CERT: |
    -----BEGIN CERTIFICATE-----
    MIIEzTCCA7WgAwIBAgIMVWI2fkPCMs08osgOMA0GCSqGSIb3DQEBCwUAMEwxCzAJ
    BgNVBAYTAkJFMRkwFwYDVQQKExBHbG9iYWxTaWduIG52LXNhMSIwIAYDVQQDExlB
    bHBoYVNTTCBDQSAtIFNIQTI1NiAtIEcyMB4XDTE2MDcyNDA0MzI1NloXDTE3MDcy
    NTA0MzI1NlowNzEhMB8GA1UECxMYRG9tYWluIENvbnRyb2wgVmFsaWRhdGVkMRIw
    EAYDVQQDDAkqLmRldnAubWUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
    AQC+aAHKrXRzb+Dey87VfwdrIp699Cdf/bQJqNUkL3auZUQaFrZh/7KlB9HgBNJ/
    EK7drxW8teR3Zpz/WHxz+5e4aX60vXucdfiyIg7DPHX8nxEoFYD9W9CHlnGAXQcB
    bvwvGW5eMx5lxFh6BQEmiblKii3y0/w1duU2A6RS3eIkN7QZZIzAN+mhYS35+rDH
    dAYEWIh9HGM4LSs0RbpYA8x88TdoJ/SAFHsaaoqkkkkzW2EKr8xztbo0UAdaV3sa
    +IA9HLOnNnjWrABy42zADShM2dObnNS6zcYXegZS+jxT2t7m0iIFQBdbIOymzye9
    9gxYys+LdKV3fpvsW9PPrHFLAgMBAAGjggHCMIIBvjAOBgNVHQ8BAf8EBAMCBaAw
    yXvQdjBielj8eWa3RojGApedMrJhjdt4GBHh4fFc2Vty
    -----END CERTIFICATE-----
ansible_python_interpreter: /usr/bin/python3              # when vagrant
```

## 4. Excute

```sh
$ ansible-playbook site.yml -i hosts.yml -k -K -vv
$ ansible-playbook site.yml -i hosts.yml -k -K -t git
```


## 5. Make Vagrant box file

```
$ vagrant up

## package 하기 전에 git tag 를 따고 box 파일이름뒤에 `-tag` 를 붙인다.
## 예를들어 태그가 `v2` 일때,
$ git tag v2 -m "some update"
$ vagrant package --output vagrant-ubuntu16-v2.box
$ vagrant destroy
```

## Tip1. yaml debugging

```sh
$ yarn
$ npx js-yaml tasks-vim.yml
```

## Tip2. proxy
**Vagrant** box download 시, plugin download 시 proxy setting 예제

```sh
export SSL_CERT_FILE=company.crt
export CURL_CA_BUNDLE=company.crt
export HTTP_PROXY=http://123.123.123.123:8080/
export HTTPS_PROXY=http://123.123.123.123:8080/
export http_proxy=http://123.123.123.123:8080/
export https_proxy=http://123.123.123.123:8080/
```

## Tip2. sshpass on MACOS

```sh
$ brew install hudochenkov/sshpass/sshpass
```

## Tip3. ansible examples

```
$ ansible -i hosts.yml -m setup -a 'gather_subset=!facter,!ohai filter=ansible_distribution*' all
```

- https://github.com/hudochenkov/homebrew-sshpass

