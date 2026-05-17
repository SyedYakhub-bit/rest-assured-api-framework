# REST Assured API Testing Framework

A comprehensive API test automation framework built with **Java**, **REST Assured**, **TestNG**, and **Cucumber BDD** — covering everything from basic REST operations to advanced patterns like OAuth 2.0, POJO serialization, session management, and file uploads.

---

## Table of Contents

- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Features Covered](#features-covered)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Running Tests](#running-tests)
- [Test Modules](#test-modules)
- [Framework Architecture](#framework-architecture)
- [Reporting](#reporting)
- [Configuration](#configuration)

---

## Tech Stack

| Technology | Version | Purpose |
| --- | --- | --- |
| Java | 8 | Primary language |
| REST Assured | 5.4.0 | API testing library |
| TestNG | 7.10.2 | Test execution framework |
| Cucumber | 7.20.0 | BDD framework |
| Maven | 3.x | Build and dependency management |
| Jackson | 2.16.0 | JSON serialization / deserialization |
| Gson | 2.10.1 | JSON parsing |
| Selenium | 4.24.0 | Web automation support |
| Hamcrest | 2.2 | Assertion matchers |
| Maven Cucumber Reporting | 5.8.2 | HTML test reports |

---

## Project Structure

```
rest-assured-api-framework/
│
├── data/
│   └── commonData.properties        # OAuth credentials, base URLs
│
├── src/
│   ├── main/java/
│   │   ├── Basics.java              # Core Place API operations (Add/Update/Get)
│   │   ├── ECommerceApi.java        # Full e-commerce flow (login → product → order)
│   │   ├── OAuth.java               # OAuth 2.0 token generation + secured endpoint
│   │   ├── OAuthRealExample.java    # Alternative OAuth implementation
│   │   ├── JiraTest.java            # Jira REST API (sessions, comments, issues)
│   │   ├── AddBooks.java            # Library API with @DataProvider
│   │   ├── libraryapi.java          # Single book add test
│   │   ├── ComplexJsonParse.java    # Complex nested JSON extraction
│   │   ├── ExternalPayload.java     # Payload loaded from file system
│   │   ├── LoginWebsite.java        # Simple website GET test
│   │   ├── TeamsMessageSender.java  # MS Teams webhook integration
│   │   │
│   │   ├── Files/
│   │   │   └── payload.java         # Static JSON payload templates
│   │   │
│   │   ├── Pojo/                    # POJOs for course API responses
│   │   │   ├── GetCourses.java
│   │   │   ├── Courses.java
│   │   │   ├── Api.java
│   │   │   ├── Mobile.java
│   │   │   ├── WebAutomation.java
│   │   │   ├── LoginRequest.java
│   │   │   └── LoginResponse.java
│   │   │
│   │   ├── Serialization/           # Serialization examples + SpecBuilder
│   │   │   ├── AddPlaceSerialization.java
│   │   │   ├── Location.java
│   │   │   ├── Serialization.java
│   │   │   └── SpecBuilder.java
│   │   │
│   │   └── supportfiles/
│   │       └── PropertiesReader.java  # Reads commonData.properties
│   │
│   └── test/java/
│       ├── resources/
│       │   ├── APIResources.java      # Enum: all API endpoint paths
│       │   ├── TestDataBuild.java     # Test data / payload builders
│       │   └── Utility.java          # Base request setup + JSON extractor
│       │
│       ├── features/
│       │   └── place_validation.feature  # Cucumber BDD scenarios
│       │
│       ├── stepdefinitions/
│       │   └── AddPlaceTest.java         # Cucumber step definitions
│       │
│       └── testRunner/
│           └── AddPlaceTestRunner.java   # TestNG-Cucumber runner
│
├── pom.xml
└── README.md
```

---

## Features Covered

- **REST Operations** — GET, POST, PUT, DELETE with full request/response validation
- **BDD with Cucumber** — Feature files, scenario outlines, data tables, step definitions
- **POJO Serialization** — Jackson-based object ↔ JSON conversion (request and response)
- **OAuth 2.0** — Token generation and authenticated API calls
- **Session Management** — Stateful API testing using `SessionFilter` (Jira API)
- **File Uploads** — Multipart form-data with file attachments (E-commerce API)
- **Request / Response Spec Builders** — Reusable `RequestSpecBuilder` and `ResponseSpecBuilder`
- **DataProvider** — Parameterized testing with multiple data sets (TestNG)
- **External Payloads** — Loading JSON request bodies from `.json` files
- **Complex JSON Parsing** — JsonPath extraction from deeply nested responses
- **Properties-based Config** — Externalised base URLs and credentials
- **Request/Response Logging** — All traffic captured to `logging.txt`
- **Enum-based Endpoints** — Centralized `APIResources` enum for all API paths
- **Cucumber HTML Reporting** — Auto-generated reports via maven-cucumber-reporting

---

## Prerequisites

- Java 8+
- Maven 3.6+
- Internet access (tests run against public APIs on rahulshettyacademy.com)

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/SyedYakhub-bit/rest-assured-api-framework.git
cd rest-assured-api-framework

# Install dependencies
mvn clean install -DskipTests
```

---

## Running Tests

| Command | Description |
| --- | --- |
| `mvn clean test` | Run all tests (Cucumber + TestNG) |
| `mvn test -Dcucumber.filter.tags="@regression"` | Run regression-tagged Cucumber scenarios |
| `mvn test -Dcucumber.filter.tags="@add_place"` | Run add-place scenarios only |
| `mvn test -Dcucumber.filter.tags="@delete_place"` | Run delete-place scenarios only |

Test reports are generated at:
```
target/cucumber-html-reports/overview-features.html
target/jsonReports/cucumber-report.json
```

---

## Test Modules

### Place API (Google Maps-style)
- Add, update, get, and delete place records
- BDD scenarios with Cucumber + Scenario Outline (3 data sets)
- POJO-based request building and response deserialization

### E-Commerce API
Full end-to-end flow against Rahul Shetty Academy's e-commerce API:
1. Login → retrieve auth token
2. Create product with image upload (multipart)
3. Create order referencing the new product
4. Delete product
5. Password reset flow

### OAuth 2.0
- Fetch access token using client credentials from `commonData.properties`
- Call secured `/getCourseDetails` endpoint with Bearer token
- Parse complex nested JSON (courses grouped by category)

### Library API
- Add and delete books using TestNG `@DataProvider` with 3 test records
- Endpoint: `http://216.10.245.166/Library/`

### Jira API
- Create session via `/rest/auth/1/session`
- Add comment to an issue
- Retrieve issue details
- Uses `SessionFilter` for session persistence across requests

### Complex JSON Parsing
- Parses deeply nested course response JSON
- Validates individual course prices sum to expected total

---

## Framework Architecture

```
┌─────────────────────────────────────────────────────┐
│                    Test Layer                        │
│   Cucumber Feature Files  |  TestNG Test Classes     │
└────────────────┬────────────────────┬────────────────┘
                 │                    │
┌────────────────▼────────────────────▼────────────────┐
│              Support Layer                           │
│  Utility.java (base setup) | TestDataBuild.java      │
│  APIResources.java (enums) | PropertiesReader.java   │
└────────────────┬────────────────────────────────────-┘
                 │
┌────────────────▼─────────────────────────────────────┐
│              Model Layer                             │
│  POJOs (request/response) | Payload templates        │
│  Serialization helpers    | Location / Place models  │
└────────────────┬─────────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────────┐
│              Config Layer                            │
│  data/commonData.properties  |  pom.xml versions     │
└──────────────────────────────────────────────────────┘
```

---

## Reporting

This framework uses **maven-cucumber-reporting** to produce rich HTML reports automatically after each `mvn test` run.

Open the report:
```
target/cucumber-html-reports/overview-features.html
```

The report includes:
- Feature-level pass/fail summary
- Step-by-step execution breakdown
- Tag-based filtering
- Trend charts (when run multiple times)

---

## Configuration

Edit `data/commonData.properties` to configure OAuth credentials and base URLs:

```properties
client_id=<your_client_id>
client_secret=<your_client_secret>
grant_type=password
scope=trust
baseURLForAccessToken=https://rahulshettyacademy.com/oauthapi/oauth2/resourceOwner/token
baseURLRahulShetty=https://rahulshettyacademy.com
```

---

## Author

**Syed Yakhub** — [GitHub](https://github.com/SyedYakhub-bit)

Built as part of a structured API test automation learning path, progressively advancing from basic REST calls to a full BDD-driven framework with real-world integrations.
