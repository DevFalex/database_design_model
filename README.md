# database_design_model
DESIGN OF A DATABASE MODEL FOR AIRLINE TICKET BOOKING SYSTEM

The Airline Ticket Booking System facilitates the seamless booking and management of airline tickets for passengers. The system ensures efficient flight scheduling, passenger booking, ticket issuance, and real-time flight management while maintaining a smooth customer experience.

# *Key Stakeholders:*
•	Passengers: These are individuals who book and travel on flights.
•	Airline Staff: The pilots, cabin crew, ground staff, and customer service representatives.
•	Aircraft Management Team: Oversees fleet maintenance and scheduling.
•	Ticketing and Booking System: Manages reservations, payments, and ticket issuance.
•	Airport Authorities: Handle check-in, security, and boarding processes.

# Main Activities and Workflows:
1. Flight Scheduling and Planning
•	Airlines create flight schedules based on demand, routes, and aircraft availability.
•	Flight details include departure/arrival times, flight numbers, aircraft type, and assigned crew.
2. Passenger Booking and Ticketing
•	Passengers book tickets through websites, mobile apps, or travel agencies.
•	Secure payment processing ensures booking confirmation.
•	A ticket with a unique ID and flight details is issued.
3. Check-in and Boarding
•	Passengers check in online or at airport kiosks.
•	Boarding passes are issued, and seat numbers are assigned.
•	Identity verification and baggage handling are completed.
4. Flight Operations
•	Crew members are assigned to flights.
•	Pre-flight safety checks and refueling occur.
•	Passengers board, and the flight takes off as scheduled.
5. Flight Monitoring and Updates
•	Real-time flight tracking by airline control.
•	Notifications on delays or schedule changes sent to passengers.
6. Post-Flight Operations
•	Arrival and passenger disembarkation.
•	Baggage claim and customs processing.
•	Aircraft maintenance and preparation for the next flight.

# 2.Conceptual Model (ER Model)
 
Entities and Attributes:
•	Passenger (PassengerID, Name, Email, Phone, Address)
•	Airline (AirlineID, Name, Country)
•	Aircraft (AircraftID, Model, Capacity, AirlineID)
•	Flight (FlightID, AirlineID, Departure, Destination, Date, Time, Status, AircraftID) 
•	Ticket (TicketID, PassengerID, FlightID, SeatNo, Price) 
•	Crew (CrewID, Name, Role, AircraftID) 
Relationships
•	One passenger can have multiple tickets, but each ticket is linked to only one passenger.
•	One airline operates many aircraft, but each aircraft belongs to one airline. 
•	One airline operates many flights, but each flight is operated by one airline. 
•	One aircraft can serve multiple flights over time, but each flight uses one aircraft.
•	One flight can have many tickets booked, but each ticket is for one specific flight.
•	One aircraft has multiple crew members assigned, but each crew member works on one aircraft per entry.

# 3. Logical Model
   
The logical model involves structuring these entities into tables, specifying attributes, primary keys (PK), and foreign keys (FK).

Table Definitions:

Passengers Table
•	PassengerID (PK)
•	Name
•	Email
•	Phone Number
•	Address

Flights Table
•	FlightID (PK)
•	AirlineID (FK)
•	Destination
•	Departure
•	Time
•	Date
•	Status
•	AircraftID (FK)

Tickets Table
•	TicketID (PK)
•	PassengerID (FK)
•	FlightID (FK)
•	Price
•	SeatNo

Aircraft Table
•	AircraftID (PK)
•	Model
•	Capacity
•	AirlineID (FK)

Crew Table
•	CrewID (PK)
•	Name
•	Role
•	AircraftID (FK)

Airline Table
•	AirlineID (PK)
•	Name
•	Country

# 4-5. Physical Model:
The physical model specifies how the data is stored in the database, including data types and storage mechanisms. This will also be used as the DDL (Data Definition Language) to the create tables in the database.
-- Creating Passenger Table
CREATE TABLE Passenger (
    PassengerID INT (10) PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Phone VARCHAR(20) NOT NULL,
    Address TEXT
);

