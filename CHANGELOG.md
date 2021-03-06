## v0.9.2 - 2018-08-20 - More sources

* EtcHostsProvider implementation to create static/emergency best effort
  content that can be used in /etc/hosts to resolve things.
* Add lenient support to Zone.add_record, allows populate from providers that
  have allowed/created invalid data and situations where a sub-zone is being
  extracted from a parent, but the records still exist in the remote provider.
* AXFR source support added
* google-cloud-dns requirement instead of general package

## v0.9.1 - 2018-05-21 - Going backwards with setup.py

### NOTICE

Using this version on existing records with `geo` will result in
recreating all health checks. This process has been tested pretty thoroughly to
try and ensure a seemless upgrade without any traffic shifting around. It's
probably best to take extra care when updating and to try and make sure that
all health checks are passing before the first sync with `--doit`. See
[#67](https://github.com/github/octodns/pull/67) for more information.

* Major update to geo healthchecks to allow configuring host (header), path,
  protocol, and port [#67](https://github.com/github/octodns/pull/67)
* SSHFP algorithm type 4
* NS1 and DNSimple support skipping unsupported record types
* Revert back to old style setup.py &amp; requirements.txt, setup.cfg was
  causing too much pita

## v0.9.0 - 2018-03-26 - Way too long since we last met

* Way way way too much to list out here, shouldn't have waited so long
* Initial NS1 geo support
* Major reworking of `CloudflareProvider`'s update process, was only partially
  functional before, also ignore proxied records
* Fixes and improvements to better support non-ascii records and zones
* Plans indicate when Zones are going to be created
* Fix for `GoogleCloudProvider` handling of ; escapes
* Skip Alias recordsets for Route53 (unsupported concept/type)
* Make sure that Record geo values are sorted to prevent false diffs that can
  never be fixed
* `DynProvider` fix to safely roll rulesets, things could end up on rules
  without a pool and/or hitting the default rule previously.

## v0.8.8 - 2017-10-24 - Google Cloud DNS, Large TXT Record support

* Added support for "chunking" TXT records where individual values were larger
  than 255 chars. This is common with DKIM records involving multiple
  providers.
* Added `GoogleCloudProvider`
* Configurable `UnsafePlan` thresholds to allow modification of how many
  updates/deletes are allowed before a plan is declared dangerous.
* Manager.dump bug fix around empty zones.
* Prefer use of `.` over `source` in shell scripts
* `DynProvider` warns when it ignores unrecognized traffic directors.

## v0.8.7 - 2017-09-29 - OVH support

Adds an OVH provider.

## v0.8.6 - 2017-09-06 - CAA record type,

Misc fixes and improvments.

* Azure TXT record fix
* PowerDNS api support for https
* Configurable Route53 max retries and max-attempts
* Improved key ordering error message

## v0.8.5 - 2017-07-21 - Azure, NS1 escaping, & large zones

Relatively small delta this go around. No major themes or anything, just steady
progress.

* AzureProvider added thanks to work by
  [Heesu Hwang](https://github.com/h-hwang).
* Fixed some escaping issues with NS1 TXT and SPF records that were tracked down
  with the help of [Blake Stoddard](https://github.com/blakestoddard).
* Some tweaks were made to Zone.records to vastly improve handling of zones with
  very large numbers of records, no more O(N^2).

## v0.8.4 - 2017-06-28 - It's been too long

Lots of updates based on our internal use, needs, and feedback & suggestions
from our OSS users. There's too much to list out since the previous release was
cut, but I'll try to cover the highlights/important bits and promise to do
better in the future :fingers_crossed:

#### Major:

* Complete rework of record validation with lenient mode support added to
  octodns-dump so that data with validation problems can be dumped to config
  files as a starting point. octoDNS now also ignores validation errors when
  pulling the current state from a provider before planning changes. In both
  cases this is best effort.
* Naming of record keys are based on RFC-1035 and friends, previous names have
  been kept for backwards compatibility until the 1.0 release.
* Provider record type support is now explicit, i.e. opt-in, rather than
  opt-out. This prevents bugs/oversights in record handling where providers
  don't support (new) record types and didn't correctly ignore them.
* ALIAS support for DNSimple, Dyn, NS1, PowerDNS
* Ignored record support added, `octodns:\n  ignored: True`
* Ns1Provider added

#### Miscellaneous

* Use a 3rd party lib for natural sorting of keys, rather than my old
  implementation. Sorting can be disabled in the YamlProvider with
  `enforce_order: False`.
* Semi-colon/escaping fixes and improvements.
* Meta record support, `TXT octodns-meta.<zone>`. For now just
  `provider=<provider-id>`. Optionally turned on with `include_meta` manager
  config val.
* Validations check for CNAMEs co-existing with other records and error out if
  found. Was a common mistaken/unknown issue and this surfaces the problem
  early.
* Sizeable refactor in the way Route53 record translation works to make it
  cleaner/less hacky
* Lots of docs type-o fixes
* Fixed some pretty major bugs in DnsimpleProvider
* Relax UnsafePlan checks a bit, more to come here
* Set User-Agent header on Dyn health checks

## v0.8.0 - 2017-03-14 - First public release
