# Reservations

## Get all reservations (ver 2023-06-06)

Returns all reservations within scope of the Access Token, filtered according to the specified parameters. This operation uses [Pagination](../guidelines/pagination.md) and supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/getAll/2023-06-06`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationIds": [
        "9b59b50d-bd32-4ce5-add8-09ea0e1300e7"
    ],
    "ServiceIds": [
        "ae8da28c-e8a4-4141-9df0-8c998976c691",
        "6b02d015-47ac-4c41-8e9f-5b4db61d4284"
    ],
    "AccountIds": [
        "94843f6f-3be3-481b-a1c7-06458774c3df"
    ],
    "ReservationGroupIds": [
        "94843f6f-3be3-481b-a1c7-06458774c3df"
    ],
    "UpdatedUtc": {
      "StartUtc": "2023-04-01T00:00:00Z",
       "EndUtc": "2023-05-05T00:00:00Z"
    },
    "Limitation":{
        "Cursor": "819e3435-7d5e-441f-bc68-76d89c69b8f5", 
        "Count": 10
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseIds` | array of string | optional, max 1000 items | Unique identifiers of the [Enterprises](enterprises.md#enterprise). |
| `ReservationIds` | array of string | optional, max 1000 items | Unique identifiers of the [Reservations](#reservation-ver-2023-06-06). |
| `ServiceIds` | array of string | optional, max 1000 items | Unique identifiers of the [Services](services.md#service). If not provided, all bookable services are used. |
| `AccountIds` | array of string | optional, max 1000 items | Unique identifiers of accounts (for example [Customers](customers.md#customer) or [Companies](companies.md#company)) the reservation is associated with. |
| `ReservationGroupIds` | array of string | optional, max 1000 items | Unique identifiers of [Reservation groups](#reservation-group). |
| `States` | array of string [Service order state](#service-order-state) | optional | A list of service order states to filter by. |
| `UpdatedUtc` | [Time interval](_objects.md#time-interval) | optional, max length 3 months | Interval in which the [Reservation](#reservation-ver-2023-06-06) were updated. |
| `CollidingUtc` | [Time interval](_objects.md#time-interval) | optional, max length 3 months | Interval in which the [Reservation](#reservation-ver-2023-06-06) are active. |
| `Limitation` | [Limitation](../guidelines/pagination.md#limitation) | required | Limitation on the quantity of data returned. |

### Response

```javascript
{
    "Reservations": [
        {
            "Id": "9b59b50d-bd32-4ce5-add8-09ea0e1300e7",
            "EnterpriseId": "bd26d8db-86da-4f96-9efc-e5a4654a4a94",
            "ServiceId": "ae8da28c-e8a4-4141-9df0-8c998976c691",
            "AccountId": "94843f6f-3be3-481b-a1c7-06458774c3df",
            "AccountType": "Customer",
            "CreatorProfileId": "3cd637ef-4728-47f9-8fb1-afb900c9cdcf",
            "UpdaterProfileId": "3cd637ef-4728-47f9-8fb1-afb900c9cdcf",
            "BookerId": "ebd507c5-6bfd-4ca9-96aa-ffed6fa94f72",
            "StartUtc": "2023-04-23T14:00:00Z",
            "EndUtc": "2023-04-24T14:00:00Z",
            "Number": "52",
            "State": "Confirmed",
            "Origin": "Connector",
            "CommanderOrigin": null,
            "OriginDetails": null,
            "CreatedUtc": "2023-04-23T14:58:02Z",
            "UpdatedUtc": "2023-04-23T14:58:02Z",
            "CancelledUtc": null,
            "VoucherId": null,
            "BusinessSegmentId": null,
            "Options": {
                "OwnerCheckedIn": true,
                "AllCompanionsCheckedIn": true,
                "AnyCompanionCheckedIn": true,
                "ConnectorCheckIn": true
            },
            "RateId": "ed4b660b-19d0-434b-9360-a4de2ea42eda",
            "CreditCardId": null,
            "GroupId": null,
            "RequestedResourceCategoryId": "773d5e42-de1e-43a0-9ce6-f940faf2303f",
            "AssignedResourceId": "20e00c32-d561-4008-8609-82d8aa525714",
            "AvailabilityBlockId": null,
            "PartnerCompanyId": null,
            "TravelAgencyId": null,
            "AssignedResourceLocked": false,
            "ChannelNumber": "TW48ZP",
            "ChannelManager": "",
            "Purpose": "Leisure",        
            "PersonCounts": [
                {
                    "AgeCategoryId": "1f67644f-052d-4863-acdf-ae1600c60ca0",
                    "Count": 2
                },
                {
                    "AgeCategoryId": "ab58c939-be30-4a60-8f75-ae1600c60c9f",
                    "Count": 2
                }
            ]
        }
    ],
    "Cursor": "9b59b50d-bd32-4ce5-add8-09ea0e1300e7"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Reservations` | array of [Reservations](#reservation-ver-2023-06-06) | required | The reservations of the enterprise. |
| `Cursor` | string | optional | Unique identifier of the item one newer in time order than the items to be returned. If Cursor is not specified, i.e. null, then the latest or most recent items will be returned. |

#### Reservation (ver 2023-06-06)

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Id` | string | required | Unique identifier of the reservation. |
| `EnterpriseId` | string | required | Unique identifier of the [Enterprise](enterprises.md#enterprise). |
| `ServiceId` | string | required | Unique identifier of the [Service](services.md#service) that reservation is made againts. |
| `AccountId` | string | required | Unique identifier of the [Customer](customers.md#customer) or a [Company](companies.md#company) who owns the reservation. |
| `AccountType` | string | required | A discriminator specifying the [type of account](accounts.md#account-type), e.g. customer or company. |
| `CreatorProfileId` | string | required | Unique identifier of the user who created the order item. |
| `UpdaterProfileId` | string | required | Unique identifier of the user who updated the order item. |
| `BookerId` | string | optional | Unique identifier of the [Customer](customers.md#customer) on whose behalf the reservation was made. |
| `StartUtc` | string | required | Reservation start in UTC timezone in ISO 8601 format. |
| `EndUtc` | string | required | Reservation end in UTC timezone in ISO 8601 format. |
| `Number` | string | required | Confirmation number of the reservation in Mews. |
| `State` | string [Service order state](#service-order-state) | required | State of the reservation. |
| `Origin` | string [Service order origin](productserviceorders.md#service-order-origin) | required | Origin of the reservation. |
| `CommanderOrigin` | string [Commander origin](#commander-origin) | optional | Further detail about origin in case of Origin `Commander`. |
| `OriginDetails`| string | optional | Details about the reservation [Origin](productserviceorders.md#service-order-origin). |
| `CreatedUtc` | string | required | Creation date and time of the reservation in UTC timezone in ISO 8601 format. |
| `UpdatedUtc` | string | required | Last update date and time of the reservation in UTC timezone in ISO 8601 format. |
| `CancelledUtc` | string | optional | Cancellation date and time in UTC timezone in ISO 8601 format. |
| `VoucherId` | string | optional | Unique identifier of the [Voucher](vouchers.md#voucher) that has been used to create reservation. |
| `BusinessSegmentId` | string | optional | Identifier of the reservation [Business segment](businesssegments.md#business-segment). |
| `RateId` | string | required | Identifier of the reservation [Rate](rates.md#rate). |
| `CreditCardId` | string | optional | Unique identifier of the [Credit card](creditcards.md#credit-card). |
| `GroupId` | string | required | Unique identifier of the [Reservation group](#reservation-group). |
| `RequestedResourceCategoryId` | string | required | Unique identifier of the [Resource category](resources.md#resource-category). |
| `AssignedResourceId` | string | optional | Identifier of the assigned [Resource](resources.md#resource). |
| `AvailabilityBlockId` | string | optional | Unique identifier of the [Availability block](availabilityblocks.md#availability-block) the reservation is assigned to. |
| `CompanyId` | string | optional | Identifier of the [Company](companies.md#company) on behalf of which the reservation was made. |
| `TravelAgencyId` | string | optional | Identifier of the [Company](companies.md#company) that mediated the reservation. | 
| `AssignedResourceLocked` | bool | required | Whether the reservation is locked to the assigned [Resource](resources.md#resource) and cannot be moved. |
| `ChannelNumber` | string | optional | Number of the reservation within the Channel \(i.e. OTA, GDS, CRS, etc\) in case the reservation group originates there \(e.g. Booking.com confirmation number\). |
| `ChannelManagerNumber` | string | optional | Unique number of the reservation within the reservation group. |
| `CancellationReason` | string [Cancellation reason](#reservation-cancellation-reason) | optional | Cancellation reason of the reservation. |
| `ReleasedUtc` | string | optional | Date when the optional reservation is released in UTC timezone in ISO 8601 format. |
| `Purpose` | string [Reservation purpose](#reservation-purpose) | optional | Purpose of the reservation. |
| `Options` | string [Service order options](serviceorders.md#service-order-options) | optional | Options of the reservations. |
| `PersonCounts` | array of [Person count](#person-count) | required | Number of people per age category the reservation was booked for. |

#### Person count

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `AgeCategoryId` | string | required | Unique identifier of the [Age category](agecategories.md#age-category). |
| `Count` | string | required | Number of people of a given age category. Only positive value is accepted. |

#### Reservation state

* `Enquired` - Confirmed neither by the customer nor enterprise.
* `Requested` - Confirmed by the customer but not by the enterprise \(waitlist\).
* `Optional` - Confirmed by enterprise but not by the guest \(the enterprise is holding resource for the guest\).
* `Confirmed` - Confirmed by both parties, before check-in.
* `Started` - Checked in.
* `Processed` - Checked out.
* `Canceled` - Canceled.

#### Reservation origin

* `Distributor` - Reservation from the Mews Booking Engine or Booking Engine API
* `ChannelManager` - Reservation from a channel integration
* `Commander` - Reservation from Mews Operations
* `Import` - Reservation from an import process
* `Connector` - Reservation from the Mews Connector API
* `Navigator` - Reservation from Mews Guest Services
* ...

#### Reservation purpose

* `Leisure`
* `Business`
* `Student`
* ...

#### Reservation group

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Id` | string | required | Unique identifier of the reservation group. |
| `Name` | string | optional | Name of the reservation group, might be empty or same for multiple groups. |

#### Order note

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Id` | string | required | Unique identifier of the note. |
| `OrderId` | string | required | Unique identifier of the order or [Reservation](#reservation-ver-2023-06-06). |
| `Text` | string | required | Value of the note. |
| `Type` | string [Order note type](#order-note-type) | required | Type of the note. |
| `CreatedUtc` | string | required | Creation date and time of the note in UTC timezone in ISO 8601 format. |

#### Order note type

* `General`
* `ChannelManager`
* ...

#### QrCode data

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `Data` | string | required | Reservation data for QR code generation. |

## ~~Get all reservations (ver 2017-04-12)~~

> ### Deprecated!
> This operation is [deprecated](../deprecations/README.md), please use [Get all reservations \(ver 2023-06-06\)](../operations/reservations.md#get-all-reservations-ver-2023-06-06) instead.

Returns all reservations specified by any identifier, customer or other filter. At least one filter must be present. Note this operation uses [Pagination](../guidelines/pagination.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/getAll`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "StartUtc": "2016-01-01T00:00:00Z",
    "EndUtc": "2016-01-07T00:00:00Z",
    "ServiceIds": [
        "bd26d8db-86da-4f96-9efc-e5a4654a4a94"
    ],
    "ReservationIds": [
        "db6cad34-9a91-448b-bea1-abbe01240d9c"
    ],
    "CustomerIds": [
        "8e1d0ca6-1dde-4be0-8566-aafc01866110"
    ],
    "RateIds": [
        "ed4b660b-19d0-434b-9360-a4de2ea42eda"
    ], 
    "ChannelNumbers": [
        "TW48ZP"
    ],
    "States": [
        "Started"
    ],
    "Extent": {
        "Reservations": true,
        "ReservationGroups": true,
        "Customers": true
    },
    "Limitation": {
        "Cursor": "e7f26210-10e7-462e-9da8-ae8300be8ab7",
        "Count": 10
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `TimeFilter` | string [Reservation time filter](#reservation-time-filter) | optional | Time filter of the interval. If not specified, reservations `Colliding` with the interval are returned. |
| `StartUtc` | string | optional, max length 3 months | Start of the interval in UTC timezone in ISO 8601 format. Required when used in conjunction with the `TimeFilter` or `States` search parameter.|
| `EndUtc` | string | optional, max length 3 months | End of the interval in UTC timezone in ISO 8601 format. Required when used in conjunction with the `TimeFilter` or `States` search parameter.|
| `ServiceIds` | array of string | required, max 1000 items | Unique identifiers of the [Services](services.md#service) from which the reservations are requested. |
| `ReservationIds` | array of string | optional, max 1000 items | Unique identifiers of the requested [Reservations](#reservation-ver-2017-04-12). |
| `GroupIds` | array of string | optional, max 1000 items | Unique identifiers of the requested [Reservation groups](#reservation-group). |
| `CustomerIds` | array of string | optional, max 1000 items | Unique identifiers of the [Customers](customers.md#customer) which own the reservations. |
| `AssignedResourceIds` | array of string | optional, max 1000 items | Unique identifiers of [Resources](resources.md#resource) assigned to the reservations. |
| `RateIds` | array of string | optional, max 1000 items | Unique identifiers of [Rates](rates.md#rate) assigned to the reservations. |
| `ChannelNumbers` | array of string | optional, max 1000 items | Set of numbers or references used by the Channel \(i.e. OTA, GDS, CRS, etc.\) in case the reservation group originates there, e.g. Booking.com confirmation numbers. |
| `BusinessSegmentIds` | array of string | optional, max 1000 items | Unique identifiers of [Business segments](businesssegments.md#business-segment) assigned to the reservations. |
| `Numbers` | array of string | optional, max 1000 items | Confirmation numbers of [Reservations](#reservation-ver-2017-04-12). |
| `States` | array of string [Reservation state](#reservation-state) | optional | States the reservations should be in. If not specified, reservations in `Confirmed`, `Started` or `Processed` states or reservations specified by `ReservationIds` regardless of state are returned. |
| `Extent` | [Reservation extent](#reservation-extent) | required | Extent of data to be returned. E.g. it is possible to specify that together with the reservations, customers, groups and rates should be also returned. |
| `Currency` | string | optional | ISO-4217 code of the [Currency](currencies.md#currency) the item costs should be converted to. |
| `Limitation` | [Limitation](../guidelines/pagination.md#limitation) | required | Limitation on the quantity of reservation data returned. |

#### Reservation time filter

* `Colliding` - reservations whose intervals collide with the specified interval.
* `Created` - reservations created within the specified interval.
* `Updated` - reservations updated within the specified interval.
* `Start`- reservations starting \(= arriving\) within the specified interval.
* `End` - reservations ending \(= departing\) within the specified interval.
* `Overlapping` - reservations whose intervals contain the specified interval.
* `Canceled` - reservations canceled within the specified interval.

#### Reservation extent

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `BusinessSegments` | bool | optional | Whether the response should contain business segmentation. |
| `Customers` | bool | optional | Whether the response should contain customers of the reservations. |
| `OrderItems` | bool | optional | Whether the response should contain reservation items. |
| `Products` | bool | optional | Whether the response should contain products orderable with the reservations. |
| `Rates` | bool | optional | Whether the response should contain rates and rate groups. |
| `Reservations` | bool | optional | Whether the response should contain reservations. |
| `ReservationGroups` | bool | optional | Whether the response should contain groups of the reservations. |
| `Services` | bool | optional | Whether the response should contain services reserved by the reservations. |
| `Resources` | bool | optional | Whether the response should contain resources. |
| `ResourceCategories` | bool | optional | Whether the response should contain resource categories. |
| `ResourceCategoryAssignments` | bool | optional | Whether the response should contain assignments of the resources to categories. |
| `Notes` | bool | optional | Whether the response should contain notes. |
| `QrCodeData` | bool | optional | Whether the response should contain QR code data. |
| `Companies` | bool | optional | Whether the response should contain companies. |
| `AccountingStates` | array of string [Accounting item state](accountingitems.md#accounting-item-state) | optional | States the items should be in. If not specified, items in `Open` or `Closed` states are returned. |

### Response

```javascript
{
    "BusinessSegments": null,
    "Customers": [
        {
            "Address": null,
            "BirthDate": null,
            "BirthPlace": null,
            "CategoryId": null,
            "Classifications": [],
            "CreatedUtc": "2016-01-01T00:00:00Z",
            "Email": null,
            "FirstName": "John",
            "Sex": null,
            "Id": "35d4b117-4e60-44a3-9580-c582117eff98",
            "IdentityCard": null,
            "LanguageCode": null,
            "LastName": "Smith",
            "LoyaltyCode": null,
            "NationalityCode": "US",
            "Notes": "",
            "Number": "1",
            "Passport": null,
            "Phone": "00420123456789",
            "SecondLastName": null,
            "TaxIdentificationNumber": null,
            "Title": null,
            "UpdatedUtc": "2016-01-01T00:00:00Z",
            "Visa": null
        }
    ],
    "OrderItems": null,
    "Products": null,
    "RateGroups": null,
    "Rates": null,
    "ReservationGroups": [
        {
            "Id": "c704dff3-7811-4af7-a3a0-7b2b0635ac59",
            "Name": "13-12-Smith-F712"
        }
    ],
    "Reservations": [
        {
            "Id": "bfee2c44-1f84-4326-a862-5289598f6e2d",
            "ServiceId": "bd26d8db-86da-4f96-9efc-e5a4654a4a94",
            "GroupId": "94843f6f-3be3-481b-a1c7-06458774c3df",
            "Number": "52",
            "ChannelNumber": "TW48ZP",
            "ChannelManager": "",
            "ChannelManagerGroupNumber": null,
            "ChannelManagerNumber": null,
            "State": "Processed",
            "Origin": "Connector",
            "OriginDetail": null,
            "Purpose": "Student",
            "CreatedUtc": "2016-02-20T14:58:02Z",
            "UpdatedUtc": "2016-02-20T14:58:02Z",
            "CancelledUtc": null,
            "StartUtc": "2016-02-20T13:00:00Z",
            "EndUtc": "2016-02-22T11:00:00Z",
            "ReleasedUtc": null,
            "RequestedCategoryId": "773d5e42-de1e-43a0-9ce6-f940faf2303f",
            "AssignedResourceId": "20e00c32-d561-4008-8609-82d8aa525714",
            "AssignedResourceLocked": false,
            "BusinessSegmentId": null,
            "CompanyId": null,
            "TravelAgencyId": null,
            "AvailabilityBlockId": null,
            "RateId": "ed4b660b-19d0-434b-9360-a4de2ea42eda",
            "VoucherId": null,
            "CustomerId": "35d4b117-4e60-44a3-9580-c582117eff98"
            "PersonCounts": [
                {
                    "AgeCategoryId": "1f67644f-052d-4863-acdf-ae1600c60ca0",
                    "Count": 2
                },
                {
                    "AgeCategoryId": "ab58c939-be30-4a60-8f75-ae1600c60c9f",
                    "Count": 2
                }
            ]
        }
    ],
    "Services": null,
    "Resources": null,
    "ResourceCategories": null,
    "ResourceCategoryAssignments": null,
    "Notes": null,
    "Cursor": "8d02142f-31cf-4115-90bf-ae5200c7a1ba"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `BusinessSegments` | array of [Business segment](businesssegments.md#business-segment) | optional | Business segments of the reservations. |
| `Customers` | array of [Customer](customers.md#customer) | optional | Customers that are members of the reservations. |
| `OrderItems` | array of [Order item](accountingitems.md#order-item) | optional | Revenue items of the reservations. |
| `Products` | array of [Product](products.md#product) | optional | Products orderable with reservations. |
| `RateGroups` | array of [Rate group](rates.md#rate-group) | optional | Rate groups of the reservation rates. |
| `Rates` | array of [Rate](rates.md#rate) | optional | Rates of the reservations. |
| `ReservationGroups` | array of [Reservation group](#reservation-group) | optional | Reservation groups that the reservations are members of. |
| `Reservations` | array of [Reservation](#reservation-ver-2017-04-12) | optional | The reservations that collide with the specified interval. |
| `Services` | array of [Service](services.md#service) | optional | Services that have been reserved. |
| `Resources` | array of [Resource](resources.md#resource) | optional | Assigned resources of the reservations. |
| `ResourceCategories` | array of [Resource category](resources.md#resource-category) | optional | Resource categories of the resources. |
| `ResourceCategoryAssignments` | array of [Resource category assignment](resources.md#resource-category-assignment) | optional | Assignments of the resources to categories. |
| `Notes` | array of [Order note](#order-note) | optional | Notes of the reservations. | 
| `QrCodeData` | array of [QrCode data](#qrcode-data) | optional | QR code data of the reservations. |
| `Cursor` | string | required | Unique identifier of the last and hence oldest reservation returned. This can be used in [Limitation](../guidelines/pagination.md#limitation) in a subsequent request to fetch the next batch of older reservations. |

#### Reservation (ver 2017-04-12)

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Id` | string | required | Unique identifier of the reservation. |
| `ServiceId` | string | required | Unique identifier of the [Service](services.md#service) that is reserved. |
| `GroupId` | string | required | Unique identifier of the [Reservation group](#reservation-group). |
| `Number` | string | required | Confirmation number of the reservation in Mews. |
| `ChannelNumber` | string | optional | Number of the reservation within the Channel \(i.e. OTA, GDS, CRS, etc\) in case the reservation group originates there \(e.g. Booking.com confirmation number\). |
| `ChannelManagerNumber` | string | optional | Unique number of the reservation within the reservation group. |
| `ChannelManagerGroupNumber` | string | optional | Number of the reservation group within a Channel manager that transferred the reservation from Channel to Mews. |
| `ChannelManager` | string | optional | Name of the Channel manager \(e.g. AvailPro, SiteMinder, TravelClick, etc\). |
| `State` | string [Reservation state](#reservation-state) | required | State of the reservation. |
| `Origin` | string [Reservation origin](#reservation-origin) | required | Origin of the reservation. |
| `OriginDetail`| string | optional | Details about the [reservation origin](#reservation-origin). |
| `Purpose` | string [Reservation purpose](#reservation-purpose) | optional | Purpose of the reservation. |
| `CreatedUtc` | string | required | Creation date and time of the reservation in UTC timezone in ISO 8601 format. |
| `UpdatedUtc` | string | required | Last update date and time of the reservation in UTC timezone in ISO 8601 format. |
| `CancelledUtc` | string | optional | Cancellation date and time in UTC timezone in ISO 8601 format. |
| `StartUtc` | string | required | Start of the reservation \(arrival\) in UTC timezone in ISO 8601 format. |
| `EndUtc` | string | required | End of the reservation \(departure\) in UTC timezone in ISO 8601 format. |
| `ReleasedUtc` | string | optional | Date when the optional reservation is released in UTC timezone in ISO 8601 format. |
| `RequestedCategoryId` | string | required | Identifier of the requested [Resource category](resources.md#resource-category). |
| `AssignedResourceId` | string | optional | Identifier of the assigned [Resource](resources.md#resource). |
| `AssignedResourceLocked` | bool | required | Whether the reservation is locked to the assigned [Resource](resources.md#resource) and cannot be moved. |
| `BusinessSegmentId` | string | optional | Identifier of the reservation [Business segment](businesssegments.md#business-segment). |
| `CompanyId` | string | optional | Identifier of the [Company](companies.md#company) on behalf of which the reservation was made. |
| `TravelAgencyId` | string | optional | Identifier of the [Company](companies.md#company) that mediated the reservation. |
| `AvailabilityBlockId` | string | optional | Unique identifier of the [Availability block](availabilityblocks.md#availability-block) the reservation is assigned to. |
| `RateId` | string | required | Identifier of the reservation [Rate](rates.md#rate). |
| `VoucherId` | string | optional | Unique identifier of the [Voucher](vouchers.md#voucher) that has been used to create reservation. |
| `CustomerId` | string | required | Unique identifier of the [Customer](customers.md#customer) who owns the reservation. |
| `PersonCounts` | array of [Age category](agecategories.md#age-category) | required | Number of people per age category the reservation was booked for. |

## ~~Get all reservation items~~

> ### Deprecated!
> This operation is [deprecated](../deprecations/README.md), please use [Get all order items](../operations/orderitems.md#get-all-order-items) instead.

Returns all revenue items associated with the specified reservations.

### Request

`[PlatformAddress]/api/connector/v1/reservations/getAllItems`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationIds": [
        "e6ea708c-2a2a-412f-a152-b6c76ffad49b"
    ],
    "Currency": "EUR",
    "AccountingStates": [
        "Closed"
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ReservationIds` | array of string | required, max 1000 items | Unique identifiers of the reservation. |
| `Currency` | string | optional | ISO-4217 code of the [Currency](currencies.md#currency) the item costs should be converted to. |
| `AccountingStates` | array of string [Accounting item state](accountingitems.md#Accounting-item-state) | optional | States the items should be in. If not specified, items in `Open` or `Closed` states are returned. |

### Response

```javascript
{
    "Reservations": [
        {
            "ReservationId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b",
            "OrderItems": [
                {
                    "Id": "784a29df-6196-4402-96a0-58695a881239",
                    "AccountId": "d73a087a-00c7-40cf-9372-314d0296334d",
                    "OrderId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b",
                    "BillId": "7fdc2957-bc08-44d3-ba2a-1c1760a84894",
                    "AccountingCategoryId": "0cf7aa90-736f-43e9-a7dc-787704548d86",
                    "Amount": {
                        "Currency": "EUR",
                        "NetValue": 19.38,
                        "GrossValue": 23.26,
                        "TaxValues": [
                            {
                                "Code": "UK-S",
                                "Value": 3.88
                            }
                        ],
                        "Breakdown": {
                            "Items": [
                                {
                                    "TaxRateCode": "UK-S",
                                    "NetValue": 19.38,
                                    "TaxValue": 3.88
                                }
                            ]
                        }
                    },
                    "RevenueType": "Service",
                    "ConsumedUtc": "2016-03-10T13:00:00Z",
                    "ClosedUtc": "2016-04-06T06:41:44Z",
                    "AccountingState": "Closed",
                    "Data": {
                        "Discriminator": "ProductOrder",
                        "Value": {
                            "ProductId": "a3f4e006-b3e4-4485-8a00-ad11015ac494"
                        }
                    }
                }
            ]
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Reservations` | array of [Reservation items](#reservation-items) | required | The reservations with their items. |

#### Reservation items

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `OrderItems` | array of [Order item](accountingitems.md#order-item) | required | The items associated with the reservation. |

## Price reservations

Returns prices of reservations with the specified parameters. Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/price`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ServiceId": "bd26d8db-86da-4f96-9efc-e5a4654a4a94",
    "Reservations": [
        {
            "Identifier": "1234",
            "StartUtc": "2018-01-01T14:00:00Z",
            "EndUtc": "2018-01-02T10:00:00Z",
            "RequestedCategoryId": "0a5da171-3663-4496-a61e-35ecbd78b9b1",
            "RateId": "33667cab-f17f-4089-ad07-c2cd50fa0df1",
            "Notes": "Test reservation",
            "ProductOrders": [
                {
                    "ProductId": "3dc5d79b-67ce-48ed-9238-47fcf5d1a59f"
                }
            ],
            "PersonCounts": [
                {
                    "AgeCategoryId": "1f67644f-052d-4863-acdf-ae1600c60ca0",
                    "Count": 2
                },
                {
                    "AgeCategoryId": "ab58c939-be30-4a60-8f75-ae1600c60c9f",
                    "Count": 2
                }
            ]
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ServiceId` | string | required | Unique identifier of the [Service](services.md#service) to be priced. |
| `Reservations` | array of [Reservation parameters](#reservation-parameters) | required | Parameters of the reservations to price. Note that `CustomerId` is not required when pricing reservations. |

### Response

```javascript
{
    "ReservationPrices": [
        {
            "Identifier": "1234",
            "Total": {
                "Currency": "GBP",
                "NetValue": 20,
                "GrossValue": 23
                "TaxValues": [
                    {
                       "Code":"UK-S",
                       "Value": 3
                    }
                ],
            }
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ReservationPrices` | array of [Reservation price](#reservation-price) | required | The reservation prices. |

#### Reservation price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Identifier` | string | optional | Identifier of the reservation within the transaction. |
| `Total` | [Currency value](currencies.md#currency-value) | required | Total price of the reservation. |

## Add reservations

Adds the specified reservations as a single group. If `GroupId` is specified, adds the reservations to an already existing group. Note that all reservations linked to an availability block must belong to the same reservation group.

### Request

`[PlatformAddress]/api/connector/v1/reservations/add`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ServiceId": "bd26d8db-86da-4f96-9efc-e5a4654a4a94",
    "GroupId": null,
    "GroupName": null,
    "SendConfirmationEmail": true,
    "Reservations": [
        {
            "Identifier": "1234",
            "State" : "Confirmed",
            "StartUtc": "2021-01-01T14:00:00Z",
            "EndUtc": "2021-01-03T10:00:00Z",
            "ReleasedUtc": null,
            "CustomerId": "e465c031-fd99-4546-8c70-abcf0029c249",
            "BookerId": "e465c031-fd99-4546-8c70-abcf0029c249",
            "RequestedCategoryId": "0a5da171-3663-4496-a61e-35ecbd78b9b1",
            "RateId": "a39a59fa-3a08-4822-bdd4-ab0b00e3d21f",
            "TravelAgencyId": null,
            "CompanyId": null,
            "Notes": "Test reservation",
            "TimeUnitAmount": null,
            "PersonCounts": [
                {
                    "AgeCategoryId": "1f67644f-052d-4863-acdf-ae1600c60ca0",
                    "Count": 2
                },
                {
                    "AgeCategoryId": "ab58c939-be30-4a60-8f75-ae1600c60c9f",
                    "Count": 2
                }
            ],
            "TimeUnitPrices": [
                {
                    "Index": 0,
                    "Amount": {
                        "Currency": "GBP",
                        "GrossValue": 20,
                        "TaxCodes": [ "UK-S" ]
                    }
                },
                {
                    "Index": 1,
                    "Amount": {
                        "Currency": "GBP",
                        "GrossValue": 30,
                        "TaxCodes": [ "UK-S" ]
                    }
                }
            ],
            "ProductOrders": [
                {
                    "ProductId": "2e9eb3fc-8a77-466a-9cd9-abcf002a2a01",
                    "StartUtc": "2021-01-02T00:00:00Z",
                    "EndUtc": "2021-01-03T00:00:00Z"
                }
            ],
            "AvailabilityBlockId": null,
            "VoucherCode": "SpringSale2021"
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ServiceId` | string | required | Unique identifier of the [Service](services.md#service) to be reserved. |
| `GroupId` | string | optional | Unique identifier of the [Reservation group](#reservation-group) where the reservations are added. If not specified, a new group is created. |
| `GroupName` | string | optional | Name of the [Reservation group](#reservation-group) which the reservations are added to. If `GroupId` is specified, this field is ignored. If not specified, the group name is automatically created. |
| `Reservations` | array of [Reservation parameters](#reservation-parameters) | required | Parameters of the new reservations. |
| `SendConfirmationEmail` | bool | optional | Whether the confirmation email is sent. Default value is `true`. |
| `CheckRateApplicability ` | bool | optional | Indicates whether the system will check and prevent a booking being made using a restricted rate, e.g. a private rate. The default is `true`, i.e. the system will normally check for this unless the property is set to `false`. |
| `CheckOverbooking` | bool | optional | Indicates whether the system will check and prevent a booking being made in the case of an overbooking, i.e. where there is an insufficient number of resources available to meet the request<sup>\*1</sup>. The default is `true`, i.e. the system will normally check for this unless the property is set to `false`. |

<sup>\*1</sup> Note that the calculation for whether there is sufficient resources or not depends on both the requested resource category and the setting configured for *Availability calculation strategy* - see [Bookable Service Settings](https://help.mews.com/s/article/set-up-a-bookable-service?language=en_US).

#### Reservation parameters

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Identifier` | string | optional | Identifier of the reservation within the transaction. |
| `State` | string [Reservation state](#reservation-state) | optional | State of the newly created reservation \(either `Optional`, `Enquired` or `Confirmed`\). If not specified, `Confirmed` is used. |
| `StartUtc` | string | required | Reservation start in UTC timezone in ISO 8601 format. |
| `EndUtc` | string | required | Reservation end in UTC timezone in ISO 8601 format. |
| `ReleasedUtc` | string | optional | Release date and time of an unconfirmed reservation in UTC timezone in ISO 8601 format. |
| `CustomerId` | string | required | Unique identifier of the [Customer](customers.md#customer) who owns the reservation. |
| `BookerId` | string | optional | Unique identifier of the [Customer](customers.md#customer) on whose behalf the reservation was made. |
| `RequestedCategoryId` | string | required | Identifier of the requested [Resource category](resources.md#resource-category). |
| `RateId` | string | required | Identifier of the reservation [Rate](rates.md#rate). |
| `TravelAgencyId` | string | optional | Identifier of the [Company](companies.md#company) that mediated the reservation. |
| `CompanyId` | string | optional | Identifier of the [Company](companies.md#company) on behalf of which the reservation was made. |
| `Notes` | string | optional | Additional notes. |
| `TimeUnitAmount` | [Amount](accountingitems.md#amount-parameters) | optional | Amount of each night of the reservation. |
| `TimeUnitPrices` | array of [Time unit amount parameters](#time-unit-amount-parameters) | optional | Prices for time units of the reservation. E.g. prices for the first or second night. |
| `ProductOrders` | array of [Product order parameters](products.md#product-order-parameters) | optional | Parameters of the products ordered together with the reservation. |
| `PersonCounts` | array of [Age category parameters](#age-category-parameters) | required | Number of people per age category the reservation was booked for. At least one category with valid count must be provided. |
| `CreditCardId` | string | optional | Identifier of [Credit card](creditcards.md#credit-card) belonging to [Customer](customers.md#customer) who owns the reservation. |
| `AvailabilityBlockId` | string | optional | Unique identifier of the [Availability block](availabilityblocks.md#availability-block) the reservation is assigned to. |
| `VoucherCode` | string | optional | Voucher code value providing access to specified private [Rate](rates.md#rate) applied to this reservation. |

#### Age category parameters

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `AgeCategoryId` | string | required | Unique identifier of the [Age category](agecategories.md#age-category). |
| `Count` | string | required | Number of people of a given age category. Only positive value is accepted. |

### Response

```javascript
{
    "Reservations": [
        {
            "Identifier": "1234",
            "Reservation":
            {
                "Id": "bfee2c44-1f84-4326-a862-5289598f6e2d",
                "ServiceId": "bd26d8db-86da-4f96-9efc-e5a4654a4a94",
                "GroupId": "94843f6f-3be3-481b-a1c7-06458774c3df",
                "Number": "52",
                "ChannelManager": "",
                "ChannelManagerGroupNumber": null,
                "ChannelManagerNumber": null,
                "ChannelNumber": null,
                "State": "Processed",
                "Origin": "Connector",
                "CreatedUtc": "2016-02-20T14:58:02Z",
                "UpdatedUtc": "2016-02-20T14:58:02Z",
                "CancelledUtc": null,
                "StartUtc": "2016-02-20T13:00:00Z",
                "EndUtc": "2016-02-22T11:00:00Z",
                "ReleasedUtc": null,
                "RequestedCategoryId": "773d5e42-de1e-43a0-9ce6-f940faf2303f",
                "AssignedResourceId": "20e00c32-d561-4008-8609-82d8aa525714",
                "AssignedResourceLocked": false,
                "BusinessSegmentId": null,
                "CompanyId": null,
                "TravelAgencyId": null,
                "AvailabilityBlockId": null,
                "RateId": "ed4b660b-19d0-434b-9360-a4de2ea42eda",
                "VoucherId": null,
                "CustomerId": "35d4b117-4e60-44a3-9580-c582117eff98",
                "CompanionIds": [
                    "b22bf671-ccdf-40aa-a7e6-b20a4f91d79a"
                ]
            }
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Reservations` | array of [Added reservation](#added-reservation) | required | The added reservations. |

#### Added reservation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Identifier` | string | optional | Identifier of the reservation within the transaction. |
| `Reservation` | [Reservation](#reservation-ver-2017-04-12) | required | The added reservation. |

## Update reservations

Updates information about the specified reservations. Note that if any of the fields are sent as `null`, it won't clear the field in Mews. If the `Value` within the object is sent as `null`, the field will be cleared in Mews. Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/update`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "Client": "Sample Client 1.0.0",
    "Reason": "Testing",
    "CheckOverbooking": true,
    "CheckRateApplicability": true,
    "Reprice": true,
    "ApplyCancellationFee": true,
    "ReservationUpdates": [
        {
            "ReservationId": "622605a9-2969-441f-9610-aa720099ae1c",
            "StartUtc": {
                "Value": "2019-10-01T14:00:00Z"
            },
            "EndUtc": {
                "Value": "2019-10-03T10:00:00Z"
            },
            "AssignedResourceId": {
                "Value": "16ce4335-2198-408b-8949-9722894a42fb"
            },
            "AssignedResourceLocked": {
                "Value": "false"
            },
            "ChannelNumber": null,
            "RequestedCategoryId": null,
            "TravelAgencyId": {
                "Value": null
            },
            "CompanyId": {
                "Value": "73ba34d1-f375-460c-bf2d-8a63e71677a6"
            },
            "BusinessSegmentId": null,
            "Purpose": {
                "Value": "Business"
            },
            "RateId": null,
            "BookerId": {
                "Value": "92923102-bf91-4a4a-8ee8-9dcb79c9d6de"
            },
            "TimeUnitPrices": {
                "Value": [
                    {
                        "Index": 0,
                        "Amount": {
                            "Currency": "GBP",
                            "GrossValue": 20,
                            "TaxCodes": [
                                "UK-S"
                            ]
                        }
                    },
                    {
                        "Index": 1,
                        "Amount": {
                            "Currency": "GBP",
                            "GrossValue": 30,
                            "TaxCodes": [
                                "UK-S"
                            ]
                        }
                    }
                ]
            },
            "PersonCounts": {
                "Value": [
                    {
                        "AgeCategoryId": "ab58c939-be30-4a60-8f75-ae1600c60c9f",
                        "Count": 2
                    },
                        {
                        "AgeCategoryId": "1f67644f-052d-4863-acdf-ae1600c60ca0",
                        "Count": 2
                    }
                ]
            },            
            "CreditCardId": {
                "Value": "5d802a8f-3238-42b2-94be-ab0300ab2b6c"
            },
            "AvailabilityBlockId": {
                "Value": "aaaa654a4a94-4f96-9efc-86da-bd26d8db"
            }
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `Reason` | string | optional | Reason for updating the reservation. Required when updating the price of the reservation. |
| `CheckRateApplicability ` | bool | optional | Indicates whether the system will check and prevent a booking being made using a restricted rate, e.g. a private rate. The default is `true`, i.e. the system will normally check for this unless the property is set to `false`. |
| `CheckOverbooking` | bool | optional | Indicates whether the system will check and prevent a booking being made in the case of an overbooking, i.e. where there is an insufficient number of resources available to meet the request<sup>\*1</sup>. The default is `true`, i.e. the system will normally check for this unless the property is set to `false`. |
| `Reprice` | bool | optional | Whether the price should be updated to latest value for date/rate/category combination set in Mews. If not specified, the reservation price is updated. |
| `ApplyCancellationFee` | bool | optional | Whether the cancellation fees should be applied according to rate cancellation policies. If not specified, the cancellation fees are applied. |
| `ReservationUpdates` | array of [Reservation updates](#reservation-updates) | required, max 1000 items | Array of properties to be updated in each reservation specified. |

<sup>\*1</sup> Note that the calculation for whether there is sufficient resources or not depends on both the requested resource category and the setting configured for *Availability calculation strategy* - see [Bookable Service Settings](https://help.mews.com/s/article/set-up-a-bookable-service?language=en_US).

#### Reservation updates
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `StartUtc` | [String update value](_objects.md#string-update-value) | optional | Reservation start in UTC timezone in ISO 8601 format. \(or `null` if the start time should not be updated). |
| `EndUtc` | [String update value](_objects.md#string-update-value) | optional | Reservation end in UTC timezone in ISO 8601 format. \(or `null` if the end time should not be updated). |
| `AssignedResourceId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the assigned [Resource](resources.md#resource). |
| `AssignedResourceLocked` | [Bool update value](_objects.md#bool-update-value) | optional | Whether the reservation should be locked to the assigned [Resource](resources.md#resource). Unlocking and assigning reservation to new [Resource](resources.md#resource) can be done in one call. |
| `ChannelNumber` | [String update value](_objects.md#string-update-value) | optional | Number of the reservation within the Channel (i.e. OTA, GDS, CRS, etc) in case the reservation group originates there (e.g. Booking.com confirmation number) \(or `null` if the channel number should not be updated). |
| `RequestedCategoryId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the requested [Resource category](resources.md#resource-category) \(or `null` if resource category should not be updated). |
| `TravelAgencyId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the [Company](companies.md#company) that mediated the reservation \(or `null` if travel agency should not be updated). |
| `CompanyId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the [Company](companies.md#company) on behalf of which the reservation was made \(or `null` if company should not be updated). |
| `BusinessSegmentId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the reservation [Business segment](businesssegments.md#business-segment) \(or `null` if the business segment should not be updated).|
| `Purpose` | [String update value](_objects.md#string-update-value) | optional | [Purpose](#reservation-purpose) of the reservation \(or `null` if the purpose should not be updated).|
| `RateId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the reservation [Rate](rates.md#rate) \(or `null` if the rate should not be updated). |
| `BookerId` | [String update value](_objects.md#string-update-value) | optional | Identifier of the [Customer](customers.md#customer) on whose behalf the reservation was made. \(or `null` if the booker should not be updated). |
| `TimeUnitPrices` | [Time unit amount update value](#time-unit-amount-update-value) | optional | Prices for time units of the reservation. E.g. prices for the first or second night. \(or `null` if the unit amounts should not be updated). |
| `PersonCounts` | array of [Person counts update value](#person-counts-update-value) | optional | Number of people per age category the reservation is for. Is supplied the person counts will be replaced. \(or `null` if the person counts should not be updated). |
| `CreditCardId` | [String update value](_objects.md#string-update-value) | optional | Identifier of [Credit card](creditcards.md#credit-card) belonging to [Customer](customers.md#customer) who owns the reservation.  \(or `null` if the credit card should not be updated). |
| `AvailabilityBlockId` | [String update value](_objects.md#string-update-value) | optional | Unique identifier of the [Availability block](availabilityblocks.md#availability-block) the reservation is assigned to. |

#### Time unit amount update value

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Value` | array of [Time unit amount parameters](#time-unit-amount-parameters) | required | Value which is to be updated. |

#### Time unit amount parameters

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `Index` | integer | required | Index of the unit. Indexing starts with `0`. E.g the first night of the reservation has index 0. |
| `Amount` | [Amount parameters](orders.md#amount-parameters) | required | Amount of the unit. |

#### Person counts update value

| Property | Type | Contract | Description |
| --- | --- | --- | --- |
| `Value` | array of [Age category parameters](#age-category-parameters) | required | Value which is to be updated. |

### Response

Same structure as in [Get all reservations](#get-all-reservations) operation.

## Confirm reservation

Marks all specified reservations as `Confirmed`. Succeeds only if all confirmation conditions are met \(the reservations have the `Optional` state\).

### Request

`[PlatformAddress]/api/connector/v1/reservations/confirm`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationIds": [
        "9af9d8b0-43ae-414d-80a8-abc1012a2a59"
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ReservationIds` | array of string | required | Unique identifier of the reservations to confirm. |
| `SendConfirmationEmail` | bool | optional | Wheter the confirmation email is sent. Default value is `true`. |

### Response

```javascript
{
    "ReservationIds": [
        "9af9d8b0-43ae-414d-80a8-abc1012a2a59"
    ]
}
```

## Start reservation

Marks a reservation as `Started` \(= checked in\). Succeeds only if all starting conditions are met \(the reservation has the `Confirmed` state, does not have start set to future, has an inspected room assigned etc\). Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/start`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ReservationId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ReservationId` | string | required | Unique identifier of the reservation to start. |

### Response

```javascript
{}
```

## Process reservation

Marks a reservation as `Processed` \(= checked out\). Succeeds only if all processing conditions are met \(the reservation has the `Started` state, balance of all reservation members is zero etc\). Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Conditions

- Reservation has already been checked in.
- Reservation isn't alredy in `Processed` state.
- Reservation can't be checked out sooner than last day of planned stay.
- The companion profiles of reservation are complete (details can be found in error message).
- If `AllowOpenBalance` set to `false`, all bills have to be closable (items on bills are either paid by current customer, or set to be paid by other customer). With `CloseBills` option set to `true` they can be automatically closed, when set to `false` they must be closed manually.
- If `AllowOpenBalance` set to `true`, `Notes` must be filled in.

### Request

`[PlatformAddress]/api/connector/v1/reservations/process`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ReservationId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b",
    "CloseBills": false,
    "AllowOpenBalance": false,
    "Notes": null
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ReservationId` | string | required | Unique identifier of the reservation to process. |
| `CloseBills` | bool | optional | Whether closable bills of the reservation members should be automatically closed. |
| `AllowOpenBalance` | bool | optional | Whether non-zero consumed balance of all reservation members is allowed. |
| `Notes` | string | optional | Required if `AllowOpenBalance` set to `true`. Used to provide reason for closing with unbalanced bill. |

### Response

```javascript
{}
```

## Cancel reservation

Cancels all reservation with specified identifiers. Succeeds only if the reservations are cancellable. Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/cancel`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ReservationIds": [
        "5ca70705-cbb7-48c4-8cc4-abb900aa278c"
    ],
    "PostCancellationFee": true,
    "Notes": "Cancellation through Connector API"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ReservationIds` | array of string | required | Unique identifiers of the reservation to cancel. |
| `PostCancellationFee` | boolean | required | Whether cancellation fees should be charged according to rate conditions. |
| `Notes` | string | required | Additional notes describing the reason for the cancellation. |

### Response

```javascript
{  
    "ReservationIds": [
        "5ca70705-cbb7-48c4-8cc4-abb900aa278c"
    ]
}
```

## Update reservation customer

Updates customer of a reservation. Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/updateCustomer`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ReservationId": "209d984d-4985-4efb-96ec-f6591fc597bf",
    "CustomerId": "35d4b117-4e60-44a3-9580-c582117eff98"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ReservationId` | string | required | Unique identifier of the reservation to be updated. |
| `CustomerId` | string | required | Unique identifier of the [Customer](customers.md#customer). |

### Response

```javascript
{}
```

## Update reservation interval

Updates reservation interval \(start, end or both\). Note this operation supports [Portfolio Access Tokens](../guidelines/multi-property.md).

### Request

`[PlatformAddress]/api/connector/v1/reservations/updateInterval`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "EnterpriseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "ReservationId": "209d984d-4985-4efb-96ec-f6591fc597bf",
    "StartUtc": "2017-08-12T15:00:00Z",
    "EndUtc": "2017-08-15T12:00:00Z",
    "ChargeCancellationFee": false
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `EnterpriseId` | string | optional | Unique identifier of the [Enterprise](enterprises.md#enterprise). Required when using a [Portfolio Access Token](../guidelines/multi-property.md), ignored otherwise. |
| `ReservationId` | string | required | Unique identifier of the reservation to be updated. |
| `StartUtc` | string | optional | New reservation start in UTC timezone in ISO 8601 format. |
| `EndUtc` | string | optional | New reservation end in UTC timezone in ISO 8601 format. |
| `ChargeCancellationFee` | boolean | required | Whether cancellation fee should be charged for potentially canceled nights. |

### Response

```javascript
{}
```

## Add reservation companion

Adds a customer as a companion to the reservation. Succeeds only if there is space for the new companion \(count of current companions is less than `AdultCount + ChildCount`\).

### Request

`[PlatformAddress]/api/connector/v1/reservations/addCompanion`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b",
    "CustomerId": "35d4b117-4e60-44a3-9580-c582117eff98"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `CustomerId` | string | required | Unique identifier of the [Customer](customers.md#customer). |

### Response

```javascript
{}
```

## Delete reservation companion

Removes customer companionship from the reservation. Note that the customer profile stays untouched, only the relation between the customer and reservation is deleted.

### Request

`[PlatformAddress]/api/connector/v1/reservations/deleteCompanion`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationId": "e6ea708c-2a2a-412f-a152-b6c76ffad49b",
    "CustomerId": "35d4b117-4e60-44a3-9580-c582117eff98"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `CustomerId` | string | required | Unique identifier of the [Customer](customers.md#customer). |

### Response

```javascript
{}
```

## Add reservation product

Adds a new product order of the specified product to the reservation.

### Request

`[PlatformAddress]/api/connector/v1/reservations/addProduct`

```javascript
{
    "ClientToken": "E0D439EE522F44368DC78E1BFB03710C-D24FB11DBE31D4621C4817E028D9E1D",
    "AccessToken": "C66EF7B239D24632943D115EDE9CB810-EA00F8FD8294692C940F6B5A8F9453D",
    "Client": "Sample Client 1.0.0",
    "ReservationId": "4d2aa234-5d30-472c-899f-ab45008c3479",
    "ProductId": "47312820-2268-4f5c-864d-aa4100ed82bc",
    "Count": 1,
    "StartUtc": "2021-01-02T00:00:00Z",
    "EndUtc": "2021-01-03T00:00:00Z",
    "UnitAmount": {
        "Currency": "GBP",
        "GrossValue": 10,
        "TaxCodes": [
            "UK-S"
        ]
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ClientToken` | string | required | Token identifying the client application. |
| `AccessToken` | string | required | Access token of the client application. |
| `Client` | string | required | Name and version of the client application. |
| `ReservationId` | string | required | Unique identifier of the reservation. |
| `ProductId` | string | required | Unique identifier of the [Product](products.md#product). |
| `Count` | int | required | The amount of the products to be added. Note that if the product is charged e.g. per night, count 1 means a single product every night. Count 2 means two products every night. |
| `StartUtc` | string | optional | Product start in UTC timezone in ISO 8601 format. For products with charging `Once` and `PerPerson` must be set to same value as `EndUtc`. |
| `EndUtc` | string | optional | Product end in UTC timezone in ISO 8601 format. For products with charging `Once` and `PerPerson` must be set to same value as `StartUtc`. |
| `UnitAmount` | [Amount parameters](orders.md#amount-parameters) | optional | Price of the product that overrides the price defined in Mews. |

### Response

```javascript
{
    "ItemIds": [
        "ff81fd7a-29ba-4160-8e22-ab4300f67b23"
    ]
}
```

#### Commander origin

* `InPerson`
* `Channel`
* `Phone`
* `Email`
* `Website`
* `Message`
* `CallCenter`
* ...