-- Creating Airline Table
CREATE TABLE Airline (
    AirlineID INT (10) PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Country VARCHAR(50) NOT NULL
);

-- Creating Aircraft Table
CREATE TABLE Aircraft (
    AircraftID INT (10) PRIMARY KEY AUTO_INCREMENT,
    Model VARCHAR (100) NOT NULL,
    Capacity INT (10) NOT NULL,
    AirlineID INT (10),
    FOREIGN KEY (AirlineID) REFERENCES Airline(AirlineID) ON DELETE SET NULL
);

-- Creating Flight Table
CREATE TABLE Flight (
    FlightID INT (10) PRIMARY KEY AUTO_INCREMENT,
    AirlineID INT (10),
    Departure VARCHAR(100) NOT NULL,
    Destination VARCHAR(100) NOT NULL,
    Date DATE NOT NULL,
    Time TIME NOT NULL,
    Status VARCHAR(50) DEFAULT 'Scheduled',
    AircraftID INT (10),
    FOREIGN KEY (AircraftID) REFERENCES Aircraft(AircraftID) ON DELETE SET NULL,
    FOREIGN KEY (AirlineID) REFERENCES Airline(AirlineID) ON DELETE CASCADE
);

-- Creating Ticket Table
CREATE TABLE Ticket (
    TicketID INT (10) PRIMARY KEY AUTO_INCREMENT,
    PassengerID INT (10),
    FlightID INT (10),
    SeatNo VARCHAR(10) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (PassengerID) REFERENCES Passenger(PassengerID) ON DELETE CASCADE,
    FOREIGN KEY (FlightID) REFERENCES Flight(FlightID) ON DELETE CASCADE
);

-- Creating Crew Table
CREATE TABLE Crew (
    CrewID INT (10) PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Role VARCHAR(50) NOT NULL,
    FlightID INT (10),
    FOREIGN KEY (FlightID) REFERENCES Flight(FlightID) ON DELETE CASCADE
);

# 6. The Data Manipulation Language (DML): This showcases the way data is being inserted into the tables.
   
-- Insert into Airline
INSERT INTO Airline (Name, Country) VALUES ('Air Nigeria', 'Nigeria'), ('Ethiopian Airlines', 'Ethiopia');
-- Insert into Aircraft
INSERT INTO Aircraft (Model, Capacity, AirlineID) VALUES ('Boeing 737', 180, 1), ('Airbus A320', 160, 2);
-- Insert into Passenger
INSERT INTO Passenger (Name, Email, Phone, Address) VALUES (‘Faleye Opeoluwa’, ‘faleyeopeoluwa7@gmail.com’, ' 08105386988', 'Lagos, Nigeria'), (‘Aminat Alli’, ‘aminatalli@gmail.com’, '07015382288', 'Kaduna, Nigeria');
-- Insert into Flight
INSERT INTO Flight (AirlineID, Departure, Destination, Date, Time, Status, AircraftID) VALUES
(1, 'Lagos', 'Abuja', '2025-04-01', '10:00:00', 'Scheduled', 1),
(2, 'Addis Ababa', 'Lagos', '2025-04-02', '14:30:00', 'Scheduled', 2);
-- Insert into Ticket
INSERT INTO Ticket (PassengerID, FlightID, SeatNo, Price) VALUES (1, 1, '12A', 50000.00);
-- Insert into Crew
INSERT INTO Crew (Name, Role, FlightID) VALUES ('James Smith', 'Pilot', 1), ('Sarah Johnson', 'Flight Attendant', 2);


# 7. Querying the Database and showing its outputs:
Query 1: Query the DB to get the Passengers Flight Details
SELECT p.Name,f.Departure, f.Destination, f.Time, f.Date, t.SeatNo
FROM Passenger p
JOIN Ticket t ON p.PassengerID = t.PassengerID
JOIN Flight f ON t.FlightID = f.FlightID;
 

Query 2: Query the DB to get the aircraft details with airline information
SELECT A.Model, A.Capacity, L.Name AS Airline 
FROM Aircraft A
JOIN Airline L ON A.AirlineID = L.AirlineID;
 


