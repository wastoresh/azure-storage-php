Tracking Breaking changes in 0.17.0

All
* Refined code logic for continuation token. Now continuation token will be null if there are no more instance to be queried/listed.

Blob
* Removed `MicrosoftAzure\Storage\Tests\unit\Blob\Models\BlobContinuationToken`, now use `MicrosoftAzure\Storage\Common\MarkerContinuationToken` instead for better code structure and reuse.

Table
* Deprecated ATOM support for Table service.

Queue
* Removed `MicrosoftAzure\Storage\Tests\unit\Queue\Models\QueueContinuationToken`, now use `MicrosoftAzure\Storage\Common\MarkerContinuationToken` instead for better code structure and reuse.

File
* Removed `MicrosoftAzure\Storage\Tests\unit\File\Models\FileContinuationToken`, now use `MicrosoftAzure\Storage\Common\MarkerContinuationToken` instead for better code structure and reuse.

Tracking Breaking changes in 0.16.0

All
* Renamed and moved `MicrosoftAzure\Storage\Blob\Models\PageRange` to `MicrosoftAzure\Storage\Common\Models\Range`.

Table
* Created new types for the following APIs to support specifying accepted content type of response payload. Payload is now by default `application/json;odata=minimalmetadata`.
  - `MicrosoftAzure\Storage\Table\TableRestProxy::createTable` & `MicrosoftAzure\Storage\Table\TableRestProxy::createTableAsync` now uses `MicrosoftAzure\Storage\Table\Models\TableServiceCreateOptions`.
  - `MicrosoftAzure\Storage\Table\TableRestProxy::insertEntity` & `MicrosoftAzure\Storage\Table\TableRestProxy::insertEntityAsync` now uses `MicrosoftAzure\Storage\Table\Models\TableServiceCreateOptions`.
  - `MicrosoftAzure\Storage\Table\TableRestProxy::getTable` & `MicrosoftAzure\Storage\Table\TableRestProxy::getTableAsync` now uses `MicrosoftAzure\Storage\Table\Models\GetTableOptions`.
  - `MicrosoftAzure\Storage\Table\TableRestProxy::getEntity` & `MicrosoftAzure\Storage\Table\TableRestProxy::getEntityAsync` now uses `MicrosoftAzure\Storage\Table\Models\GetEntityOptions`.
