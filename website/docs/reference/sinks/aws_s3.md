---
delivery_guarantee: "at_least_once"
event_types: ["log"]
issues_url: https://github.com/timberio/vector/issues?q=is%3Aopen+is%3Aissue+label%3A%22sink%3A+aws_s3%22
operating_systems: ["linux","macos","windows"]
sidebar_label: "aws_s3|[\"log\"]"
source_url: https://github.com/timberio/vector/tree/master/src/sinks/aws_s3.rs
status: "beta"
title: "aws_s3 sink"
unsupported_operating_systems: []
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/docs/reference/sinks/aws_s3.md.erb
-->

The `aws_s3` sink [batches](#buffers-and-batches) [`log`][docs.data-model#log] events to [AWS S3][urls.aws_s3] via the [`PutObject` API endpoint](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html).

## Configuration

import Tabs from '@theme/Tabs';

<Tabs
  block={true}
  defaultValue="common"
  values={[
    { label: 'Common', value: 'common', },
    { label: 'Advanced', value: 'advanced', },
  ]
}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

import CodeHeader from '@site/src/components/CodeHeader';

<CodeHeader fileName="vector.toml" learnMoreUrl="/docs/setup/configuration"/ >

```toml
[sinks.my_sink_id]
  # REQUIRED - General
  type = "aws_s3" # example, must be: "aws_s3"
  inputs = ["my-source-id"] # example
  bucket = "my-bucket" # example
  endpoint = "127.0.0.0:5000" # example
  region = "us-east-1" # example
  
  # REQUIRED - requests
  encoding = "ndjson" # example, enum
  
  # OPTIONAL - Object Names
  key_prefix = "date=%F/" # default
```

</TabItem>
<TabItem value="advanced">

<CodeHeader fileName="vector.toml" learnMoreUrl="/docs/setup/configuration" />

```toml
[sinks.my_sink_id]
  # REQUIRED - General
  type = "aws_s3" # example, must be: "aws_s3"
  inputs = ["my-source-id"] # example
  bucket = "my-bucket" # example
  endpoint = "127.0.0.0:5000" # example
  region = "us-east-1" # example
  
  # REQUIRED - requests
  encoding = "ndjson" # example, enum
  
  # OPTIONAL - General
  healthcheck = true # default
  
  # OPTIONAL - Batching
  batch_size = 10490000 # default, bytes
  batch_timeout = 300 # default, seconds
  
  # OPTIONAL - Object Names
  filename_append_uuid = true # default
  filename_extension = true # default
  filename_time_format = "%s" # default
  key_prefix = "date=%F/" # default
  
  # OPTIONAL - Requests
  request_in_flight_limit = 5 # default
  request_rate_limit_duration_secs = 1 # default, seconds
  request_rate_limit_num = 5 # default
  request_retry_attempts = 5 # default
  request_retry_backoff_secs = 1 # default, seconds
  request_timeout_secs = 30 # default, seconds
  
  # OPTIONAL - Buffer
  [sinks.my_sink_id.buffer]
    type = "memory" # default, enum
    max_size = 104900000 # example, no default, bytes, relevant when type = "disk"
    num_items = 500 # default, events, relevant when type = "memory"
    when_full = "block" # default, enum
```

</TabItem>

</Tabs>

## Options

import Fields from '@site/src/components/Fields';

import Field from '@site/src/components/Field';

<Fields filters={true}>


<Field
  common={false}
  defaultValue={10490000}
  enumValues={null}
  examples={[10490000]}
  name={"batch_size"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"bytes"}
  >

### batch_size

The maximum size of a batch before it is flushed. See [Buffers & Batches](#buffers-batches) for more info.


</Field>


<Field
  common={false}
  defaultValue={300}
  enumValues={null}
  examples={[300]}
  name={"batch_timeout"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  >

### batch_timeout

The maximum age of a batch before it is flushed. See [Buffers & Batches](#buffers-batches) for more info.


</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["my-bucket"]}
  name={"bucket"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### bucket

The S3 bucket name. Do not include a leading `s3://` or a trailing `/`.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  name={"buffer"}
  nullable={true}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  >

### buffer

Configures the sink specific buffer.

<Fields filters={false}>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[104900000]}
  name={"max_size"}
  nullable={true}
  path={"buffer"}
  relevantWhen={{"type":"disk"}}
  required={false}
  templateable={false}
  type={"int"}
  unit={"bytes"}
  >

#### max_size

The maximum size of the buffer on the disk.


</Field>


<Field
  common={false}
  defaultValue={500}
  enumValues={null}
  examples={[500]}
  name={"num_items"}
  nullable={true}
  path={"buffer"}
  relevantWhen={{"type":"memory"}}
  required={false}
  templateable={false}
  type={"int"}
  unit={"events"}
  >

#### num_items

The maximum number of [events][docs.data-model#event] allowed in the buffer.


</Field>


<Field
  common={false}
  defaultValue={"memory"}
  enumValues={{"memory":"Stores the sink's buffer in memory. This is more performant (~3x), but less durable. Data will be lost if Vector is restarted abruptly.","disk":"Stores the sink's buffer on disk. This is less performance (~3x),  but durable. Data will not be lost between restarts."}}
  examples={["memory","disk"]}
  name={"type"}
  nullable={false}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### type

The buffer's type / location. `disk` buffers are persistent and will be retained between restarts.


</Field>


<Field
  common={false}
  defaultValue={"block"}
  enumValues={{"block":"Applies back pressure when the buffer is full. This prevents data loss, but will cause data to pile up on the edge.","drop_newest":"Drops new data as it's received. This data is lost. This should be used when performance is the highest priority."}}
  examples={["block","drop_newest"]}
  name={"when_full"}
  nullable={false}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### when_full

The behavior when the buffer becomes full.


</Field>


</Fields>

</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={{"ndjson":"Each event is encoded into JSON and the payload is new line delimited.","text":"Each event is encoded into text via the `message` key and the payload is new line delimited."}}
  examples={["ndjson","text"]}
  name={"encoding"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### encoding

The encoding format used to serialize the events before outputting.


</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["127.0.0.0:5000"]}
  name={"endpoint"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### endpoint

Custom endpoint for use with AWS-compatible services.


</Field>


<Field
  common={false}
  defaultValue={true}
  enumValues={null}
  examples={[true,false]}
  name={"filename_append_uuid"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

### filename_append_uuid

Whether or not to append a UUID v4 token to the end of the file. This ensures there are no name collisions high volume use cases. See [Object Naming](#object-naming) for more info.


</Field>


<Field
  common={false}
  defaultValue={"log"}
  enumValues={null}
  examples={[true,false]}
  name={"filename_extension"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

### filename_extension

The extension to use in the object name.


</Field>


<Field
  common={false}
  defaultValue={"%s"}
  enumValues={null}
  examples={["%s"]}
  name={"filename_time_format"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### filename_time_format

The format of the resulting object file name. [`strftime` specifiers][urls.strptime_specifiers] are supported. See [Object Naming](#object-naming) for more info.


</Field>


<Field
  common={false}
  defaultValue={true}
  enumValues={null}
  examples={[true,false]}
  name={"healthcheck"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

### healthcheck

Enables/disables the sink healthcheck upon start. See [Health Checks](#health-checks) for more info.


</Field>


<Field
  common={true}
  defaultValue={"date=%F"}
  enumValues={null}
  examples={["date=%F/","date=%F/hour=%H/","year=%Y/month=%m/day=%d/","application_id={{ application_id }}/date=%F/"]}
  name={"key_prefix"}
  nullable={true}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={true}
  type={"string"}
  unit={null}
  >

### key_prefix

A prefix to apply to all object key names. This should be used to partition your objects, and it's important to end this value with a `/` if you want this to be the root S3 "folder". See [Object Naming](#object-naming), [Partitioning](#partitioning), and [Template Syntax](#template-syntax) for more info.


</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["us-east-1"]}
  name={"region"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### region

The [AWS region][urls.aws_s3_regions] of the target S3 bucket.


</Field>


<Field
  common={false}
  defaultValue={5}
  enumValues={null}
  examples={[5]}
  name={"request_in_flight_limit"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={null}
  >

### request_in_flight_limit

The maximum number of in-flight requests allowed at any given time. See [Rate Limits](#rate-limits) for more info.


</Field>


<Field
  common={false}
  defaultValue={1}
  enumValues={null}
  examples={[1]}
  name={"request_rate_limit_duration_secs"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  >

### request_rate_limit_duration_secs

The window used for the[`request_rate_limit_num`](#request_rate_limit_num) option See [Rate Limits](#rate-limits) for more info.


</Field>


<Field
  common={false}
  defaultValue={5}
  enumValues={null}
  examples={[5]}
  name={"request_rate_limit_num"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={null}
  >

### request_rate_limit_num

The maximum number of requests allowed within the[`request_rate_limit_duration_secs`](#request_rate_limit_duration_secs) window. See [Rate Limits](#rate-limits) for more info.


</Field>


<Field
  common={false}
  defaultValue={5}
  enumValues={null}
  examples={[5]}
  name={"request_retry_attempts"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={null}
  >

### request_retry_attempts

The maximum number of retries to make for failed requests. See [Retry Policy](#retry-policy) for more info.


</Field>


<Field
  common={false}
  defaultValue={1}
  enumValues={null}
  examples={[1]}
  name={"request_retry_backoff_secs"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  >

### request_retry_backoff_secs

The amount of time to wait before attempting a failed request again. See [Retry Policy](#retry-policy) for more info.


</Field>


<Field
  common={false}
  defaultValue={30}
  enumValues={null}
  examples={[30]}
  name={"request_timeout_secs"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  >

### request_timeout_secs

The maximum time a request can take before being aborted. It is highly recommended that you do not lower value below the service's internal timeout, as this could create orphaned requests, pile on retries, and result in deuplicate data downstream.


</Field>


</Fields>

## Env Vars

<Fields filters={true}>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["AKIAIOSFODNN7EXAMPLE"]}
  name={"AWS_ACCESS_KEY_ID"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### AWS_ACCESS_KEY_ID

Used for AWS authentication when communicating with AWS services. See relevant [AWS components][pages.aws_components] for more info. See [Authentication](#authentication) for more info.


</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["wJalrXUtnFEMI/K7MDENG/FD2F4GJ"]}
  name={"AWS_SECRET_ACCESS_KEY"}
  nullable={false}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### AWS_SECRET_ACCESS_KEY

Used for AWS authentication when communicating with AWS services. See relevant [AWS components][pages.aws_components] for more info. See [Authentication](#authentication) for more info.


</Field>


</Fields>

## Output

The `aws_s3` sink [batches](#buffers-and-batches) [`log`][docs.data-model#log] events to [AWS S3][urls.aws_s3] via the [`PutObject` API endpoint](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html).
Batches are flushed via the [`batch_size`](#batch_size) or
[`batch_timeout`](#batch_timeout) options. You can learn more in the [buffers &
batches](#buffers--batches) section.

## How It Works

### Authentication

Vector checks for AWS credentials in the following order:

1. Environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
2. The [`credential_process` command][urls.aws_credential_process] in the AWS config file. (usually located at `~/.aws/config`)
3. The [AWS credentials file][urls.aws_credentials_file]. (usually located at `~/.aws/credentials`)
4. The [IAM instance profile][urls.iam_instance_profile]. (will only work if running on an EC2 instance with an instance profile/role)

If credentials are not found the [healtcheck](#healthchecks) will fail and an
error will be [logged][docs.monitoring#logs].

#### Obtaining an access key

In general, we recommend using instance profiles/roles whenever possible. In
cases where this is not possible you can generate an AWS access key for any user
within your AWS account. AWS provides a [detailed guide][urls.aws_access_keys] on
how to do this.

### Buffers & Batches

import SVG from 'react-inlinesvg';

<SVG src="/img/buffers-and-batches-partitioned.svg" />

The `aws_s3` sink buffers & batches data as
shown in the diagram above. You'll notice that Vector treats these concepts
differently, instead of treating them as global concepts, Vector treats them
as sink specific concepts. This isolates sinks, ensuring services disruptions
are contained and [delivery guarantees][docs.guarantees] are honored.

*Batches* are flushed when 1 of 2 conditions are met:

1. The batch age meets or exceeds the configured[`batch_timeout`](#batch_timeout) (default: `300 seconds`).
2. The batch size meets or exceeds the configured[`batch_size`](#batch_size) (default: `10490000 bytes`).

*Buffers* are controlled via the [`buffer.*`](#buffer) options.

### Columnar Formats

Vector has plans to support column formats, such as ORC and Parquet, in
[`v0.6`][urls.vector_roadmap].

### Environment Variables

Environment variables are supported through all of Vector's configuration.
Simply add `${MY_ENV_VAR}` in your Vector configuration file and the variable
will be replaced before being evaluated.

You can learn more in the [Environment Variables][docs.configuration#environment-variables]
section.

### Health Checks

Health checks ensure that the downstream service is accessible and ready to
accept data. This check is performed upon sink initialization.
If the health check fails an error will be logged and Vector will proceed to
start.

#### Require Health Checks

If you'd like to exit immediately upon a health check failure, you can
pass the `--require-healthy` flag:

```bash
vector --config /etc/vector/vector.toml --require-healthy
```

#### Disable Health Checks

If you'd like to disable health checks for this sink you can set the[`healthcheck`](#healthcheck) option to `false`.

### Object Naming

By default, Vector will name your S3 objects in the following format:

<Tabs
  block={true}
  defaultValue="manual"
  values={[
    { label: 'Without Compression', value: 'without_compression', },
    { label: 'With Compression', value: 'with_compression', },
  ]
}>

<TabItem value="without_compression">

```
<key_prefix><timestamp>-<uuidv4>.log
```

For example:

```
date=2019-06-18/1560886634-fddd7a0e-fad9-4f7e-9bce-00ae5debc563.log
```

</TabItem>
<TabItem value="with_compression">

```
<key_prefix><timestamp>-<uuidv4>.log.gz
```

For example:

```
date=2019-06-18/1560886634-fddd7a0e-fad9-4f7e-9bce-00ae5debc563.log.gz
```

</TabItem>
</Tabs>

Vector appends a [UUIDV4][urls.uuidv4] token to ensure there are no name
conflicts in the unlikely event 2 Vector instances are writing data at the same
time.

You can control the resulting name via the[`key_prefix`](#key_prefix),[`filename_time_format`](#filename_time_format),
and[`filename_append_uuid`](#filename_append_uuid) options.



### Partitioning

Partitioning is controlled via the[`key_prefix`](#key_prefix)
options and allows you to dynamically partition data on the fly.
You'll notice that Vector's [template sytax](#template-syntax) is supported
for these options, enabling you to use field values as the partition's key.

### Rate Limits

Vector offers a few levers to control the rate and volume of requests to the
downstream service. Start with the[`request_rate_limit_duration_secs`](#request_rate_limit_duration_secs) and[`request_rate_limit_num`](#request_rate_limit_num) options to ensure Vector does not exceed the specified
number of requests in the specified window. You can further control the pace at
which this window is saturated with the[`request_in_flight_limit`](#request_in_flight_limit) option, which
will guarantee no more than the specified number of requests are in-flight at
any given time.

Please note, Vector's defaults are carefully chosen and it should be rare that
you need to adjust these. If you found a good reason to do so please share it
with the Vector team by [opening an issie][urls.new_aws_s3_sink_issue].

### Retry Policy

Vector will retry failed requests (status == `429`, >= `500`, and != `501`).
Other responses will _not_ be retried. You can control the number of retry
attempts and backoff rate with the[`request_retry_attempts`](#request_retry_attempts) and[`request_retry_backoff_secs`](#request_retry_backoff_secs) options.

### Template Syntax

The[`key_prefix`](#key_prefix) options
support [Vector's template syntax][docs.configuration#template-syntax],
enabling dynamic values derived from the event's data. This syntax accepts
[strptime specifiers][urls.strptime_specifiers] as well as the
`{{ field_name }}` syntax for accessing event fields. For example:

<CodeHeader fileName="vector.toml" />

```toml
[sinks.my_aws_s3_sink_id]
  # ...
  key_prefix = "date=%F/"
  key_prefix = "date=%F/hour=%H/"
  key_prefix = "year=%Y/month=%m/day=%d/"
  key_prefix = "application_id={{ application_id }}/date=%F/"
  # ...
```

You can read more about the complete syntax in the
[template syntax section][docs.configuration#template-syntax].


[docs.configuration#environment-variables]: /docs/setup/configuration#environment-variables
[docs.configuration#template-syntax]: /docs/setup/configuration#template-syntax
[docs.data-model#event]: /docs/about/data-model#event
[docs.data-model#log]: /docs/about/data-model#log
[docs.guarantees]: /docs/about/guarantees
[docs.monitoring#logs]: /docs/administration/monitoring#logs
[pages.aws_components]: /components?providers%5B%5D=aws
[urls.aws_access_keys]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html
[urls.aws_credential_process]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sourcing-external.html
[urls.aws_credentials_file]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[urls.aws_s3]: https://aws.amazon.com/s3/
[urls.aws_s3_regions]: https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region
[urls.iam_instance_profile]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html
[urls.new_aws_s3_sink_issue]: https://github.com/timberio/vector/issues/new?labels=sink%3A+aws_s3
[urls.strptime_specifiers]: https://docs.rs/chrono/0.3.1/chrono/format/strftime/index.html
[urls.uuidv4]: https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)
[urls.vector_roadmap]: https://github.com/timberio/vector/milestones?direction=asc&sort=due_date&state=open