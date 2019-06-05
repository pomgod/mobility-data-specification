# Mobility Data Specification: Metric

This specification defines a set of core metrics for assessing the status of mobility services, and an API endpoint for accessing them.

- Authors: LADOT
- Date: 15 June 2019
- Version: alpha

## Table of Contents

- [Audience](#audience)
- [Background](#background)

<a name="audience"></a>

## Audience

There are three intended audiences of this API:

1. Agencies, as consumers of metrics for assessing status
2. Tooling companies that want to create applications that provide these metrics
3. Providers, so that they can verify their status

<a name="background"></a>

## Background

The first goal of this specification is to define a set of core metrics that are consistently calculated and published across MDS implementations. These core metrics are not intended to be comprehensive, but rather to support key uses cases while maintaining data privacy.

The second goal is to define an API endpoint providing immediate access to these core metrics as well as additional non-core metrics that have not been standardized.
