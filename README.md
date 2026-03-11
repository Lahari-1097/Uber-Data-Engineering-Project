# Uber Real-Time Data Engineering Project

An end-to-end **Azure-based real-time data engineering project** that simulates Uber ride bookings, streams events through **Azure Event Hubs**, orchestrates ingestion with **Azure Data Factory**, processes streaming data using **Azure Databricks and PySpark Structured Streaming**, and models the final data into a **STAR schema** for analytics and reporting.

---

# Project Overview

This project demonstrates how to build a modern streaming data pipeline for ride-booking data using Azure services. It starts with a **FastAPI web application** that generates realistic Uber ride events using synthetic data. Each ride event is published to **Azure Event Hubs**, then processed downstream through Azure-native services for ingestion, storage, streaming transformation, dimensional modeling, and analytics.

This project showcases both:

- **Application-side event generation**
- **Cloud-side real-time data engineering architecture**

---


# High-Level Architecture

The project follows this end-to-end flow:

1. **FastAPI Web Application**
   - Simulates Uber ride bookings
   - Generates synthetic ride confirmation events

2. **Azure Event Hubs**
   - Receives JSON ride events in real time
   - Acts as the streaming ingestion layer

3. **Azure Data Factory**
   - Orchestrates ingestion and data movement workflows
   - Supports pipeline scheduling and monitoring

4. **Azure Data Lake**
   - Stores raw and staged event data
   - Acts as the persistent storage layer

5. **Azure Databricks**
   - Reads and transforms streaming data using PySpark
   - Performs scalable event processing and enrichment

6. **Dimensional Modeling**
   - Implements Slowly Changing Dimensions (SCD)
   - Builds a STAR schema for reporting and analytics

---

# Key Features

- Real-time event-driven architecture
- FastAPI-based booking simulation UI
- Synthetic Uber ride event generation
- Azure Event Hubs integration
- Azure Data Factory ingestion pipelines
- Azure Databricks + PySpark Structured Streaming
- Metadata-driven streaming logic
- Slowly Changing Dimensions (SCD)
- STAR schema dimensional modeling
- Analytics-ready design

---

# Tech Stack

## Programming & Frameworks

- Python 3.12+
- FastAPI
- Uvicorn
- Jinja2
- Faker
- python-dotenv

## Azure Services

- Azure Event Hubs
- Azure Data Factory
- Azure Data Lake
- Azure Databricks

## Processing & Analytics

- PySpark
- Structured Streaming
- Slowly Changing Dimensions (SCD)
- STAR Schema

---

# Repository Structure

```text
Uber_Data_Engineer_Project/
│
├── Code_Files/                # Project notebooks / processing assets
├── Data/                      # Data files used in the project
├── templates/                 # Jinja2 HTML templates for FastAPI UI
│
├── api.py                     # FastAPI application entry point
├── connection.py              # Azure Event Hub producer logic
├── data.py                    # Synthetic Uber ride data generator
├── files_array.json           # Metadata/config mapping file
├── architecture.png           # Architecture diagram
├── Uber_Project.svg           # Visual project asset
├── pyproject.toml             # Project metadata and dependencies
├── requirements.txt           # Python dependencies
└── README.md                  # Project documentation
```

---

# How the Project Works

## 1. FastAPI Web Application

The project starts with a lightweight **FastAPI** application.

### Routes

- `/` → Displays the home page
- `/book` → Generates a ride event and sends it to Azure Event Hubs

When the user triggers the booking flow, the application:

1. Generates a synthetic Uber ride record
2. Sends the ride event to Azure Event Hubs
3. Returns a booking confirmation page

---

## 2. Synthetic Ride Data Generation

The `data.py` module generates realistic Uber ride events using Python and Faker.

### Example attributes generated

- Ride ID
- Confirmation number
- Passenger ID
- Driver ID
- Vehicle ID
- Pickup and dropoff locations
- Booking, pickup, and dropoff timestamps
- Distance and duration
- Vehicle type and make
- Payment method
- Ride status
- Fare breakdown
- Ratings
- Cancellation details

### Dimension-style reference data included

- Vehicle types
- Vehicle makes
- Payment methods
- Ride statuses
- Cities
- Cancellation reasons

This structure makes the raw event payload suitable for downstream transformation and dimensional modeling.

---

## 3. Azure Event Hubs Publishing

The `connection.py` module sends generated ride events to **Azure Event Hubs**.

### Process

1. Load environment variables from `.env`
2. Read `CONNECTION_STRING`
3. Read `EVENT_HUBNAME`
4. Create an `EventHubProducerClient`
5. Convert ride data into JSON
6. Create an event batch
7. Send the batch to Azure Event Hubs

This simulates how real-world applications publish operational events into a streaming platform.

---

## 4. Azure Data Factory Ingestion

**Azure Data Factory** is used to create orchestration and ingestion workflows.

### Typical responsibilities

- Move data between services
- Schedule and monitor workflows
- Parameterize ingestion pipelines
- Support landing data into storage
- Coordinate downstream processing

This layer acts as the orchestration backbone of the pipeline.

---

## 5. Azure Databricks + PySpark Structured Streaming

**Azure Databricks** provides the compute layer for scalable event processing.

### PySpark Structured Streaming is used to

- Read streaming or landed event data
- Apply schemas to JSON payloads
- Standardize data types
- Enrich records with business logic
- Process events continuously or in micro-batches
- Build reusable transformation pipelines

---

## 6. Metadata-Driven PySpark Streaming

The project introduces a **metadata-driven processing approach**, where configuration controls transformation behavior instead of hardcoded logic.

### Benefits

- Reusable design
- Easier onboarding of new datasets
- Better maintainability
- Reduced code duplication
- More scalable enterprise-style architecture

