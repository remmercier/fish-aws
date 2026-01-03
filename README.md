# fish-aws

A port of some nice features of [AWS plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/aws/README.md) of Oh My Zsh framework.
The main work of translation from zsh to fish has been made by Claude. The result has been approved and tested by me.
The usage and configuration part of this documentation come from the original documentation of the Oh My Zsh [AWS plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/aws/README.md).

## Installation

Install using [fisher](https://github.com/jorgebucaran/fisher):
```fish
fisher install remmercier/fish-aws
```

## Usage

* `asp [<profile>]`: sets `$AWS_PROFILE` and `$AWS_DEFAULT_PROFILE` (legacy) to `<profile>`.
  It also sets `$AWS_EB_PROFILE` to `<profile>` for the Elastic Beanstalk CLI. It sets `$AWS_PROFILE_REGION` for display in `aws_prompt_info`.
  Run `asp` without arguments to clear the profile.
* `asp [<profile>] login`: If AWS SSO has been configured in your aws profile, it will run the `aws sso login` command following profile selection.
* `asp [<profile>] login [<sso_session>]`: In addition to `asp [<profile>] login`, if SSO session has been configured in your aws profile, it will run the `aws sso login --sso-session <sso_session>` command following profile selection.
* `asp [<profile>] logout`: If AWS SSO has been configured in your aws profile, it will run the `aws sso logout` command following profile selection.

* `asr [<region>]`: sets `$AWS_REGION` and `$AWS_DEFAULT_REGION` (legacy) to `<region>`.
  Run `asr` without arguments to clear the profile.

* `acp [<profile>] [<mfa_token>]`: in addition to `asp` functionality, it actually changes
   the profile by assuming the role specified in the `<profile>` configuration. It supports
   MFA and sets `$AWS_ACCESS_KEY_ID`, `$AWS_SECRET_ACCESS_KEY` and `$AWS_SESSION_TOKEN`, if
   obtained. It requires the roles to be configured as per the
   [official guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html).
   Run `acp` without arguments to clear the profile.

* `agp`: gets the current value of `$AWS_PROFILE`.

* `agr`: gets the current value of `$AWS_REGION`.

* `aws_profiles`: lists the available profiles in the  `$AWS_CONFIG_FILE` (default: `~/.aws/config`).
  Used to provide completion for the `asp` function.

* `aws_regions`: lists the available regions.
  Used to provide completion for the `asr` function.

## Configuration

[Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) by AWS

### Scenario: IAM roles with a source profile and MFA authentication

Source profile credentials in `~/.aws/credentials`:

```ini
[source-profile-name]
aws_access_key_id = ...
aws_secret_access_key = ...
```

Role configuration in `~/.aws/config`:

```ini
[profile source-profile-name]
mfa_serial = arn:aws:iam::111111111111:mfa/myuser
region = us-east-1
output = json

[profile profile-with-role]
role_arn = arn:aws:iam::9999999999999:role/myrole
mfa_serial = arn:aws:iam::111111111111:mfa/myuser
source_profile = source-profile-name
region = us-east-1
output = json
```

# Similar projects

* [anakaiti/fish-aws-profile-switcher: Fish shell plugin to switch between AWS profiles](https://github.com/anakaiti/fish-aws-profile-switcher)
* [AWS profile switcher for fish shell](https://gist.github.com/ariksidney/c981938e37052335d8305746568474c9)
* [shidil/awsctx: Shell script to switch between aws cli profiles using fzf and trigger sso login if unauthenticated](https://github.com/shidil/awsctx)

I made this project cause most of existing projects listed above do not handle the login process.
