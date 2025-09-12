Object storage for Open edX with S3
===================================

This is an **experimental** plugin for
[Tutor](https://docs.tutor.overhang.io) that allows [Open edX](https://openedx.org/) to be
configured with custom S3 settings, including the use of an [S3](https://aws.amazon.com/s3/)
API-compatible storage platform — such as [Ceph with
radosgw](https://docs.ceph.com/en/latest/radosgw/index.html).

This plugin is meant to make Tutor-managed Open edX interface with S3
*hosted outside of Tutor.* If instead you want to deploy your own
S3-compatible storage platform *as part of Tutor,* please consider the
[minio](https://github.com/overhangio/tutor-minio) Tutor plugin.

Version compatibility matrix
----------------------------

You must install a supported release of this plugin to match the Open
edX and Tutor version you are deploying. If you are installing this
plugin from a branch in this Git repository, you must select the
appropriate one:

| Open edX release | Tutor version     | Plugin branch | Plugin release |
|------------------|-------------------|---------------|----------------|
| Lilac            | `>=12.0, <13`     | Not supported | Not supported  |
| Maple            | `>=13.2, <14`[^1] | `maple`       | 0.3.x          |
| Nutmeg           | `>=14.0, <15`     | `quince`      | `>=1.0.0, <2`  |
| Olive            | `>=15.0, <16`     | `quince`      | `>=1.1.0, <2`  |
| Palm             | `>=16.0, <17`     | `quince`      | `>=1.2.0, <2`  |
| Quince           | `>=17.0, <18`     | `quince`      | `>=1.3.0, <2`  |
| Redwood          | `>=18.0, <19`     | `teak`        | `>=2`          |
| Sumac            | `>=19.0, <20`     | `teak`        | `>=2.1.0`      |
| Teak             | `>=20.0, <21`     | `teak`        | `>=2.3.1, <2.4`|
| Ulmo             | `>=21.0, <22`     | `main`        | `>=2.4.0`      |

[^1]: For Open edX Maple and Tutor 13, you must run version 13.2.0 or
    later. That is because this plugin uses the Tutor v1 plugin API,
    [which was introduced with that
    release](https://github.com/overhangio/tutor/blob/master/CHANGELOG.md#v1320-2022-04-24).

Installation
------------

    pip install git+https://github.com/hastexo/tutor-contrib-s3@v2.3.1

Then, to enable this plugin, run:

    tutor plugins enable s3

Plugin configuration
--------------------

* `OPENEDX_AWS_ACCESS_KEY` (default: `""`)
* `OPENEDX_AWS_SECRET_ACCESS_KEY` (default: `""`)
* `S3_HOST` (default: `""`) — set only if using any other service than AWS S3
* `S3_PORT` (default: `""`) — set only if using any other service than AWS S3
* `S3_REGION` (default: `""`)
* `S3_USE_SSL` (default: `true`)
* `S3_STORAGE_BUCKET` (default: `"openedx"`)
* `S3_FILE_UPLOAD_BUCKET` (default: `"{{ S3_STORAGE_BUCKET }}"`)
* `S3_PROFILE_IMAGE_BUCKET` (default: `""`, meaning profile images are
  stored in the filesystem, rather than in S3)
* `S3_GRADE_BUCKET` (default: `"{{ S3_STORAGE_BUCKET }}"`)
* `S3_ADDRESSING_STYLE` (default: `"auto"`)
* `S3_SIGNATURE_VERSION` (default: `"s3v4"`)
* `S3_CUSTOM_DOMAIN` (default: `""`) — do not set if you are using AWS S3
* `S3_PROFILE_IMAGE_CUSTOM_DOMAIN` (default: `""`)
* `S3_DEFAULT_ACL` (default: `None`[^null], meaning inherit from the parent bucket and fall back to the S3 provider's default canned ACL[^private] if unset)
* `S3_REQUEST_CHECKSUM_CALCULATION` (default: `"when_required"`)

These values can be modified by the `tutor config save --set
PARAM_NAME=VALUE` command, or by setting them in `$(tutor config
printroot)/config.yml`.

[^null]: If you want to explicitly set a value to None in `config.yml`, use `!!null`.

[^private]: In AWS S3, the default ACL is `private`.

Depending on the nature and configuration of your S3-compatible
service, some of these values may be required to set.

* If using AWS S3, you will need to set `S3_REGION` to a non-empty value.
  And make sure `S3_ADDRESSING_STYLE` is set to `"auto"` (default) and
  `S3_CUSTOM_DOMAIN` is set to `""` (default).
* If you want to use an alternative S3-compatible service, you need to set the
  `S3_HOST` and `S3_PORT` parameters.
* For a Ceph Object Gateway that doesn’t set
  [`rgw_dns_name`](https://docs.ceph.com/en/latest/radosgw/config-ref/#confval-rgw_dns_name),
  you will need `S3_ADDRESSING_STYLE: path`.
* Due to limitations in Open edX, if you are using `s3v4` signatures, your
  `S3_PROFILE_IMAGE_BUCKET` must have a public ACL and you must set
  `S3_PROFILE_IMAGE_CUSTOM_DOMAIN`.

Guide for configuring buckets for AWS S3
----------------------------------------

Follow [this guide](README-aws.md) to configure buckets on AWS S3 to
be used with Open edX.