* E-Tag can now be null value since when user specified to return minimal/no metadata, E-Tag will not be returned with response.
* When specifying `NO_METADATA` for querying entities, some Edm type, including Edm.Binary, Edm.DateTime and Edm.Guid could not be determined through the type detection heuristics. For more information, please see [Payload Format for Table Service Operations](https://docs.microsoft.com/en-us/rest/api/storageservices/payload-format-for-table-service-operations).

Queue
* Renamed `MicrosoftAzure\Storage\Queue\Models\MicrosoftAzureQueueMessage` to `MicrosoftAzure\Storage\Queue\Models\QueueMessage`

Tracking Breaking changes in 0.15.0

All
* Removed `setRequestOptions` for service options, instead, added `middlewares`, `middlewareStack`, `numberOfConcurrency`, `isStreaming`, `locationMode` and `decodeContent` for user to specify the corresponding options.
* Added `MicrosoftAzure\Storage\Common\Middlewares\RetryMiddleware` to support retry from secondary endpoint. Advice to use this instead of Guzzle's retry middleware for secondary endpoint retry support.
* Removed `MicrosoftAzure\Storage\Common\Models\ServiceProperties::getMetrics` and `MicrosoftAzure\Storage\Common\Models\ServiceProperties::setMetrics`. Added following methods to access hour metrics and minute metrics.
```
MicrosoftAzure\Storage\Common\Models\ServiceProperties::getHourMetrics
MicrosoftAzure\Storage\Common\Models\ServiceProperties::setHourMetrics
MicrosoftAzure\Storage\Common\Models\ServiceProperties::getMinuteMetrics
MicrosoftAzure\Storage\Common\Models\ServiceProperties::setMinuteMetrics
```

Blob
* Lease feature parity
  - Renamed `LeaseBlobResult` to `LeaseResult` to support container and blob lease.
  - Added container lease support - passing `null` to `$blob` parameter of the lease related APIs.
  - Added new parameters `$proposedLeaseId` and `$leaseDuration` to `acquireLease` API and changed the `$options` parameter from `AcquireLeaseOptions` to `BlobServiceOptions`.
  - Added new parameter `$breakPeriod` to  `breakLease` API and removed the `$leaseId` parameter.

* Refactored Options class:
  - Exracted `getLeaseId`, `setLeaseId`, `getAccessConditions` and `setAccessConditions` to the base options class `BlobServiceOptions`.
  - Refactored the `CreateBlobOptions`, `CommitBlobBlocksOptions` class to remove duplicate options and standardize the content settings related properties like `ContentType`, `ContentMD5`, `ContentEncoding`, `ContentLanguage`, `CacheControl` and `ContentDisposition`.

* Removed the useless API `ctrCrypt` from `Utilities` class.

Table
* Removed `MicrosoftAzure\Storage\Table\Models\BatchError`. When batch operation fails, exception is thrown immediately instead.

Tracking Breaking changes in 0.14.0

* Restructured the classes based on their intended functionality and visiblity. The changes includes:
  - `MicrosoftAzure\Storage\Common\Internal\InvalidArgumentTypeException` was moved to `MicrosoftAzure\Storage\Common\Exceptions\InvalidArgumentTypeException`
  - `MicrosoftAzure\Storage\Common\ServiceException` was moved to `MicrosoftAzure\Storage\Exceptions\ServiceException`
  - `MicrosoftAzure\Storage\Common\Internal\HttpFormatter` was moved to `MicrosoftAzure\Storage\Common\Internal\Http\HttpFormatter`
  - `MicrosoftAzure\Storage\Common\ServiceOptionsBase` was moved to `MicrosoftAzure\Storage\Common\Internal\ServiceOptionsBase`
  - `MicrosoftAzure\Storage\Common\Internal\Logger` was moved to `MicrosoftAzure\Storage\Common\Logger`
  - `MicrosoftAzure\Storage\Common\Internal\Middlewares\HistoryMiddleware` was moved to `MicrosoftAzure\Storage\Common\Middlewares\HistoryMiddleware`
  - `MicrosoftAzure\Storage\Common\Internal\IMiddleware` was moved to `MicrosoftAzure\Storage\Common\Middlewares\IMiddleware`
  - `MicrosoftAzure\Storage\Common\Internal\Middlewares\MiddlewareBase` was moved to `MicrosoftAzure\Storage\Common\Middlewares\MiddlewareBase`
  - `MicrosoftAzure\Storage\Common\Internal\RetryMiddlewareFactory` was moved to `MicrosoftAzure\Storage\Common\Middlewares\RetryMiddlewareFactory`

Tracking Breaking changes in 0.13.0

* Modified the setters of most classes that represent API call result from `public` to `protected` to avoid unwanted corruption of SDK constructed data. If the user is using the setters prior to the release there could be a breaking change.

Tracking Breaking changes in 0.12.0

* Moved `getMetadataArray` and `validateMetadata` from ServiceRestProxy.php to Utilities.php
* Refined return type of the following API calls, to be more reasonable.
    ```
    createPageBlob
    createBlockBlob
    createBlobBlock
    renewLease
    acquireLease
    ```
* Applied strong type for the project. This may break some cases where user use to mis-use the type of some input parameters.

Tracking Breaking Changes in 0.10.0
ALL
* Remove all pear dependencies: HTTP_Request2, Mail_mime, and Mail_mimeDecode. Use Guzzle as underlying http client library.
* Change root namespace from "WindowsAzure" to "MicrosoftAzure/Storage".
* When set metadata operations contains invalid characters, it throws a ServiceException with 400 bad request error instead of Http_Request2_LogicException.

BLOB
* MicrosoftAzure\Storage\Blob\Models\Blocks.setBlockId now requires a base64 encoded string.
