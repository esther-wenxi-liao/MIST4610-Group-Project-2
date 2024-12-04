# Team Name and Members
### Team name:

AKEA


### Members:

1. Andrew Hancock [@AndyH775](https://github.com/AndyH775)
2. Kaniyah McGee [@Kmcgee2026](https://github.com/Kmcgee2026)
3. Esther Liao [@esther-wenxi-liao](https://github.com/esther-wenxi-liao)
4. Alexis Williams ​[@AlexisWilliams26](https://github.com/AlexisWilliams26)

# Project Scenario

Expanding the Atlanta Airport Database to a Global Scale

## Background
This project builds upon the foundation established in **Project 1**, which focused solely on one day of domestic flights in and out of Atlanta's Hartsfield-Jackson International Airport. Recognizing the growing importance of Atlanta as a global hub, the database has been significantly expanded to include all domestic and international flights from airports worldwide. This evolution reflects the complexities of modern air travel and supports advanced decision-making through data-driven insights.

## Objectives
As Atlanta grows into a major international hub, the airport and its stakeholders aim to:
1. **Manage International Flight Routes Effectively:** Expand beyond domestic flights to handle the complexities of international routes, including time zones, layovers, and customs regulations.
2. **Optimize Employee Assignments:** Assign staff to a broader range of roles, ensuring efficient operations across domestic and international flights.
3. **Enhance Decision-Making with Analytics:** Gain actionable insights into revenue trends, flight delays, and passenger patterns for strategic planning.
4. **Leverage Data Visualization:** Present operational trends and performance metrics to stakeholders in an intuitive and impactful manner.

## General Changes to the Database
1. **Expanded Relationships:**
   - Added a **many-to-one relationship** between `Flight` and `Airport` to remove the restriction of flights limited to Atlanta. Now, flights can originate or terminate at any airport globally.
   - Updated schemas to accommodate new fields relevant to international airports (e.g., time zones, customs requirements).

2. **Updated Schema for International Operations:**
   - Incorporated **foreign airports** to manage international routes effectively.
   - Expanded the `Airline` and `Flight` tables to accommodate non-US-specific attributes, ensuring compatibility with international standards.

### Key Changes and Enhancements in Project 2

1. **Expansion of Flight Routes**
   - **Project 1**: The `Flight` table was limited to domestic flights originating from Atlanta Airport.
   - **Project 2**: The `Flight` table now accommodates **both domestic and international flights**:
     - Added the `flightType` column (ENUM) to specify whether the flight is Domestic or International.
     - Expanded relationships with the `Airport` table to include **departure (`departAirport`)** and **destination (`destinAirport`)** airports, enabling flights from/to any location worldwide.

2. **Airport Table**
   - **Project 1**: Contained only basic information for local airports relevant to domestic operations.
   - **Project 2**: 
     - Enhanced to include **international airports**.
     - Added the `country` field to support global operations, providing more context for international routes.

3. **Enhancements in the Flight Table**
   - Several new fields were added to the `Flight` table in **Project 2**:
     - `delayMinutes`: Tracks the duration of flight delays in minutes.
     - `delayReason`: Records the reason for the delay, allowing for more detailed operational insights.
     - `status`: An ENUM field that tracks the flight’s current status, such as `On-Time`, `Delayed`, or `Cancelled`.
     - `codeshare`: A TINYINT field to indicate whether the flight is part of a **codeshare agreement** between airlines. This is critical for managing shared international routes.

4. **Passenger and Ticket Data**
   - **Project 1**: The `Passenger` table only included fields for domestic travel, and the `Ticket` table was basic.
   - **Project 2**:
     - **Passenger Table**:
       - Added `citizenship` and `passportNum` fields to support **international passenger information**.
       - Retained essential fields like `priorityBoard` and `email` for operational needs.
     - **Ticket Table**:
       - Expanded with fields like `class` (ENUM) to classify ticket types (e.g., Economy, Business).
       - Added `seatNum` to track seat assignments for passengers.

5. **Aircraft and Plane Model Enhancements**
   - **Project 1**: The `Plane` and `PlaneModel` tables supported basic domestic flight data.
   - **Project 2**:
     - **Plane Table**:
       - Expanded to include relationships with the `Airline` table through `airlineCode`.
     - **PlaneModel Table**:
       - Added the `maxDist` field to specify the maximum distance the aircraft can travel, supporting international operations.
       - Retained fields for `capacity` and manufacturer details.

6. **Crew and Employee Data**
   - **Project 1**: Limited to basic employee data with generic roles.
   - **Project 2**:
     - **AirCrew Table**:
       - Expanded with a `roleOnBoard` field to specify the crew’s role on each flight (e.g., Pilot, Co-Pilot, Attendant).
     - **Employee Table**:
       - Enhanced with the `position` ENUM field to support international-specific roles, such as multilingual staff or customs officers.
       - The `flightHour` field tracks the total flight hours worked by employees, ensuring compliance with work regulations.

7. **Certifications**
   - **Project 1**: The `Certificate` table only tracked basic certifications for employees.
   - **Project 2**:
     - Enhanced to include certifications specific to international operations and specialized aircraft models.
     - Retains the relationship between employees and plane models.

8. **Operational Insights**
   - **Project 1**: Provided minimal metrics for operational analysis.
   - **Project 2**:
     - Introduced fields like `delayReason` and `codeshare` to enable better tracking and analysis of flight delays and partnerships.
     - Expanded data relationships to capture and analyze trends in flight delays and customer satisfaction.

---

# Data Model

![MIST 4610 Group Project 2 Data Model](https://github.com/user-attachments/assets/2bf9302d-576d-4c39-9321-333db7cabb7b)

## Flight, Passenger, and Ticket Relationship: Many-to-Many (M : M) through Ticket (Non-Identifying)

The relationship between **Flight**, **Passenger**, and **Ticket** is many-to-many because each flight can have multiple passengers, and each passenger can have multiple tickets for different flights. The **Ticket** table serves as a bridge, connecting these two entities while also storing key information such as seat number, class, and ticket price.

In this model, **Ticket** has a composite primary key `ticketNum`, ensuring that each ticket is uniquely identified. Additionally, **Ticket** contains two foreign keys: `flightNum` (linking it to the **Flight** table) and `idPassenger` (linking it to the **Passenger** table). This setup keeps clear records of which passengers are on each flight, as well as their seat numbers, travel class, and ticket types.

The data type for `ticketNum` is `VARCHAR(10)` to accommodate various formats of ticket numbers, which can be a mix of alphanumeric characters. Prices are stored as `DECIMAL(6,2)` to handle currency with precision, ensuring accuracy for ticket prices.



## Airport and Flight Relationship: One-to-Many (1 : M)

The relationship between **Airport** and **Flight** is one-to-many, as each flight departs from or arrives at one airport, but each airport can manage multiple flights. The **Airport** table stores information about different airports, while **Flight** tracks the scheduled flights departing from or arriving at these airports.

The **Flight** table contains two foreign keys, `departAirport` and `destinAirport`, linking it to the **Airport** table. The primary key of **Airport** is `airportCode`, a three-character IATA code (e.g., ATL for Atlanta). This ensures that each airport is uniquely identified. The data type for `airportCode` is `CHAR(3)` to enforce a standardized length of three characters for all airport codes. 



## Flight and Employee Relationship: Many-to-Many (M : M) through AirCrew (Identifying)

The relationship between **Flight** and **Employee** is many-to-many, facilitated by the **AirCrew** table. Each flight can have multiple employees (e.g., pilots, flight attendants), and each employee can be assigned to multiple flights in a single day. The **AirCrew** table tracks which employees are working on each flight and their respective roles onboard.

**AirCrew** has a composite primary key consisting of `flightNum` and `idEmployee`, ensuring that each employee's assignment to a specific flight is unique. The foreign keys `flightNum` (linking to the **Flight** table) and `idEmployee` (linking to the **Employee** table) ensure that the correct employees are assigned to the right flights. The role of the employee onboard is represented by `roleOnboard`, which is now a `VARCHAR(45)` field, providing flexibility for more specific roles (e.g., "Captain," "First Officer," or "Cabin Crew").


## Airline and Employee Relationship: Many-to-Many (M : M) through EmployeeContract (Identifying)

The **Airline** and **Employee** entities are related through a many-to-many relationship, handled by the **EmployeeContract** table. An airline can employ many employees, and each employee may work for multiple airlines over their career, but only for one at a time. The **EmployeeContract** table records the employment history of employees, including when they joined and left an airline.

The composite primary key for **EmployeeContract** is made up of `airlineCode` and `idEmployee`, which ensures that each contract between an airline and an employee is unique. Foreign keys `airlineCode` and `idEmployee` link the **EmployeeContract** table to the **Airline** and **Employee** tables, respectively. The data types `joinedDate` and `resignedDate` are `DATE` fields to accurately record the employee’s start and end dates with the airline.



## Employee and PlaneModel Relationship: Many-to-Many (M : M) through Certificate (Identifying)

The **Employee** and **PlaneModel** entities have a many-to-many relationship, facilitated by the **Certificate** table. Employees, particularly pilots and crew, need to be certified to operate specific plane models. The **Certificate** table tracks which employees are certified to operate or serve on which plane models.

The composite primary key of **Certificate** (`modelName` and `idEmployee`) uniquely identifies each certification. Foreign keys `modelName` and `idEmployee` link to the **PlaneModel** and **Employee** tables, respectively. This setup ensures that the system can track which employees are certified for which plane models. The `certificationDate` field is of type `DATE` to store the precise date when an employee received their certification.



## PlaneModel and Plane Relationship: One-to-Many (1 : M)

Each **PlaneModel** (such as a Boeing 737 or Airbus A320) can have multiple planes, but each **Plane** is associated with only one plane model. This one-to-many relationship is essential for tracking the specific details of each individual plane and its associated model.

The **Plane** table contains the foreign key `modelName`, linking it to the **PlaneModel** table, which holds details about the model, such as capacity and maximum distance. The data types for these fields are set to `INT` as they represent numeric values for capacity and distance.



## Airline and Plane Relationship: One-to-Many (1 : M)

Each **Airline** can operate many planes, but each **Plane** belongs to one specific airline. The **Airline** table holds the details of each airline, and the **Plane** table links to it via the foreign key `airlineCode`.

The foreign key `airlineCode` is of type `CHAR(2)`, as airlines use standardized two-character IATA codes (e.g., DL for Delta) to identify themselves. This relationship allows us to track which airline operates which planes, ensuring clarity in airline ownership and plane usage.



## Flight and Plane Relationship: One-to-Many (1 : M)

A **Flight** can only be assigned one **Plane**, but each **Plane** can be used for multiple flights. This one-to-many relationship is crucial for tracking which plane is assigned to which flight. The **Flight** table contains the foreign key `planeRegistrationNum`, linking it to the **Plane** table, which holds the plane’s details.

The `planeRegistrationNum` is of type `VARCHAR(45)` to accommodate alphanumeric registration numbers, which can vary in length depending on the country and regulatory body. This ensures that the system can handle various registration formats while tracking plane assignments.



## Codeshare Flights: Boolean Representation in Flight Table

The **Flight** table includes a new `codeshare` field (TINYINT) to indicate whether a flight is operated as part of a codeshare agreement between airlines. A value of `1` represents a codeshare flight, while `0` indicates it is not. This addition provides critical insight into airline partnerships and shared international operations.





# Data Dictionary

## AirCrew

| Column Name  | Description                              | Data Type      | Size | Format       | Key? |
|--------------|------------------------------------------|----------------|------|--------------|------|
| flightNum    | Unique identifier for each flight        | VARCHAR        | 10   |              | PK   |
| idEmployee   | Unique identifier for the employee       | INT            |      |              | PK   |
| roleOnboard  | Role of the employee on the flight       | VARCHAR        | 45   |              |      |



## Airline

| Column Name   | Description                              | Data Type      | Size | Format       | Key? |
|---------------|------------------------------------------|----------------|------|--------------|------|
| airlineCode   | Unique identifier for each airline       | CHAR           | 2    |              | PK   |
| airlineName   | Name of the airline                     | VARCHAR        | 45   |              |      |
| originCountry | Country of the airline's origin          | VARCHAR        | 45   |              |      |



## Airport

| Column Name   | Description                              | Data Type      | Size | Format       | Key? |
|---------------|------------------------------------------|----------------|------|--------------|------|
| airportCode   | Unique identifier for each airport       | CHAR           | 3    |              | PK   |
| airportName   | Name of the airport                     | VARCHAR        | 100  |              |      |
| city          | City where the airport is located        | VARCHAR        | 45   |              |      |
| state         | State where the airport is located       | VARCHAR        | 45   |              |      |
| country       | Country where the airport is located     | VARCHAR        | 45   |              |      |



## Certificate

| Column Name       | Description                          | Data Type      | Size | Format       | Key? |
|-------------------|--------------------------------------|----------------|------|--------------|------|
| certificationDate | Date of certification               | DATE           |      |              | PK   |
| modelName         | Plane model for certification       | VARCHAR        | 45   |              | PK   |
| idEmployee        | Unique identifier for the employee   | INT            |      |              | PK   |



## Employee

| Column Name  | Description                              | Data Type      | Size | Format       | Key? |
|--------------|------------------------------------------|----------------|------|--------------|------|
| idEmployee   | Unique identifier for the employee       | INT            |      |              | PK   |
| empFName     | First name of the employee              | VARCHAR        | 45   |              |      |
| empLName     | Last name of the employee               | VARCHAR        | 45   |              |      |
| empdob       | Date of birth of the employee           | DATE           |      |              |      |
| position     | Employee's job position                 | ENUM           |      | Predefined   |      |
| flightHour   | Total flight hours of the employee      | VARCHAR        | 45   |              |      |
| email        | Email address of the employee           | VARCHAR        | 100  |              |      |
| phoneNum     | Phone number of the employee            | VARCHAR        | 15   |              |      |



## EmployeeContract

| Column Name  | Description                              | Data Type      | Size | Format       | Key? |
|--------------|------------------------------------------|----------------|------|--------------|------|
| airlineCode  | Unique identifier for the airline        | CHAR           | 2    |              | PK   |
| idEmployee   | Unique identifier for the employee       | INT            |      |              | PK   |
| joinedDate   | Start date of the employment contract    | DATE           |      |              | PK   |
| resignedDate | End date of the employment contract      | DATE           |      |              |      |



## Flight

| Column Name           | Description                          | Data Type      | Size | Format           | Key? |
|-----------------------|--------------------------------------|----------------|------|------------------|------|
| flightNum             | Unique identifier for the flight     | VARCHAR        | 10   |                  | PK   |
| depTime               | Departure time of the flight         | DATETIME       |      | YYYY-MM-DD HH:mm | PK   |
| arrTime               | Arrival time of the flight           | DATETIME       |      | YYYY-MM-DD HH:mm |      |
| gate                  | Gate number for the flight           | INT            |      |                  |      |
| terminal              | Terminal for the flight             | CHAR           | 1    |                  |      |
| status                | Status of the flight                | ENUM           |      | Predefined       |      |
| delayMinutes          | Delay duration in minutes           | INT            |      |                  |      |
| delayReason           | Reason for the delay                | VARCHAR        | 250  |                  |      |
| codeshare             | Codeshare status (1 = yes, 0 = no)  | TINYINT        | 1    | "1" or "0"       |      |
| destinAirport         | Destination airport code            | CHAR           | 3    |                  |      |
| departAirport         | Departure airport code              | CHAR           | 3    |                  |      |
| planeRegistrationNum  | Plane registration number           | VARCHAR        | 45   |                  |      |



## Passenger

| Column Name   | Description                              | Data Type      | Size | Format       | Key? |
|---------------|------------------------------------------|----------------|------|--------------|------|
| idPassenger   | Unique identifier for the passenger      | INT            |      |              | PK   |
| passFName     | First name of the passenger             | VARCHAR        | 45   |              |      |
| passLName     | Last name of the passenger              | VARCHAR        | 45   |              |      |
| passdob       | Date of birth of the passenger          | DATE           |      |              |      |
| citizenship   | Country of citizenship                  | VARCHAR        | 45   |              |      |
| priorityBoard | Priority boarding status (1 = yes, 0 = no)| TINYINT        | 1    | "1" or "0"   |      |
| email         | Email address of the passenger          | VARCHAR        | 100  |              |      |
| phoneNum      | Phone number of the passenger           | VARCHAR        | 15   |              |      |
| passportNum   | Passport number of the passenger        | VARCHAR        | 15   |              |      |



## Plane

| Column Name          | Description                          | Data Type      | Size | Format       | Key? |
|----------------------|--------------------------------------|----------------|------|--------------|------|
| planeRegistrationNum | Unique identifier for the plane      | VARCHAR        | 45   |              | PK   |
| planeAge             | Age of the plane                    | INT            |      |              |      |
| airlineCode          | Airline operating the plane         | CHAR           | 2    |              |      |
| modelName            | Model of the plane                  | VARCHAR        | 45   |              |      |



## PlaneModel

| Column Name  | Description                              | Data Type      | Size | Format       | Key? |
|--------------|------------------------------------------|----------------|------|--------------|------|
| modelName    | Unique identifier for the plane model    | VARCHAR        | 45   |              | PK   |
| manufacturer | Manufacturer of the plane model         | VARCHAR        | 45   |              |      |
| capacity     | Passenger capacity of the plane model   | INT            |      |              |      |
| maxDist      | Maximum distance of the plane model     | INT            |      |              |      |



## Ticket

| Column Name  | Description                              | Data Type      | Size | Format       | Key? |
|--------------|------------------------------------------|----------------|------|--------------|------|
| ticketNum    | Unique identifier for the ticket         | VARCHAR        | 45   |              | PK   |
| idPassenger  | Passenger linked to the ticket           | INT            |      |              |      |
| class        | Class of the ticket                     | ENUM           |      | Predefined   |      |
| seatNum      | Seat number assigned to the ticket      | VARCHAR        | 5    |              |      |
| ticketPrice  | Price of the ticket                     | DECIMAL        | 10,2 |              |      |
| flightNum    | Flight associated with the ticket       | VARCHAR        | 10   |              |      |
| depTime      | Departure time of the ticketed flight   | DATETIME       |      | YYYY-MM-DD HH:mm |  |




# Five Queries

| Features                  | Query 1 | Query 2 | Query 3 | Query 4 | Query 5 |
|---------------------------|---------|---------|---------|---------|---------|
| Multi-Table Join          |         |    x    |    x    |    x    |    x    |
| Subquery                  |    x    |         |         |         |         |
| Correlated Subquery       |    x    |         |         |         |         |
| GROUP BY                  |    x    |    x    |         |         |    x    |
| GROUP BY with HAVING      |         |         |         |         |         |
| Multi-Condition WHERE     |    x    |    x    |    x    |         |         |
| Built-in Functions        |    x    |    x    |         |         |         |
| Calculated Field          |    x    |    x    |         |         |         |


## 1. Find the top 6 most delayed routes (domestic and international) based on average delay minutes, and include the total number of delays and the percentage of flights delayed on each route.​

```ruby
SELECT 
    CASE
        WHEN
            f.departAirport IN (SELECT 
                    airportCode
                FROM
                    Airport
                WHERE
                    country = 'USA')
                AND f.destinAirport IN (SELECT 
                    airportCode
                FROM
                    Airport
                WHERE
                    country = 'USA')
        THEN
            'Domestic'
        ELSE 'International'
    END AS FlightType,
    f.departAirport AS Departure,
    f.destinAirport AS Destination,
    COUNT(*) AS TotalFlights,
    SUM(CASE
        WHEN f.delayMinutes > 0 THEN 1
        ELSE 0
    END) AS TotalDelays,
    AVG(CASE
        WHEN f.delayMinutes > 0 THEN f.delayMinutes
        ELSE NULL
    END) AS AvgDelayMinutes
FROM
    Flight f
GROUP BY FlightType , f.departAirport , f.destinAirport
ORDER BY FlightType , AvgDelayMinutes DESC
LIMIT 6; 
```

<img width="1269" alt="Screenshot 2024-12-03 at 6 11 35 PM" src="https://github.com/user-attachments/assets/1cbac4b1-f531-491f-8494-43508ce5b78d">


### Explanation
This query helps identify the 6 most delayed flight routes, splitting them into two types: Domestic (flights where both the departure and destination airports are in the USA) and International (flights where at least one airport is outside the USA). It calculates three key pieces of information for each route: the total number of flights, how many of those flights were delayed, and the average delay time (in minutes) for delayed flights. The query then sorts the routes by flight type and average delay time, showing the routes with the worst delays first. Finally, it limits the results to the top 6 routes with the highest average delay time.

### Managerial Relevance
This query helps managers focus on the most problematic flight routes with frequent and long delays. By knowing whether delays happen more on domestic or international routes, they can create specific plans to address the issues. This could include better scheduling, improving operations, or adding staff to routes with high delays. It also gives insights into customer pain points, helping improve services on these routes.


## 2. Calculate the average number of hours worked by Pilots who flight international flights.​

```ruby
SELECT 
    CASE
        WHEN
            f.departAirport IN (SELECT 
                    airportCode
                FROM
                    Airport
                WHERE
                    country = 'USA')
                AND f.destinAirport IN (SELECT 
                    airportCode
                FROM
                    Airport
                WHERE
                    country = 'USA')
        THEN
            'Domestic'
        ELSE 'International'
    END AS FlightType,
    AVG(e.flightHour) AS AvgHoursWorked
FROM
    Flight f
        JOIN
    AirCrew ac ON f.flightNum = ac.flightNum
        JOIN
    Employee e ON ac.idEmployee = e.idEmployee
WHERE
    e.position = 'Pilot'
GROUP BY FlightType;
```

<img width="1267" alt="Screenshot 2024-12-03 at 6 14 25 PM" src="https://github.com/user-attachments/assets/739ede27-36cd-4902-a8c3-f25bcca3e404">

### Explanation
This query calculates the average number of hours worked by pilots on domestic and international flights. It classifies each flight as either **Domestic** (both departure and destination airports are in the USA) or **International** (at least one airport is outside the USA). The query then focuses only on pilots (using `e.position = 'Pilot'`), calculates the average flight hours they have worked (`AVG(e.flightHour)`), and groups the results by flight type (Domestic or International).

### Managerial Relevance
This query helps managers understand how much time pilots spend flying on domestic versus international routes. This insight can assist in workload management, ensuring pilots aren't overworked on international flights, which can be longer and more demanding. It also provides data to optimize pilot scheduling, improve resource allocation, and ensure compliance with industry regulations about pilot working hours. This information is critical for maintaining safety, efficiency, and pilot satisfaction.


## 3. Delayed Flights and Their Causes

```ruby
SELECT 
    Airline.airlineName AS AirlineName,
    Flight.flightNum AS FlightNumber,
    Flight.departAirport AS DepartureAirport,
    Flight.destinAirport AS DestinationAirport,
    Flight.delayMinutes AS DelayDuration,
    Flight.delayReason AS DelayCause
FROM 
    Flight
JOIN 
    Plane ON Flight.planeRegistrationNum = Plane.planeRegistrationNum
JOIN 
    Airline ON Plane.airlineCode = Airline.airlineCode
WHERE 
    Flight.delayMinutes > 0
    AND Flight.delayReason IS NOT NULL
    AND Flight.status = 'Delayed'
ORDER BY 
    Flight.delayMinutes DESC;
```

<img width="1270" alt="Screenshot 2024-12-03 at 6 30 35 PM" src="https://github.com/user-attachments/assets/c8bbdc08-a193-4978-a9eb-650e906202b9">


### Explanation
This query retrieves detailed information about delayed flights, including the airline name, flight number, departure and destination airports, delay duration (in minutes), and the reason for the delay. It focuses only on flights that experienced a delay (where `Flight.delayMinutes > 0`) and ensures the delay reason is recorded (`Flight.delayReason IS NOT NULL`). The query also filters flights marked as "Delayed" (`Flight.status = 'Delayed'`) and orders the results by the longest delays first (`Flight.delayMinutes DESC`).


### Managerial Relevance
This query provides critical insights into delayed flights, helping managers identify which flights and airlines are experiencing the most significant delays. By analyzing the delay causes and durations, airlines can address operational inefficiencies, improve scheduling, or enhance resource allocation. Additionally, this data enables better communication with passengers about delay reasons and helps improve customer satisfaction by targeting problem areas for improvement. This query also supports strategic planning by highlighting patterns or trends in delays that need immediate attention.


## 4. Crew Members Assigned to Flights

```ruby
SELECT 
    Flight.flightNum AS FlightNumber,
    AirCrew.idEmployee AS CrewID,
    Employee.empFName AS FirstName,
    Employee.empLName AS LastName,
    AirCrew.roleOnBoard AS Role
FROM
    AirCrew
        JOIN
    Flight ON AirCrew.flightNum = Flight.flightNum
        JOIN
    Employee ON AirCrew.idEmployee = Employee.idEmployee
ORDER BY Flight.flightNum;
```

<img width="1270" alt="Screenshot 2024-12-03 at 6 29 44 PM" src="https://github.com/user-attachments/assets/def91ab2-8af9-4f2c-bb83-7a40c979f9fe">

### Explanation
This query provides a detailed view of the crew members assigned to flights. It retrieves the flight number, crew member's employee ID, first and last names, and their role on board the flight. By joining the `AirCrew`, `Flight`, and `Employee` tables, the query connects crew information to specific flights and organizes the results by flight number.

### Managerial Relevance
This query helps ensure proper staffing for flights by showing which employees are assigned to which roles on specific flights. It allows managers to verify crew assignments, avoid scheduling conflicts, and ensure compliance with staffing requirements. Additionally, it can be used to evaluate role distribution and identify gaps in crew coverage for specific flights.


## 5. Flight Routes by Airline

```ruby
SELECT 
    Airline.airlineName AS AirlineName,
    Airport1.city AS OriginCity,
    Airport2.city AS DestinationCity,
    COUNT(Flight.flightNum) AS TotalFlights
FROM
    Flight
        JOIN
    Airport AS Airport1 ON Flight.departAirport = Airport1.airportCode
        JOIN
    Airport AS Airport2 ON Flight.destinAirport = Airport2.airportCode
        JOIN
    Plane ON Flight.planeRegistrationNum = Plane.planeRegistrationNum
        JOIN
    Airline ON Plane.airlineCode = Airline.airlineCode
GROUP BY Airline.airlineName , Airport1.city , Airport2.city
ORDER BY TotalFlights DESC;
```

<img width="1270" alt="Screenshot 2024-12-03 at 6 29 12 PM" src="https://github.com/user-attachments/assets/0d9c322b-9590-4bf7-86fb-30584189fdd9">


### Explanation
This query lists all flight routes operated by each airline, including the origin and destination cities, and counts the total number of flights for each route. It joins the `Flight`, `Airport`, `Plane`, and `Airline` tables to link route and airline data, grouping the results by airline and route. The results are ordered by the number of flights in descending order.

### Managerial Relevance
This query provides insights into route popularity and airline performance. Managers can use this information to identify high-traffic routes and allocate resources more effectively. It also helps in strategic planning by highlighting which airlines dominate certain routes, assisting in competition analysis and route optimization. Furthermore, this data can inform decisions about adding, reducing, or altering flight frequencies for specific routes.


# Database information

The name of the database on the MySQL server: **cs_wl82230**

Each query listed above is marked in the database using stored procedures which can be called using the following format: ```CALL TP2_Qx();``` where ```x``` is the number of query.
