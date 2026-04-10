# Shopee Product Detail Task API

This repository provides a Python client for interacting with the Shopee Product Detail Task API, enabling asynchronous submission and retrieval of product detail raw data.

## Overview

The Shopee Product Detail Task API operates on an asynchronous workflow, designed to efficiently handle requests for product information. The process involves three main steps:

1.  **Submit a crawl task**: Initiate a product detail crawl by submitting a task to the API. This action returns a `batch_id` which is essential for tracking the task's progress.
2.  **Poll for task status**: Periodically query the API using the `batch_id` to check the status of the submitted task.
3.  **Retrieve raw data**: Once the task status indicates completion (`code = 0`), retrieve the raw product detail payload from the `data.source` field.

## API Details

-   **Base URL**: `https://api.bodapi.com`
-   **Authentication**: Requests require `X-API-Token` and `X-API-Secret` headers for identity, billing, rate limiting, and credential protection.
-   **Method Style**: `GET`
-   **Response Format**: `application/json`
-   **Version**: `sp/v1`

### Endpoints

#### Submit Task

-   **Endpoint**: `GET /sp/v1/submit/product_detail`
-   **Description**: Submits a new product detail crawl task. The returned `batch_id` should be stored for subsequent queries.

#### Query Result

-   **Endpoint**: `GET /sp/v1/query/product_detail`
-   **Description**: Retrieves the status and results of a previously submitted task using its `batch_id`.
    -   `code = 0`: Task completed successfully.
    -   `code = -1`: Task is still processing.
    -   `code = -2`: Service exception occurred.

### Common Parameters

| Name          | In     | Type   | Required | Description                                     |
| :------------ | :----- | :----- | :------- | :---------------------------------------------- |
| `shop_id`     | query  | string | yes      | Shopee shop ID                                  |
| `item_id`     | query  | string | yes      | Shopee item ID                                  |
| `country`     | query  | string | yes      | Site code: `id` / `tw` / `vn` / `th` / `ph` / `my` / `sg` / `br` / `mx` |
| `X-API-Token` | header | string | yes      | Permanent API token for identity and billing    |
| `X-API-Secret`| header | string | yes      | Rotatable API secret for credential protection  |

### Common Response Shape

| Field | Type    | Description                                                 |
| :---- | :------ | :---------------------------------------------------------- |
| `code`| integer | Business status code; `0` for success, negative for pending/failed |
| `msg` | string  | Server message                                              |
| `data`| object  | Business payload, contains `source` with raw product data   |

## Notes

-   The `data.source` field in the query endpoint contains the raw upstream product detail object. This object can be large and may change over time due to upstream updates.
-   If `batch_id` is omitted during task submission, the API defaults to using the current date.

## Pricing Plans

This product offers a free tier:

### Free

-   **Credits Included**: 100
-   **Billing Model**: Credit package

To subscribe or choose a plan, please visit the [pricing page](https://api.bodapi.com/pricing).


live

Jump to live

