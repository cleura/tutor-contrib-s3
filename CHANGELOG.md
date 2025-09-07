## Unreleased

* [Chore] Replace deprecated setting `DEFAULT_FILE_STORAGE` with `STORAGES['default']['BACKEND']`

## Version 2.3.1 (2025-08-01)

* [Fix] Add the missing import statement for `botocore.client import Config`.

## Version 2.3.0 (2025-07-29)

* [Enhancement] Set the `AWS_S3_CLIENT_CONFIG` variable in addition to the
  individual `AWS_S3_SIGNATURE_VERSION`, `AWS_REQUEST_CHECKSUM_CALCULATION` and
  `AWS_S3_ADDRESSING_STYLE` settings.
* [Enhancement] Support Tutor 20 and Open edX Teak.

## Version 2.2.0 (2025-04-24)

* [Enhancement] Support latest version of boto3; include the 
  `S3_REQUEST_CHECKSUM_CALCULATION` config option.

## Version 2.1.0 (2025-01-03)

* [Enhancement] Support Tutor 19 and Open edX Sumac.

## Version 2.0.0 (2024-10-07)

* [Chore] Drop support for Python 3.8.
* [Enhancement] Introduce `S3_DEFAULT_ACL` configuration option.

## Version 1.5.0 (2024-08-08)

* [Enhancement] Support Tutor 18 and Open edX Redwood.

## Version 1.4.0 (2024-04-05)

* [Enhancement] Support Python 3.12.

## Version 1.3.1 (2024-02-21)

* [Fix] Adjust options for django-storages 1.14.

## Version 1.3.0 (2023-12-22)

* [Enhancement] Support Tutor 17 and Open edX Quince.

## Version 1.2.0 (2023-07-21)

* [Enhancement] Support Tutor 16 and Open edX Palm, Python 3.10, and Python 3.11.
* [Fix] Set `COURSE_METADATA_EXPORT_STORAGE` to the correct
  django-storages backend (`s3boto3.S3Boto3Storage` rather than the
  deprecated `s3boto.S3BotoStorage`).

## Version 1.1.0 (2023-03-21)

* [Enhancement] Support Tutor 15 and Open edX Olive.

## Version 1.0.0 (2022-08-03)

* [BREAKING CHANGE] Support Tutor 14 and Open edX Nutmeg. This entails
  a configuration format change from JSON to YAML, meaning that from
  version 1.0.0 this plugin only supports Tutor versions from 14.0.0
  (and with that, only Open edX versions from Nutmeg).

## Version 0.3.0 (2022-06-29)

* [Enhancement] Use Tutor v1 plugin API.

## Version 0.2.2 (2022-05-05)

* [Enhancement] If `S3_PROFILE_IMAGE_BUCKET` is not set, store profile 
  images in the filesystem. This matches Tutor's default behavior (when 
  running without plugins). If a bucket name is provided, upload profile 
  images to S3.
* [Fix] Add a dummy `base_url` to profile image backend to prevent
  it from crashing LMS when running with `DEBUG=True` (such as when using
  `tutor dev`).

## Version 0.2.1 (2022-03-31)

* [Fix] Change the defaults for `S3_HOST`, `S3_PORT`, and
  `S3_PROFILE_IMAGE_CUSTOM_DOMAIN` from `None` to the empty
  string. Also, change the default for `S3_USE_SSL` from the string
  `"True"` to the boolean `True`.

## Version 0.2.0 (2022-03-01)

* [Enhancement] Add `S3_CUSTOM_DOMAIN` to set a custom domain for S3.
* [Enhancement] Add `S3_PROFILE_IMAGE_CUSTOM_DOMAIN` to set custom domain for 
  profile images.

## Version 0.1.2 (2022-02-22)
 
* [Fix] Rather than incorrectly relying on `ENABLE_HTTPS`, honour
  `S3_USE_SSL` when setting `AWS_S3_ENDPOINT_URL`.
* [Fix] Only populate the `AWS_S3_ENDPOINT_URL` Open edX setting if
  `S3_HOST` is set, and recommend leaving `S3_HOST` and `S3_PORT`
  unset if using AWS S3 (as opposed to an S3-compatible API).
* [Fix] Default for `S3_ADDRESSING_STYLE` is now `"auto"` to use 
  the same default as the ORA2 S3 backend, and the AWS S3 CLI.

## Version 0.1.1 (2022-01-13)

* [Fix] `S3_FILE_UPLOAD_BUCKET` parameter was ignored. Use it to set
  `FILE_UPLOAD_STORAGE_BUCKET_NAME`.

## Version 0.1.0 (2022-01-12)

* [Enhancement] Rename from tutor-s3 to tutor-contrib-s3 (this plugin
  is not affiliated with [the Tutor
  project](https://docs.tutor.overhang.io/)).
* [Enhancement] Support S3Boto3Storage, instead of the deprecated
  S3BotoStorage.
* [Enhancement] No longer rely on buckets with a public ACL, use
  query-string authentication instead.

## Version 0.0.1 (2019-10-10)

**Experimental. Do not use in production.**

* Initial Git import
