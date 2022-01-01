# Ansible role "papanito.crowdsec" <!-- omit in toc -->

[![Ansible Role](https://img.shields.io/ansible/role/57402)](https://galaxy.ansible.com/papanito/cloudflared) [![Ansible Quality Score](https://img.shields.io/ansible/quality/57402)](https://galaxy.ansible.com/papanito/cloudflared) [![Ansible Role](https://img.shields.io/ansible/role/d/57402)](https://galaxy.ansible.com/papanito/cloudflared) [![GitHub issues](https://img.shields.io/github/issues/papanito/ansible-role-crowdsec)](https://github.com/papanito/ansible-role-crowdsec/issues) [![GitHub pull requests](https://img.shields.io/github/issues-pr/papanito/ansible-role-crowdsec)](https://github.com/papanito/ansible-role-crowdsec/pulls)


Ansible role to install [crowdsec][crowdsec-git] or specifically the [crowdsec-agent][crowdsec-git] and [bouncers][crowdsec-hub]

- [Crowdsec-agent][crowdsec-git] is an open-source and lightweight software that allows you to detect peers with malevolent behaviors and block them from accessing your systems at various level (infrastructural, system, applicative)
- [Bouncers][bouncers] are standalone software pieces in charge of acting upon a decision taken by crowdsec : block an IP, present a captcha, enforce MFA on a given user, etc.

In addition it let's you install (and remove)

- **TBI** [Collections][collections] are bundle of [parsers], [scenarios], [postoverflows] that form a coherent package and are present in `/etc/crowdsec/collections/`

## Role Details

### Installation

There are [different installation options][installation] and the role will do the following

- on debian or rhel systems it installs the package from the repo
- for all others, it installs it from [tarball](https://doc.crowdsec.net/Crowdsec/v1/getting_started/installation/#using-the-unattended-wizard)

> Tarball installtion does currently not work. I plan to enable this asap, but this requires to consider a proper update mechanism (see open issues)

## Requirements

N/A

## Role Variables

There are common role variables and service specific ones. Most variables should be fine (as tested). Thus it's recommended to only define these variables

| Parameter          | Description                                                                   | Default Value |
| ------------------ | ----------------------------------------------------------------------------- | ------------- |
| `cs_install_agent` | Whether to install the crowdsec agent                                         | `true`        |
| `cs_agent_version` | Version of the crowdsec agent to install                                      | `v1.0.13`     |
| `cs_bouncers`      | Dictionary of bouncers to be installed                                        | `-`           |
| `cs_console_token` | Token to register instance in [Crowdstrike Console](https://app.crowdsec.net) | `-`           |

### Bouncers

The installation process for [bouncers] differs. Thus

- for debian or rhel install the bouncer from the repo (if awailable)
- for any other case install from tarball, which are usually installed as follows

   ```bash
   $ tar xzvf cs-firewall-bouncer.tgz
   $ sudo ./install.sh
   ```

One has to define `cf_bouncers` which contains the bouncer name and the version to be installed:

```yaml
cs_bouncers:
  cs-firewall-bouncer: ## bouncer name
     version: v0.0.12  ## bouncer version
```

The following bouncers are supported:

- [cs-firewall-bouncer](https://hub.crowdsec.net/author/crowdsecurity/bouncers/cs-firewall-bouncer)
- [cs-nginx-bouncer](https:/hub.crowdsec.net/author/crowdsecurity/bouncers/cs-nginx-bouncer)
<!-- [cs-custom-bouncer](https://hub.crowdsec.net/author/crowdsecurity/bouncers/cs-custom-bouncer) -->

If your are missing something, please check the open issues and create one if necessary.
## Dependencies

n/a

## Testing

I use [Hetzner Cloud](https://console.hetzner.cloud) for testing hence you can use this play

```bash
ansible-playbook tests/test.install.both.yml -e cs_console_token=XXXX
```

For this to work, export `HCLOUD_TOKEN`.

## Example Playbook

The following playbook installs the crowdsec agent with version `cf_agent_version` and the [bouncer][bounvers] `cs-firewall-bouncer` with version `v0.0.12`

```yml
- name: Install crowdsec agentd
  hosts: "servers"
  vars:
    bouncers:
      cs-firewall-bouncer:
        version: v0.0.12

  roles:
     - papanito.crowdsec
```

## License

This is Free Software, released under the terms of the Apache v2 license.

## Author Info

Written by [Papanito](https://wyssmann.com) - [Gitlab](https://gitlab.com/papanito) / [Github](https://github.com/papanito)


[crowdsec-doc]: https://doc.crowdsec.net/
[crowdsec-git]: https://github.com/crowdsecurity/crowdsec
[crowdsec-hub]: https://hub.crowdsec.net/
[installation]: https://doc.crowdsec.net/Crowdsec/v1/getting_started/installation/#
[csli alerts]: https://docs.crowdsec.net/docs/cscli/cscli_alerts/
[bouncers]: https://docs.crowdsec.net/docs/bouncers/intro/
[cscli bouncers]: https://docs.crowdsec.net/docs/cscli/cscli_bouncers/
[collections]: https://docs.crowdsec.net/docs/collections/intro
[cscli collections]: https://docs.crowdsec.net/docs/cscli/cscli_collections/
[parsers]: https://docs.crowdsec.net/docs/parsers/intro/
[cscli parsers]: https://docs.crowdsec.net/docs/cscli/cscli_parsers/
[decisions]: https://docs.crowdsec.net/docs/decisions/intro/
[Decision object documentation]: https://pkg.go.dev/github.com/crowdsecurity/crowdsec/pkg/models#Decision
[cscli decisions]: https://docs.crowdsec.net/docs/cscli/cscli_decisions/
[postoverflows]: https://docs.crowdsec.net/docs/parsers/intro/#postoverflows
[cscli postoverflows]: https://docs.crowdsec.net/docs/cscli/cscli_postoverflows/