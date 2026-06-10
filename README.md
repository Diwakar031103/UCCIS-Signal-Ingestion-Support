# UCCIS-Signal-Ingestion-Support

## Overview

The UCCIS Signal Ingestion Service is a FastAPI-based signal layer that ingests raw zone data, normalizes it into a consistent schema, generates traceable identifiers, and exposes structured signals through REST APIs.

This service acts as a foundational ingestion component for downstream UCCIS workflows.

---

## Features

* FastAPI REST API
* Structured signal generation
* Traceable signal identifiers
* Dataset ingestion from JSON
* Signal normalization workflow
* Multi-zone support
* Multi-domain support
* Runtime logging
* Structured error handling

Supported domains:

* traffic
* water
* flooding

---

## Project Structure

```text
project/
│
├── app.py
├── trace.py
├── traffic.json
├── requirements.txt
└── README.md
```

---

## Requirements

```text
fastapi
uvicorn
```

---

## Running the Service

Start the FastAPI application:

```bash
python -m uvicorn app:app --reload
```

Server URL:

```text
http://127.0.0.1:8000
```

Swagger Documentation:

```text
http://127.0.0.1:8000/docs
```

---

## API Endpoints

### GET /signals

Returns all generated signals.

Example:

```http
GET /signals
```

Sample Response:

```json
[
  {
    "trace_id": "TRACE_1001",
    "zone_id": "zone_1",
    "domain": "traffic",
    "timestamp": "2026-06-09T10:00:00Z",
    "metrics": {
      "traffic_density": 85,
      "violations": 11
    }
  }
]
```

---

### GET /signal?zone_id=<zone_id>

Returns signal for a specific zone.

Example:

```http
GET /signal?zone_id=zone_1
```

---

## Signal Generation Flow

```text
Data Source
    ↓
Data Parser
    ↓
Data Normalizer
    ↓
Trace ID Generation
    ↓
Structured Signal Output
```

---

## Signal Schema

Example generated signal:

```json
{
  "trace_id": "TRACE_1001",
  "zone_id": "zone_1",
  "domain": "traffic",
  "timestamp": "2026-06-09T10:00:00Z",
  "metrics": {
    "traffic_density": 85,
    "violations": 11
  }
}
```

---

## Dataset Example

Source: traffic.json

```json
[
  {
    "zone_id": "zone_1",
    "domain": "traffic",
    "traffic_density": 85,
    "violations": 11
  },
  {
    "zone_id": "zone_2",
    "domain": "water",
    "traffic_density": 45,
    "violations": 3
  },
  {
    "zone_id": "zone_3",
    "domain": "flooding",
    "traffic_density": 92,
    "violations": 18
  }
]
```

---

## Trace ID Generation

Implemented in trace.py:

```python
counter = 1000

def generate_trace_id():
    global counter
    counter += 1
    return f"TRACE_{counter}"
```

Example output:

```text
TRACE_1001
TRACE_1002
TRACE_1003
```

---

## Error Handling

Invalid zone requests return a structured error response.

Example:

```json
{
  "error": "INVALID_ZONE",
  "trace_id": "TRACE_1005"
}
```

Handled conditions:

* Invalid zone_id
* Missing records
* Dataset lookup failures

---

## Logging

Each generated signal is logged during runtime.

Example:

```bash
Signal generated → TRACE_1001 → payload
```

This provides basic observability and traceability.

---

## Outcome

The service provides:

* Reliable signal ingestion
* Structured signal generation
* Multi-zone processing
* Runtime observability
* Traceable signal flow
* FastAPI-based API access

This serves as a foundation for future UCCIS signal-processing workflows.
