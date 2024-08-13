SDG Problem Definition Document
Title: SDG Problem Definition: Promoting Clean Energy Access
Selected SDG: SDG 7: Affordable and Clean Energy
Problem Statement:
Many communities, especially in developing regions, lack access to clean energy solutions, relying on traditional fossil fuels that are harmful to health and the environment. This project aims to analyze barriers to clean energy adoption and identify solutions to improve access to affordable clean energy resources.
Objectives:
To assess the availability and affordability of clean energy solutions (like solar, wind, etc.) in various regions.
To analyze household energy expenditures and identify trends based on income levels and demographics.
To evaluate the impact of clean energy adoption on community well-being and environmental sustainability.
2. Entity-Relationship Diagram (ERD)
Entities:
Households: Information about households (ID, address, income level, current energy source).
CleanEnergySolutions: Types of clean energy solutions available (ID, name, description, installation cost, maintenance cost).
EnergyProviders: Information about companies or organizations providing clean energy (ID, name, location, type).
EnergyUsage: Records of energy consumption per household and energy source (ID, household ID, energy solution ID, consumption, date).
Sample ERD
+------------------+        +--------------------+        +---------------------+        +------------------+
|     Households    |        | CleanEnergySolutions |        |   EnergyProviders   |        |     EnergyUsage   |
+------------------+        +--------------------+        +---------------------+        +------------------+
| HouseholdID (PK) |<----+  | SolutionID (PK)    |      | ProviderID (PK)    |        | UsageID (PK)     |
| Address          |        | Name               |        | Name                |        | HouseholdID (FK) |
| IncomeLevel      |        | Description        |        | Location            |        | SolutionID (FK)  |
| CurrentSourceID  |        | InstallationCost    |        | Type                |        | Consumption       |
+------------------+        | MaintenanceCost     |        +---------------------+        | Date              |
                            +--------------------+                                            +------------------+
3. SQL Scripts
Schema Creation:

CREATE TABLE Households (
    HouseholdID INT PRIMARY KEY,
    Address VARCHAR(255),
    IncomeLevel VARCHAR(50),
    CurrentSourceID INT,
    FOREIGN KEY (CurrentSourceID) REFERENCES CleanEnergySolutions(SolutionID)
);

CREATE TABLE CleanEnergySolutions (
    SolutionID INT PRIMARY KEY,
    Name VARCHAR(100),
    Description TEXT,
    InstallationCost DECIMAL(10, 2),
    MaintenanceCost DECIMAL(10, 2)
);

CREATE TABLE EnergyProviders (
    ProviderID INT PRIMARY KEY,
    Name VARCHAR(100),
    Location VARCHAR(255),
    Type VARCHAR(50)
);

CREATE TABLE EnergyUsage (
    UsageID INT PRIMARY KEY,
    HouseholdID INT,
    SolutionID INT,
    Consumption DECIMAL(10, 2),
    Date DATE,
    FOREIGN KEY (HouseholdID) REFERENCES Households(HouseholdID),
    FOREIGN KEY (SolutionID) REFERENCES CleanEnergySolutions(SolutionID)
);
Sample Data Insertion:
INSERT INTO CleanEnergySolutions (SolutionID, Name, Description, InstallationCost, MaintenanceCost) VALUES
(1, 'Solar Panels', 'Photovoltaic panels that convert sunlight into electricity.', 5000.00, 200.00),
(2, 'Wind Turbines', 'Turbines that convert wind energy into electricity.', 8000.00, 300.00);

INSERT INTO EnergyProviders (ProviderID, Name, Location, Type) VALUES
(1, 'Green Energy Corp.', 'Urban Area A', 'Non-Profit'),
(2, 'Renewable Solutions Inc.', 'Urban Area B', 'Private');

INSERT INTO Households (HouseholdID, Address, IncomeLevel, CurrentSourceID) VALUES
(1, '123 Maple St', 'Low', 1),
(2, '456 Pine St', 'Medium', NULL),  -- No clean energy yet
(3, '789 Cedar St', 'High', 2);

INSERT INTO EnergyUsage (UsageID, HouseholdID, SolutionID, Consumption, Date) VALUES
(1, 1, 1, 150.00, '2024-08-01'),
(2, 2, NULL, 200.00, '2024-08-01'),  -- Traditional energy usage
(3, 3, 2, 180.00, '2024-08-01');
Sample Queries:
-- Retrieve households and their current energy solutions
SELECT h.Address, ces.Name AS CurrentEnergySource
FROM Households h
LEFT JOIN CleanEnergySolutions ces ON h.CurrentSourceID = ces.SolutionID;

-- Count households using each clean energy solution
SELECT ces.Name, COUNT(h.HouseholdID) AS NumberOfHouseholds
FROM CleanEnergySolutions ces
JOIN Households h ON ces.SolutionID = h.CurrentSourceID
GROUP BY ces.Name;
4. Excel Workbook with Data Analysis and Dashboard
Data Import: Use Excel's "Get Data" feature to import relevant tables from your database.
Data Analysis:
Create Pivot Tables to summarize:
The number of households using clean energy solutions.
Average installation and maintenance costs by solution type.
Use Charts to visualize:
The distribution of households based on energy source.
Trends in energy consumption over time.
Dashboard:
Create an interactive dashboard displaying:
Key metrics (total households with clean energy, average costs).
Visual representations of energy source usage.
Insights into the relationship between income level and energy adoption.
5. Integration Documentation
Title: Integration Documentation for Excel Dashboard
Overview: Document the process of importing data from the database into Excel.
Steps:
Open Excel and go to the Data tab.
Click on “Get Data” and select “From Database”.
Choose your database connection and select relevant tables.
Load the data into Excel and create Pivot Tables.
Set up your dashboard layout and link visualizations to the Pivot Tables.
Testing: Outline how to test the dashboard for accuracy and data consistency.
