# Vault secrets - Hass.io add-on for Home Assistant

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

![Supports armhf Architecture][armhf-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports i386 Architecture][i386-shield]

[![GitHub Activity][commits-shield]][commits]

## About

This add-on allows you to retrieve secrets from a remote Vault service.
[Vault](https://vaultproject.io) is a tool from [Hashicorp](https://hashicorp.com). It allows you to secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.

## Installation

Before using this add-on make sure you have created a Vault token with the correct policy.

Create the following policy:

```hcl
path "secret/hassio/*" {
  capabilities = ["read"]
}
```

Add the policy to Vault service:

```bash
vault policy write hassio ./hassio_policy.hcl
```

To able to access the Vault service, create a Vault token voor Home Assistant.

```bash
vault token create -ttl=5m -policy=hassio -orphan
```

**NOTE: Vault doesn't support blocking queries as a result changes are not
instantly loaded into the secret file, but half the ttl value. So when a new
token is created with ttl of 5 minutes it will check every 2,5 minutes.
As such, a smaller ttl will force the addon to update more often.**

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```json
{
  "ssl": true,
  "cafile": "cacert.pem",
  "certfile": "fullchain.pem",
  "keyfile": "privkey.pem",
  "vault_addr": "https://vault.local:8200",
  "vault_token": "s.SZ2ygzuFDeitqTclwptvaqxI",
  "vault_retry_attempts": 5,
  "vault_retry_backoff": "250ms",
  "log_level": "info"
}
```

**Note**: _This is just an example, don't copy and paste it! Create your own!_

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `warning`: Exceptional occurrences that are not errors.
- `error`:  Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

### Option: `ssl`

Enables/Disables SSL on the control panel. Set it `true` to enable it,
`false` otherwise.

### Option: `certfile`

The certificate file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is default for Hass.io._

### Option: `keyfile`

The private key file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is default for Hass.io._

### Option: `vault_addr`

This is the address where the Vault service is located. By default it uses
port `8200` but if you have a frontent proxy or a different port make sure
you add the correct port number.

### Option: `vault_token`

This is the actual token that is used to contact the Vault service. You need
to have a valid token, otherwise you will get permission denies.

Vault token can be created as explaint under [Installation](https://github.com/rgruyters/addon-vault-secrets#installation)

### Option: `vault_retry_attempts`

This specifies the number of attempts to make before giving up.
Each attempt adds the exponential backoff sleep time. Setting this to zero
will implement an unlimited number of retries.

### Option: `vault_retry_backoff`

This is the base amount of time to sleep between retry attempts. Each
retry sleeps for an exponent of 2 longer than this base. For 5 retries,
the sleep times would be: 250ms, 500ms, 1s, 2s, then 4s.

## License

MIT License

Copyright (c) 2019 Robin Gruyters

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[commits-shield]: https://img.shields.io/github/commit-activity/y/rgruyters/addon-vault-secrets.svg
[commits]: https://github.com/rgruyters/addon-vault-secrets/commits/master
[contributors]: https://github.com/rgruyters/addon-vault-secrets/graphs/contributors
[home-assistant]: https://home-assistant.io
[issue]: https://github.com/rgruyters/addon-vault-secrets/issues
[keepchangelog]: http://keepachangelog.com/en/1.0.0/
[license-shield]: https://img.shields.io/github/license/rgruyters/addon-vault-secrets.svg
[releases-shield]: https://img.shields.io/github/release/rgruyters/addon-vault-secrets.svg
[releases]: https://github.com/rgruyters/addon-vault-secrets/releases
[semver]: http://semver.org/spec/v2.0.0.htm
[project-stage-shield]: https://img.shields.io/badge/project%20stage-experimental-yellow.svg
