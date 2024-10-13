
# Spring Boot Setup for Review and Moderation Services

## Overview

This document provides a detailed overview of how to set up the **Review** and **Moderation** services in a single **Spring Boot** application. These services interact using **Kafka** as a messaging queue. We'll also cover how to handle **MySQL** for persistent storage and **Redis** for caching. The services will be deployed using **Docker** for containerization.

## Services Overview

1. **Review Service**: Handles CRUD operations for product reviews and interacts with the Moderation service for review validation.
2. **Moderation Service**: Processes reviews sent for moderation via a Kafka message queue.

---

## Review Service Endpoints

### 1. GET Reviews
**Endpoint**: `/reviews`

**Method**: `GET`

**Description**: Fetches reviews for a given product ID, optionally paginated.

**Query Parameters**:
- `product_id`: ID of the product (required)
- `page`: Page number (optional, default is 1)

**Sample Request**:
```bash
GET /reviews?product_id=123&page=1
```

which returns
```json
[
  {
    "review_id": 1,
    "product_id": "123",
    "title": "Great product",
    "text": "Loved the quality!",
    "rating": 5,
    "user_id": "456"
  },
  ...
]
```

### 2. POST Review
Endpoint: /reviews

Method: POST

Description: Submits a new product review. After the review is posted, it is sent to the Moderation service for approval via Kafka.

Request Body:

```json
{
  "product_id": "123",
  "title": "Great product",
  "text": "The product was amazing!",
  "rating": 5,
  "user_id": "456"
}
```

Response:
```json
{
  "status": "Review submitted for moderation"
}
```
### 3. PUT Review
Endpoint: /reviews/{review_id}

Method: PUT

Description: Updates an existing review.

Request Body:
```json
{
  "title": "Updated title",
  "text": "Updated review text",
  "rating": 4
}
```

```json
{
  "status": "Review updated successfully"
}
```

### 4. DELETE Review
Endpoint: /reviews/{review_id}

Method: DELETE

Description: Deletes a review.

Response:

```json
{
  "status": "Review deleted"
}
```

##Moderation Service Endpoints

### 1. POST Moderation Decision
Endpoint: /moderation/reviews/{review_id}

Method: POST

Description: Receives the moderation decision for a review. The Moderation service is triggered via Kafka messages.

Request Body:

```json
{
  "decision": "approved" | "rejected"
}
```

Response:

```json
{
  "status": "Moderation decision recorded"
}
```

