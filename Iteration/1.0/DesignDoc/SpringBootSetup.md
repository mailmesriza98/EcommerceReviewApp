
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
