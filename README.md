# Team Name and Members
### Team name:

AKEA


### Members:

1. Andrew Hancock [@AndyH775](https://github.com/AndyH775)
2. Kaniyah McGee [@Kmcgee2026](https://github.com/Kmcgee2026)
3. Esther Liao [@esther-wenxi-liao](https://github.com/esther-wenxi-liao)
4. Alexis Williams ​[@AlexisWilliams26](https://github.com/AlexisWilliams26)

# Project Scenario

## Inspiration 

The idea for this project emerged from the complex nature of airline operations, where numerous components—such as flights, passengers, employees, and planes—must be managed efficiently to ensure smooth functioning. Through discussions with our group and instructor, we recognized that airlines face significant challenges in tracking flights, assigning crew, managing passenger ticketing, and ensuring staff certifications. Many current systems used by airlines often lack the flexibility and integration required to manage all these elements in a centralized way. This prompted us to create a comprehensive data model to unify these components in a logical and cohesive manner.

Our team aimed to design a solution that not only tracks day-to-day operations but also provides real-time data for better decision-making. After exploring various approaches, we determined that a robust relational database model would effectively address these challenges. By tracking the key relationships between flights, employees, planes, and passengers, the model enhances operational efficiency and data accuracy, enabling the airline to better manage its operations.

To simplify the scope of this project and avoid building an overly complex system, we focused on a specific date—October 3, 2024—capturing a sample of domestic flights in and out of Atlanta’s Hartsfield-Jackson Airport. This allows us to demonstrate how the system would work while keeping the dataset manageable, providing an accurate and practical simulation of airline operations.

## System Workflow and Functionality

The data model connects various entities involved in airline operations through well-defined relationships. **Flights** are the central focus, linking to other key components such as **Passengers**, **Planes**, **Employees**, and **Airports**. Each flight in the system has passengers who purchase tickets, and each flight is serviced by a crew that must be appropriately certified for the plane being used.

For example, a passenger purchasing a ticket is linked to a specific flight, and that flight has an assigned plane that is operated by an airline. Employees like pilots and flight attendants are assigned to flights, but they need the correct certifications for the plane model they are operating or servicing. The data model ensures that only qualified employees are scheduled for each flight, that the plane's capacity matches the number of tickets sold, and that the airline can keep track of employee contracts over time.

Each entity—whether it’s a flight, a plane, or an employee—interacts through foreign keys and relationships that enable the system to capture real-world airline operations. The **Ticket** table bridges passengers and flights, the **AirCrew** table links employees to flights, and the **Certificate** table tracks employee certifications for plane models. This structure ensures that data is not only accurate but also logically connected, reducing redundancies and potential for errors.


## Main Purpose

The primary purpose of this project is to simulate how Atlanta's Hartsfield-Jackson Airport can manage the operational complexities involved in tracking flights, passengers, planes, and employees through a well-designed database system. Specifically, the project focuses on a sampling of domestic flights arriving at and departing from Atlanta on October 3, 2024, along with associated airline and plane data. This targeted approach allows us to demonstrate how the system efficiently handles key operational components without overwhelming it with the full scope of daily airport activity.

The goal is to create a centralized data model that enhances both the accuracy and efficiency of managing flight operations. The model organizes flight information, ensures that employees are properly assigned to flights with the correct certifications, and tracks which planes are used for each flight. By limiting the dataset to a specific day and set of domestic flights, the project simplifies real-world complexities while maintaining realistic airline operations.

This system serves several key objectives:

- **Operational Efficiency:** The model ensures that flights are accurately tracked, planes are appropriately assigned, and employees are assigned based on their certifications. The focused scope helps streamline the handling of data for the selected flights, preventing conflicts and ensuring operational readiness for that particular day.
- **Data Integrity:** By organizing the relationships between flights, airlines, passengers, and planes, the model reduces the chance for data discrepancies. The system’s relational structure ensures that data flows consistently across the different entities.
- **Focused Simulation:** Instead of covering the entire airport’s operations, we focus on a manageable dataset—one day’s worth of domestic flights. This allows us to highlight the system's ability to handle key elements of airport management effectively, simulating real-world airline operations on a smaller, more comprehensible scale.
- **Decision-Making Support:** The system provides airport management with real-time information on flights, staffing, and plane assignments for October 3, 2024. This helps support timely decisions about flight scheduling and resource allocation during the day.


---

# Data Model

![Final Draft Model](https://github.com/user-attachments/assets/1c8f733e-3459-4f55-b031-0f3bc2d4d5d9)


## Flight, Passenger, and Ticket Relationship: Many-to-Many (M : M) through Ticket (Non-Identifying)

The relationship between **Flight**, **Passenger**, and **Ticket** is many-to-many because each flight can have multiple passengers, and each passenger can have multiple tickets for different flights. The **Ticket** table serves as a bridge, connecting these two entities while also storing key information such as seat number, class, and ticket price. 

In this model, **Ticket** has a composite primary key `ticketNum`, ensuring that each ticket is uniquely identified. Additionally, **Ticket** contains two foreign keys: `flightNum` (linking it to the **Flight** table) and `idPassenger` (linking it to the **Passenger** table). This setup keeps clear records of which passengers are on each flight, as well as their seat numbers and travel class.

The data type for `ticketNum` is `VARCHAR(10)` to accommodate various formats of ticket numbers, which can be a mix of alphanumeric characters. Prices are stored as `DECIMAL(6,2)` to handle currency with precision, ensuring accuracy for ticket prices.

---


## Airport and Flight Relationship: One-to-Many (1 : M)

The relationship between **Airport** and **Flight** is one-to-many, as each flight departs from or arrives at one airport, but each airport can manage multiple flights. The **Airport** table stores information about different airports, while **Flight** tracks the scheduled flights departing from or arriving at these airports.

The primary key of **Airport** is `airportCode`, a three-character IATA code (e.g., ATL for Atlanta). This ensures that each airport is uniquely identified. In the **Flight** table, `airportCode` is a foreign key that links each flight to its respective airport. The data type for `airportCode` is `CHAR(3)` to enforce a standardized length of three characters for all airport codes. 

---

## Flight and Employee Relationship: Many-to-Many (M : M) through AirCrew (Identifying)

The relationship between **Flight** and **Employee** is many-to-many, facilitated by the **AirCrew** table. Each flight can have multiple employees (e.g., pilots, flight attendants), and each employee can be assigned to multiple flights in a single day. The **AirCrew** table tracks which employees are working on each flight and their respective roles onboard.

**AirCrew** has a composite primary key consisting of `flightNum` and `idEmployee`, ensuring that each employee's assignment to a specific flight is unique. The foreign keys `flightNum` (linking to the **Flight** table) and `idEmployee` (linking to the **Employee** table) ensure that the correct employees are assigned to the right flights. The role of the employee onboard is represented by `roleOnboard`, an `ENUM` data type, as the roles are predefined (e.g., pilot, flight attendant).

---

## Airline and Employee Relationship: Many-to-Many (M : M) through EmployeeContract (Identifying)

The **Airline** and **Employee** entities are related through a many-to-many relationship, handled by the **EmployeeContract** table. An airline can employ many employees, and each employee may work for multiple airlines over their career, but only for one at a time. The **EmployeeContract** table records the employment history of employees, including when they joined and left an airline.

The composite primary key for **EmployeeContract** is made up of `airlineCode` and `idEmployee`, which ensures that each contract between an airline and an employee is unique. Foreign keys `airlineCode` and `idEmployee` link the **EmployeeContract** table to the **Airline** and **Employee** tables, respectively. The data types `joinedDate` and `resignedDate` are `DATE` fields to accurately record the employee’s start and end dates with the airline.

---

## Employee and PlaneModel Relationship: Many-to-Many (M : M) through Certificate (Identifying)

The **Employee** and **PlaneModel** entities have a many-to-many relationship, facilitated by the **Certificate** table. Employees, particularly pilots and crew, need to be certified to operate specific plane models. The **Certificate** table tracks which employees are certified to operate or serve on which plane models.

The composite primary key of **Certificate** (`idModelName` and `idEmployee`) uniquely identifies each certification. Foreign keys `idModelName` and `idEmployee` link to the **PlaneModel** and **Employee** tables, respectively. This setup ensures that the system can track which employees are certified for which plane models. The `certificationDate` field is of type `DATE` to store the precise date when an employee received their certification.

---

## PlaneModel and Plane Relationship: One-to-Many (1 : M)

Each **PlaneModel** (such as a Boeing 737 or Airbus A320) can have multiple planes, but each **Plane** is associated with only one plane model. This one-to-many relationship is essential for tracking the specific details of each individual plane and its associated model.

The **Plane** table contains the foreign key `modelName`, linking it to the **PlaneModel** table, which holds details about the model, such as capacity and maximum distance. The data types for these fields are set to `INT` as they represent numeric values for capacity and distance.

---

## Airline and Plane Relationship: One-to-Many (1 : M)

Each **Airline** can operate many planes, but each **Plane** belongs to one specific airline. The **Airline** table holds the details of each airline, and the **Plane** table links to it via the foreign key `airlineCode`.

The foreign key `airlineCode` is of type `CHAR(2)`, as airlines use standardized two-character IATA codes (e.g., DL for Delta) to identify themselves. This relationship allows us to track which airline operates which planes, ensuring clarity in airline ownership and plane usage.

---

## Flight and Plane Relationship: One-to-Many (1 : M)

A **Flight** can only be assigned one **Plane**, but each **Plane** can be used for multiple flights. This one-to-many relationship is crucial for tracking which plane is assigned to which flight. The **Flight** table contains the foreign key `planeRegistrationNum`, linking it to the **Plane** table, which holds the plane’s details.

The `planeRegistrationNum` is of type `VARCHAR(45)` to accommodate alphanumeric registration numbers, which can vary in length depending on the country and regulatory body. This ensures that the system can handle various registration formats while tracking plane assignments.



# Data Dictionary

<img width="1253" alt="Screenshot 2024-10-13 at 3 18 32 PM" src="https://github.com/user-attachments/assets/f71c3129-5040-48fb-8cbe-c728f3668291">

<img width="1400" alt="Screenshot 2024-10-13 at 2 59 34 PM" src="https://github.com/user-attachments/assets/867b5915-9178-43f1-bebb-750e38c898c3">

<img width="1400" alt="Screenshot 2024-10-13 at 3 00 08 PM" src="https://github.com/user-attachments/assets/b215952a-e9dd-43df-827a-bd7364997806">

<img width="1400" alt="Screenshot 2024-10-13 at 3 00 34 PM" src="https://github.com/user-attachments/assets/7af797bf-164d-4095-ad38-47bd9454fb13">

<img width="1400" alt="Screenshot 2024-10-13 at 2 52 53 PM" src="https://github.com/user-attachments/assets/ccceb881-c990-4f60-8cc0-e1ea314c208a">

<img width="1400" alt="Screenshot 2024-10-13 at 2 53 22 PM" src="https://github.com/user-attachments/assets/867c7ec1-eb22-41ab-a215-794c8f974b09">

<img width="1400" alt="Screenshot 2024-10-13 at 2 53 49 PM" src="https://github.com/user-attachments/assets/5cd23ae8-4ffd-47f0-a04a-9fc647e719c4">

<img width="1400" alt="Screenshot 2024-10-13 at 2 58 03 PM" src="https://github.com/user-attachments/assets/b92a7c2f-e11d-4457-bde4-0d8d8aeb76a0">

<img width="1400" alt="Screenshot 2024-10-13 at 2 56 55 PM" src="https://github.com/user-attachments/assets/3d985a74-25fb-4987-8251-0bd5e442cd17">

<img width="1400" alt="Screenshot 2024-10-13 at 2 57 16 PM" src="https://github.com/user-attachments/assets/e1cf702f-1544-4677-98fb-6e3127a835d5">

<img width="1400" alt="Screenshot 2024-10-13 at 2 57 37 PM" src="https://github.com/user-attachments/assets/b99a2eb9-177e-43da-b57c-e221a992fe91">


# Ten Queries

<img width="1400" alt="Screenshot 2024-10-13 at 5 40 39 PM" src="https://github.com/user-attachments/assets/2687ad6d-3257-4960-9053-36b59b659920">


## 1. Find the total number of flights for each airline with planes that are 10 years and older

```ruby
SELECT 
    airlineName, COUNT(flightNum) AS NumofFlights
FROM
    Airline,
    Plane,
    Flight
WHERE
    Airline.airlineCode = Plane.airlineCode
        AND Plane.planeRegistrationNum = Flight.planeRegistrationNum
        AND planeAge >= 10
GROUP BY airlineName; 
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 55 07 PM" src="https://github.com/user-attachments/assets/36add6da-835d-45c1-931f-aaaacb8442be">

### Explanation
This query counts the total number of flights operated by each airline using planes that are 10 years old or older. By joining the **Airline**, **Plane**, and **Flight** tables, the query filters planes based on their age and returns a count of the flights those planes were used for. This is particularly relevant for airlines that rely on an aging fleet.

### Managerial Relevance
Airline managers can use this information to assess the operational usage of older planes, which often require more frequent maintenance and could face regulatory restrictions. Identifying how many flights are conducted with older planes helps in planning fleet renewal strategies, ensuring safety compliance, and potentially optimizing maintenance schedules. Airlines could also use this information to determine if they need to invest in newer planes or reallocate their existing fleet for higher efficiency.


## 2. Grouped by airline, terminal, and calculate the occupancy rate and revenue for each flight​

```ruby
SELECT 
    Flight.flightNum,
    Flight.terminal,
    Airline.airlineName,
    Airline.airlineCode,
    COUNT(DISTINCT Ticket.idPassenger) / PlaneModel.capacity * 100 AS occupancyRate,
    SUM(Ticket.ticketPrice) AS totalRevenue
FROM
    Flight
        JOIN
    Ticket ON Flight.flightNum = Ticket.flightNum
        JOIN
    Plane ON Flight.planeRegistrationNum = Plane.planeRegistrationNum
        JOIN
    Airline ON Plane.airlineCode = Airline.airlineCode
        JOIN
    PlaneModel ON Plane.modelName = PlaneModel.modelName
GROUP BY Flight.flightNum , Flight.terminal , Airline.airlineName , Airline.airlineCode , PlaneModel.capacity;
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 56 06 PM" src="https://github.com/user-attachments/assets/5ac9c689-faa0-4a2f-8c5f-0f225db23fb7">

### Explanation
This query calculates two key performance indicators (KPIs) for each flight: the occupancy rate and total revenue. By grouping the results by airline and terminal, it provides insights into how full flights are (as a percentage of the total capacity of the plane) and how much revenue each flight generates. The query uses joins across the Flight, Ticket, Plane, Airline, and PlaneModel tables to gather the required data.

### Managerial Relevance
Managers can use this query to track the financial performance and efficiency of each flight. Understanding occupancy rates allows them to evaluate demand for specific flights, adjust pricing strategies, or optimize the scheduling of flights to better match capacity with demand. Additionally, monitoring revenue by flight and terminal helps in identifying high-performing routes and terminals, assisting in decisions regarding resource allocation, staffing, and future investments.

## 3. Retrieve all Pilot's names who are currently working for Delta Air Lines with at least 8,000 flight hours. List results in alphabetical order.​

```ruby
SELECT 
    Employee.empFName, Employee.empLName, Employee.flightHour
FROM
    Employee
        JOIN
    EmployeeContract ON EmployeeContract.idEmployee = Employee.idEmployee
        JOIN
    Airline ON EmployeeContract.airlineCode = Airline.airlineCode
WHERE
    Airline.airlineName = 'Delta Air lines'
        AND Employee.position = 'Pilot'
        AND Employee.flightHour >= 1000
ORDER BY Employee.empLName DESC;
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 56 50 PM" src="https://github.com/user-attachments/assets/c7a1ad82-e30d-4fb7-993b-371e586f7c6e">

### Explanation
This query retrieves the names of pilots employed by Delta Air Lines who have logged more than 8,000 flight hours. By joining the **Employee**, **EmployeeContract**, and **Airline** tables, the query filters pilots based on their position and their accumulated flight hours.

### Managerial Relevance
Managers at Delta Air Lines can use this query to track highly experienced pilots who have extensive flight hours. These pilots may be assigned to more complex or high-priority routes, as they bring significant experience to the table. Additionally, managers may use this data for performance reviews, promotions, or special assignments, ensuring that experienced pilots are being utilized effectively and recognized for their contributions.


## 4. Ensure that all crew members on each flight are certified for the plane model they are assigned to, ensuring compliance. 

```ruby
SELECT 
    Flight.flightNum,
    CASE
        WHEN
            COUNT(*) = COUNT(CASE
                WHEN Certificate.modelName = PlaneModel.modelName THEN 1
            END)
        THEN 1
        ELSE 0
    END AS allCertified
FROM
    Flight
        JOIN
    AirCrew ON Flight.flightNum = AirCrew.flightNum
        JOIN
    Employee ON AirCrew.idEmployee = Employee.idEmployee
        JOIN
    Certificate ON Employee.idEmployee = Certificate.idEmployee
        JOIN
    Plane ON Flight.planeRegistrationNum = Plane.planeRegistrationNum
        JOIN
    PlaneModel ON Plane.modelName = PlaneModel.modelName
GROUP BY Flight.flightNum;
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 57 42 PM" src="https://github.com/user-attachments/assets/23bb8742-ef73-4f0c-91d6-c3f6bea10ebd">

### Explanation
This query checks whether all crew members assigned to a flight are certified for the specific plane model they are working on. The query joins the **Flight**, **AirCrew**, **Employee**, **Certificate**, **Plane**, and **PlaneModel** tables and compares the certifications held by crew members with the model of the plane they are assigned to.

### Managerial Relevance
This is a critical query for ensuring compliance with safety regulations. Airline managers must ensure that all crew members are qualified to work on the plane models they are assigned to. Failing to comply with certification requirements can lead to safety violations, legal issues, and risks to passenger safety. By running this query, managers can proactively verify certifications and avoid any potential compliance issues before flights take off.


## 5. Identify the Top 5 Most Frequent Flyers Based on Ticket Purchases and Spending

```ruby
SELECT 
    Passenger.idPassenger,
    Passenger.passFName,
    Passenger.passLName,
    COUNT(Ticket.ticketNum) AS totalTickets,
    SUM(Ticket.ticketPrice) AS totalSpending
FROM
    Passenger
        JOIN
    Ticket ON Passenger.idPassenger = Ticket.idPassenger
GROUP BY Passenger.idPassenger
ORDER BY totalSpending DESC , totalTickets DESC
LIMIT 5;
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 59 05 PM" src="https://github.com/user-attachments/assets/f39a1b99-ecf9-4a1f-bfed-8772c8e05796">

### Explanation
This query identifies the top 5 passengers based on their total number of ticket purchases and overall spending. By joining the **Passenger** and **Ticket** tables, the query groups passengers by their ID and sums the number of tickets and total ticket prices, ranking them by total spending first and number of tickets second.

### Managerial Relevance
Understanding the behavior of top customers is vital for customer retention strategies. Frequent and high-spending passengers represent a key segment of an airline’s customer base. Managers can use this information to offer loyalty rewards, personalized services, or exclusive promotions to these top customers, encouraging repeat business and improving customer satisfaction. In addition, identifying high-value customers allows airlines to strengthen relationships with their most profitable clients.


## 6. Find Employees Who Worked More Than 5 Hours Across Multiple Flights on October 3, 2024

```ruby
SELECT 
    Employee.idEmployee,
    Employee.empFName,
    Employee.empLName,
    SUM(TIMESTAMPDIFF(HOUR,
        Flight.depTime,
        Flight.arrTime)) AS totalHoursWorked
FROM
    Employee
        JOIN
    AirCrew ON Employee.idEmployee = AirCrew.idEmployee
        JOIN
    Flight ON AirCrew.flightNum = Flight.flightNum
GROUP BY Employee.idEmployee
HAVING totalHoursWorked > 5;
```

<img width="1270" alt="Screenshot 2024-10-13 at 4 59 33 PM" src="https://github.com/user-attachments/assets/e623e21d-365c-4806-8c80-9525535f9492">

### Explanation
This query calculates the total hours worked by employees across multiple flights on October 3, 2024. By joining the **Employee**, **AirCrew**, and **Flight** tables, the query can calculate the duration of each flight and sums the hours worked by each employee.

### Managerial Relevance
Monitoring the total hours worked by employees across multiple flights is important for workforce management and compliance with labor regulations. Overworking employees can lead to burnout, reduced performance, and safety risks. Managers can use this query to ensure that employees are not exceeding reasonable working hours and to make adjustments to staffing schedules if necessary. This helps maintain both employee well-being and operational efficiency. 


## 7. List employees certified for multiple plane models who have worked on more than 1 flights on October 3, 2024

```ruby
SELECT 
    Employee.idEmployee,
    Employee.empFName,
    Employee.empLName,
    COUNT(DISTINCT Certificate.ModelName) AS totalModels,
    COUNT(DISTINCT Flight.flightNum) AS totalFlights
FROM
    Employee
        JOIN
    Certificate ON Employee.idEmployee = Certificate.idEmployee
        JOIN
    AirCrew ON Employee.idEmployee = AirCrew.idEmployee
        JOIN
    Flight ON AirCrew.flightNum = Flight.flightNum
GROUP BY Employee.idEmployee
HAVING COUNT(DISTINCT Certificate.ModelName) > 1
    AND COUNT(DISTINCT Flight.flightNum) > 1;
```

<img width="1270" alt="Screenshot 2024-10-13 at 5 00 50 PM" src="https://github.com/user-attachments/assets/90bc7baf-1020-4166-a999-10cd4e1dcad2">

### Explanation
This query identifies employees who are certified for multiple plane models and have worked on more than one flight in a single day. By joining the **Employee**, **Certificate**, **AirCrew**, and **Flight** tables, the query counts the number of distinct plane models and flights each employee is associated with and filters based on those who meet the criteria.

### Managerial Relevance
Employees certified for multiple plane models provide airlines with more flexibility in staffing and scheduling. These versatile employees can be assigned to a wider range of flights, reducing staffing constraints and improving operational efficiency. Managers can use this query to identify these employees and ensure they are optimally utilized in day-to-day operations. Additionally, this data can help to avoid aire creews overwork.


## 8. List airlines with planes averaging less than 50% occupancy

```ruby
SELECT 
    Airline.airlineName, Airline.airlineCode,
    AVG((flightPassengerCount.totalPassengers / PlaneModel.capacity) * 100) AS avgOccupancy
FROM
    Airline
        JOIN
    Plane ON Airline.airlineCode = Plane.airlineCode
        JOIN
    Flight ON Plane.planeRegistrationNum = Flight.planeRegistrationNum
        JOIN
    PlaneModel ON Plane.modelName = PlaneModel.modelName
        JOIN
    (SELECT 
        Ticket.flightNum,
        COUNT(Ticket.idPassenger) AS totalPassengers
    FROM
        Ticket
    GROUP BY Ticket.flightNum) AS flightPassengerCount ON Flight.flightNum = flightPassengerCount.flightNum
GROUP BY Airline.airlineName, Airline.airlineCode
HAVING avgOccupancy < 50;
```

<img width="1270" alt="Screenshot 2024-10-13 at 5 01 20 PM" src="https://github.com/user-attachments/assets/d5d8bf20-3d36-4f1d-9de9-b3e76344d9a2">

### Explanation
This query calculates the average occupancy rate for planes operated by each airline and identifies airlines whose planes have less than 50% occupancy. By joining the **Airline**, **Plane**, **Flight**, **Ticket**, and **PlaneModel** tables, the query calculates occupancy based on the number of passengers divided by the plane’s capacity.

### Managerial Relevance
Low occupancy rates can signal inefficiencies in flight scheduling or demand forecasting. Flights with less than 50% occupancy may indicate a need to reevaluate the flight schedule, reduce the frequency of certain routes, or invest in targeted marketing efforts to increase ticket sales. Managers can use this query to optimize route planning, improve profitability, and reduce unnecessary operating costs by ensuring flights are filled to a more profitable capacity.


## 9. Calculate the average ticket price by class for each flight and flag outliers

```ruby
WITH AvgPrice AS (
    SELECT Ticket.flightNum, 
           Ticket.class, 
           AVG(Ticket.ticketPrice) AS avgPrice
    FROM Ticket
    GROUP BY Ticket.flightNum, Ticket.class
)
SELECT Ticket.flightNum, 
       Ticket.class, 
       Ticket.ticketNum, 
       Ticket.ticketPrice, 
       AvgPrice.avgPrice, 
       (Ticket.ticketPrice - AvgPrice.avgPrice) / AvgPrice.avgPrice * 100 AS deviationPercentage
FROM Ticket
JOIN AvgPrice ON Ticket.flightNum = AvgPrice.flightNum AND Ticket.class = AvgPrice.class
WHERE (Ticket.ticketPrice - AvgPrice.avgPrice) / AvgPrice.avgPrice * 100 > 25;
```

<img width="1270" alt="Screenshot 2024-10-13 at 5 01 52 PM" src="https://github.com/user-attachments/assets/de05b1fa-74fb-48bd-963d-c210df0d4070">

### Explanation
This query calculates the average ticket price for each class (e.g., economy, business) on each flight and flags any ticket prices that deviate significantly (more than 25%) from the average. The query first calculates the average price for each flight and class combination, then joins this result with individual ticket prices to compute the percentage deviation.

### Managerial Relevance
Detecting pricing outliers is important for maintaining consistency in ticket pricing strategies. Large deviations from the average ticket price could indicate pricing errors, issues with dynamic pricing algorithms, or unusual booking patterns. Managers can use this query to investigate and address any discrepancies, ensuring that prices remain competitive while avoiding potential revenue losses from underpriced tickets or customer dissatisfaction from overpriced tickets.


## 10. Identify planes that haven’t been used for any flight on October 3, 2024

```ruby
SELECT 
    Plane.planeRegistrationNum, Plane.modelName
FROM
    Plane
WHERE
    NOT EXISTS( SELECT 
            1
        FROM
            Flight
        WHERE
            Plane.planeRegistrationNum = Flight.planeRegistrationNum);
```

<img width="1270" alt="Screenshot 2024-10-13 at 5 02 24 PM" src="https://github.com/user-attachments/assets/282daa8f-26f7-4090-b352-860ef1fb8c03">

### Explanation
This query identifies plane models that were not used for any flights on October 3, 2024. By using NOT EXISTS, the query compares the **Plane** and **Flight** tables and returns plane models that are not associated with any flights on the specified date.

### Managerial Relevance
Identifying underutilized planes is crucial for fleet management and resource optimization. Managers can use this query to track planes that are sitting idle, helping them make decisions about better plane allocation, whether to retire underused planes, or how to adjust flight schedules to maximize the use of available resources. This query ensures that airlines are getting the most out of their fleet, reducing unnecessary costs from keeping planes idle. 


# Database information

The name of the database on the MySQL server: **cs_wl82230**

Each query listed above is marked in the database using stored procedures which can be called using the following format: ```CALL TP_Qx();``` where ```x``` is the number of query.
