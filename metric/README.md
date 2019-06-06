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

The first goal of this specification is to define a set of core metrics that:

- Are consistently calculated and published across MDS implementations
- Support key use cases
- Meet data privacy guidelines
- Are readily computable from MDS data

The second goal is to define an API endpoint that:

- provides immediate access to historical core metrics
- Meets data privacy guidelines
- is extendable to support new or experimental metrics without introducing breaking changes

## Core Metrics

/metrics/historical

### Abandonment

Name: abandonment

Summary: number of vehicles that have been idle for a specified number of days

Calculation: count of vehicles whose metric:idleTimeSince is greater than or equal to `days`

Constants:

- `days` - number of days to be considered abandoned

Request parameters:

- `interval` -
- `provider` -

Response: vehicle count

### Metric API

### Request Parameters

Each metric supports one or more request parameters, which are implemented consistently across metrics unless otherwise specified. A minimum set of parameters and parameter values are defined which a focused on supporting key use cases and ensuring results are readily computable.

#### Time Range

- `start_time`
- `end_time`

#### Time Interval

- `time_interval` -
  - "15m", "1h", "1d", "1w"

#### Geographic Aggregation

- `geo_agg` - specify a geographic aggregation method to apply. The result is metrics binned to geographic units defined by the method. Possible methods include:
  - `geography` - as used for policy defintion
  - `street_segment`
  - `census_tract`
  - `s2`
  - `h3`