---

## 7. Slowly Changing Dimensions (SCD)

The project implements **Slowly Changing Dimensions** to preserve historical changes in descriptive attributes.

### Why SCD matters

Dimension attributes may change over time such as:

- Driver details
- Vehicle assignments
- Payment classifications
- Ride categories
- Geographic mappings

SCD techniques allow historical tracking of these changes.

---

## 8. STAR Schema Data Model

The final stage organizes transformed data into a **STAR schema** for reporting and analytics.

### Fact Table

The fact table can include measures such as:

- Total fare
- Subtotal
- Tip amount
- Trip distance
- Trip duration
- Surge multiplier
- Booking timestamp
- Pickup timestamp
- Dropoff timestamp

### Dimension Tables

Example dimension tables include:

- `dim_vehicle_type`
- `dim_vehicle_make`
- `dim_payment_method`
- `dim_ride_status`
- `dim_city`
- `dim_cancellation_reason`

This structure enables efficient analytics and BI reporting.

---

# End-to-End Workflow

```text
User books ride from web app
        ↓
FastAPI generates synthetic Uber ride event
        ↓
JSON event is sent to Azure Event Hubs
        ↓
Azure Data Factory orchestrates ingestion
        ↓
Data lands in Azure Data Lake / staging layer
        ↓
Azure Databricks reads and transforms data
        ↓
PySpark Structured Streaming processes events
        ↓
SCD logic applied to dimension tables
        ↓
STAR schema fact and dimension tables created
        ↓
Analytics-ready data available for reporting
```

---

# Example Event Payload

```json
{
  "ride_id": "uuid",
  "confirmation_number": "AB1-1234-CD56",
  "passenger_id": "uuid",
  "driver_id": "uuid",
  "vehicle_id": "uuid",
  "vehicle_type_id": 1,
  "vehicle_make_id": 2,
  "payment_method_id": 3,
  "ride_status_id": 1,
  "pickup_city_id": 1,
  "dropoff_city_id": 2,
  "cancellation_reason_id": 4,
  "passenger_name": "John Doe",
  "driver_name": "Jane Smith",
  "pickup_address": "123 Main St, New York, NY",
  "dropoff_address": "456 Park Ave, Chicago, IL",
  "distance_miles": 12.5,
  "duration_minutes": 34,
  "booking_timestamp": "2026-03-01T10:15:00",
  "pickup_timestamp": "2026-03-01T10:20:00",
  "dropoff_timestamp": "2026-03-01T10:54:00",
  "base_fare": 2.5,
  "distance_fare": 21.88,
  "time_fare": 11.9,
  "surge_multiplier": 1.45,
  "subtotal": 52.45,
  "tip_amount": 4.0,
  "total_fare": 56.45,
  "rating": 5
}
```

---

# Setup Instructions

## 1. Clone the Repository

```bash
git clone https://github.com/anshlambagit/Uber_Data_Engineer_Project.git
cd Uber_Data_Engineer_Project
```

## 2. Create Virtual Environment

```bash
python -m venv .venv
```

Activate the virtual environment:

**Windows**

```bash
.venv\Scripts\activate
```

**Mac / Linux**

```bash
source .venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Or using `uv`:

```bash
pip install uv
uv sync
```

---

## 4. Create `.env` File

Create a `.env` file in the project root:

```env
CONNECTION_STRING=your_azure_event_hub_connection_string
EVENT_HUBNAME=your_event_hub_name
```

---

## 5. Run the FastAPI Application

```bash
python api.py
```

Or run using Uvicorn:

```bash
uvicorn api:app --host 0.0.0.0 --port 8000 --reload
```

---

## 6. Open in Browser

```
http://127.0.0.1:8000
```

Trigger the booking flow to generate a ride event and send it to Azure Event Hubs.

---

# Main Python Modules

## `api.py`

Responsible for:

- FastAPI application setup
- Route definitions
- HTML template rendering
- Booking event trigger

## `data.py`

Responsible for:

- Synthetic Uber ride event generation
- Fare calculation logic
- Ride metadata creation
- Lookup and mapping values

## `connection.py`

Responsible for:

- Azure Event Hub connection
- JSON event serialization
- Event batch creation
- Event publishing

---

# Business Questions This Pipeline Can Answer

Once processed and modeled, the pipeline can answer:

- Which cities have the highest ride volume?
- What vehicle types generate the most revenue?
- What are peak booking hours?
- What payment methods are most common?
- How does surge pricing affect revenue?
- What is the cancellation rate by city?

---

# Why This Project Matters

This project demonstrates important real-world data engineering concepts including:

- Event-driven architecture
- Real-time streaming pipelines
- Cloud-native data platforms
- Data orchestration
- Scalable distributed processing
- Historical dimension management
- Analytics-ready data modeling

It showcases skills in:

- Azure Data Engineering
- Databricks
- PySpark Structured Streaming
- Data Modeling
- End-to-End Data Pipeline Design

---

# Future Enhancements

Potential improvements include:

- Adding consumer applications for downstream processing
- Persisting curated outputs into Delta Lake tables
- Building Power BI dashboards
- Adding CI/CD pipelines
- Implementing data quality validation
- Expanding metadata-driven ingestion
- Implementing bronze, silver, gold data layers
- Supporting replay and backfill workflows

---

# Learning Outcomes

By completing this project, you will learn:

- How web applications generate real-time events
- How Azure Event Hubs handles streaming ingestion
- How Azure Data Factory orchestrates pipelines
- How Databricks processes streaming data at scale
- How Slowly Changing Dimensions preserve historical data
- How STAR schema enables analytics and reporting



