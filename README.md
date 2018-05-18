 # RockNSM Suricata Rules

 This repository is designed to facilitate a service that allows organizations
 to use git for change control of their Suricata rules. It also contains
 configuration to use `suricata-update` for rule.

 The structure is as follows:

```
rock-suricata/
├── disable.conf
├── enable.conf
├── modify.conf
├── README.md
├── rules
│   └── eicar.rules
└── site_vars.yaml
```

## Suricata Update Config

All the `*.conf` files modify the behavior of `suricata-update` and
are documented in the [suricata-update docs](https://suricata-update.readthedocs.io/en/latest/update.html).
The path to these files needs to be added to the main configuration file
located at `/etc/suricata/update.conf`.

In RockNSM, this entire directory will be located at `/var/lib/suricata/site/`
and the `update.conf` will be configured to reflect that.

## Rules

The `rules/` directory contains all your local site or sensor specific rules.
This is currently empty except for a single `eicar` rule that looks for a
test malicious document. It is disabled by default. Any file matching the glob
`./rules/*.rules` will be included when `suricata-update` runs, according
to the `*.conf` file tweaks.

## Site Variables

The `site_vars.yaml` file is intended to provide a means to override the default
variables and inject new ones that can be leveraged in your signatures. For this
to work, it must be included in the main Suricata configuration file
(`suricata.yaml`).

## Rationale and Workflow

When managing a large number of sensors, or even if you just like to practice
good sensor hygiene, it's useful to use `git` as a source control/change
control. Namely, GitHub and similar platforms allow for a nice git workflow
where users (in this case, the analysts) can branch off of production, make
edits, and submit for inclusion into production. Add some automated testing to
make sure it doesn't blow up your sensors, and you've got the dream.

The rationale is we can use the de-facto standard `suricata-update` by cloning
our repo to the locale filesystem. We can then configure `suricata-update` to
merge in those changes as a "local" ruleset.

This repository is just to outline the structure of the sensor-specific rules
and configuration.

## Contributing

The RockNSM project loves contributors! If you'd like to contribute useful
signatures not commonly provided by other rulesets, feel free to submit a
pull request!
