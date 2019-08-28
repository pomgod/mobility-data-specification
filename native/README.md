# Native API

## Version 0.1 (Draft)

# Background

MDS enables cities or municipal operators to receive information directly from regulated / permitted entities (“providers”) relating to the behavior of vehicles and devices in the public right of way.
This API makes that information available to agencies or vendors that want to use an aggregated feed to build applications and/or tools using this information.

# Authorization

See [Authorization](../common/CommonStandards.md#authorization). In addition, certain endpoints require specific `scope` values to be present in the access token supplied by the user/application.

# Endpoints

The Native API consists of endpoints for retrieving providers as well as device information and events as reported by providers via MDS Agency.

## Get Providers

```
GET /native/providers
```

This endpoint shall be called to retrieve the canonical list of providers from MDS.

#### Response Codes

| Code          | Description                                                          |
| ------------- | -------------------------------------------------------------------- |
| 200 OK        | This response contains the list of all providers.                    |
| 403 Forbidden | The authorization token does not contain the `providers:read` scope. |

#### Response Body

The response body contains the following properties:

| Property  | Description                                                |
| --------- | ---------------------------------------------------------- |
| version   | The version of the schema currently implemeted by the API. |
| providers | The list of all providers.                                 |

Refer to the [Get Providers JSON Schema](./GetProviders.json) document for the exact response format.

## Get Events (Initial)

```
GET /native/events
```

This endpoint shall be called to retrieve an initial set of vehicle events from MDS.

#### Query String Parameters

| Name        | Type                                                          | R/O | Description                                                                                                                       |
| ----------- | ------------------------------------------------------------- | --- | --------------------------------------------------------------------------------------------------------------------------------- |
| limit       | number                                                        | O   | The maximum of matching events to be returned. The default and maximum value for this parameter is defined by the implementation. |
| provider_id | [UUID](../common/DataDefinitions.md#unique-identifiers-uuids) | O   | Only include events for the specified provider.                                                                                   |
| device_id   | [UUID](../common/DataDefinitions.md#unique-identifiers-uuids) | O   | Only include events for the specified device.                                                                                     |
| start_time  | [Timestamp](../common/DataDefinitions.md#timestamps)          | O   | Only include events with timestamps on or after the specified start time.                                                         |
| end_time    | [Timestamp](../common/DataDefinitions.md#timestamps)          | O   | Only include events with timestamps on or before the specified end time.                                                          |

#### Response Codes

| Code            | Description                                                       |
| --------------- | ----------------------------------------------------------------- |
| 200 OK          | This response contains the list of all MDS providers.             |
| 400 Bad Request | A validation error occurred.                                      |
| 403 Forbidden   | The authorization token does not contain the `events:read` scope. |

#### Response Body

The response body contains the following properties:

| Property | Description                                                                                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version  | The version of the schema currently implemeted by the API.                                                                                                           |
| cursor   | An opaque string that shall be included in the URL of a subsequent request to obtain the next set of events in sequence.                                             |
| events   | A list of vehicle events. If the list of events is empty, the same cursor value will be returned and can be re-used after some time perioud to check for new events. |

Refer to the [Get Events JSON Schema](./GetEvents.json) document for the exact response format.

## Get Events (Subsequent)

```
GET /native/events/{cursor}
```

This endpoint shall be called to retrieve the next set of vehicle events from MDS using a cursor obtained from a previous request.

#### Path Parameters

| Name   | Type   | R/O | Description                                                                                          |
| ------ | ------ | --- | ---------------------------------------------------------------------------------------------------- |
| cursor | string | O   | An opaque string obtained from a previous request used to obtain the next set of events in sequence. |

#### Query String Parameters

| Name  | Type   | R/O | Description                                                                                                                                     |
| ----- | ------ | --- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| limit | number | O   | If specified, the maximum of matching events to be returned. The default and maximum value for this parameter is defined by the implementation. |

> The specified `cursor` is used to reconstruct filter parameters from the initial request. Supplying filter values when using a cursor is considered an invalid request. `limit` is the only query string parameter supported when using a cursor.

#### Response Codes

| Code            | Description                                                       |
| --------------- | ----------------------------------------------------------------- |
| 200 OK          | This response contains the list of all MDS providers.             |
| 400 Bad Request | A validation error occurred.                                      |
| 403 Forbidden   | The authorization token does not contain the `events:read` scope. |

#### Response Body

The response body contains the following properties:

| Property | Description                                                                                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version  | The version of the schema currently implemeted by the API.                                                                                                           |
| cursor   | An opaque string that shall be included in the URL of a subsequent request to obtain the next set of events in sequence.                                             |
| events   | A list of vehicle events. If the list of events is empty, the same cursor value will be returned and can be re-used after some time perioud to check for new events. |

Refer to the [Get Events JSON Schema](./GetEvents.json) document for the exact response format.


## Get Device

```
GET /native/device/{device_id}
```

This endpoint shall be called to retrieve information about a specific device from MDS.

#### Path Parameters

| Name      | Type                                                          | R/O | Description                          |
| --------- | ------------------------------------------------------------- | --- | ------------------------------------ |
| device_id | [UUID](../common/DataDefinitions.md#unique-identifiers-uuids) | R   | The unique identifier of the device. |

#### Response Codes

| Code            | Description                                                        |
| --------------- | ------------------------------------------------------------------ |
| 200 OK          | This response contains the list of all MDS providers.              |
| 400 Bad Request | A validation error occurred.                                       |
| 403 Forbidden   | The authorization token does not contain the `devices:read` scope. |
| 404 Not Found   | A device with the specified `device_id` was not found.             |

#### Response Body

The response body contains the following properties:

| Property | Description                                                |
| -------- | ---------------------------------------------------------- |
| version  | The version of the schema currently implemeted by the API. |
| device   | Information about the specified device.                    |

Refer to the [Get Device JSON Schema](./GetDevice.json) document for the exact response format.