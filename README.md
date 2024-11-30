ER Model - https://app.diagrams.net/#HMbuzinous%2F2.SemesterObligatoriskOpgave%2Fmain%2F2.SemesterObligatoriskOpgave.drawio#%7B%22pageId%22%3A%22R2lEEEUBdFMjLlhIrx00%22%7D

RDS Diagram - https://docs.google.com/spreadsheets/d/1tiCFCr-6zcx7nHaq9VKTSr5Nhw2XW5zyqJkIy6s1Kgc/edit?usp=sharing

---------- MSSQL SCRIPT ----------

---- 1. Opret database
CREATE DATABASE JumpForFun;
GO
Use JumpForFun;
GO

---- 2. Opret skema
---- (IKKE NØDVENDIGT)

---- 3. Opret tabeller
CREATE TABLE Members(
MemberId INT IDENTITY(100000,1),
FirstName NVARCHAR(50),
LastName NVARCHAR(50),
MemberPhoneNumber INT,
[Password] NVARCHAR(100), --Jeg bruger parenteser for at sikre, at Password behandles som en identifikator (som et kolonnenavn) i stedet for et indbygget reserveret nøgleord. Sikre jeg ikke får syntax fejl.
CreatedDate DATETIME,
Email NVARCHAR(50),
MaxClassesPerMonth INT,
Birthdate DateTime,
AddressId INT,
SubscriptionId INT,
FitnessCenterId INT
);

CREATE TABLE Addresses(
AddressId INT IDENTITY(1,1),
StreetName NVARCHAR(100),
HouseNumber NVARCHAR(50),
PostalCode INT,
City NVARCHAR(50)
);

CREATE TABLE Subscriptions(
SubscriptionId INT IDENTITY(1,1),
SubscriptionName NVARCHAR(50),
Benefit NVARCHAR(500)
);

CREATE TABLE FitnessClassBookings(
FitnessClassBookingiD INT IDENTITY(1,1),
BookingDate DATETIME,
FitnessClassId INT,
MemberId INT,
);

CREATE TABLE FitnessClasses(
FitnessClassId INT IDENTITY(1,1),
ClassName NVARCHAR(50),
StartTime DATETIME,
EndTime DATETIME,
Duration INT,
CurrentParticipants INT,
MaxParticipants INT,
WeekdayId INT,
ClassTypeId INT,
TrainerId INT,
RoomId INT,
FitnessCenterId INT
);

CREATE TABLE Weekdays(
WeekdayId INT IDENTITY(1,1),
WeekdayName NVARCHAR(50)
);

CREATE TABLE ClassTypes(
ClassTypeId INT IDENTITY(1,1),
[Description] NVARCHAR(500) --Jeg bruger parenteser for at sikre, at Description behandles som en identifikator (som et kolonnenavn) i stedet for et indbygget reserveret nøgleord. Sikre jeg ikke får syntax fejl.
);

CREATE TABLE Rooms(
RoomId INT IDENTITY(1,1),
RoomName NVARCHAR(50)
);

CREATE TABLE Trainers(
TrainerId INT IDENTITY(1,1),
FirstName NVARCHAR(50),
LastName NVARCHAR(50),
TrainerPhoneNumber INT,
CreatedDate DATETIME,
Email NVARCHAR(50),
[Password] NVARCHAR(100), --Jeg bruger parenteser for at sikre, at Password behandles som en identifikator (som et kolonnenavn) i stedet for et indbygget reserveret nøgleord. Sikre jeg ikke får syntax fejl.
FitnessCenterId INT
);

CREATE TABLE FitnessCenters(
FitnessCenterId INT IDENTITY(1,1),
[Location] NVARCHAR(50) --Jeg bruger parenteser for at sikre, at Location behandles som en identifikator (som et kolonnenavn) i stedet for et indbygget reserveret nøgleord. Sikre jeg ikke får syntax fejl.
);

GO

---- 4. Opret fremmednøgler
---- Primary Keys
ALTER TABLE Members
ADD CONSTRAINT PK_Member PRIMARY KEY (MemberId); 

ALTER TABLE Addresses
ADD CONSTRAINT PK_Address PRIMARY KEY (AddressId); 

ALTER TABLE Subscriptions
ADD CONSTRAINT PK_Subscription PRIMARY KEY (SubscriptionId); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT PK_FitnessClass PRIMARY KEY (FitnessClassId); 

ALTER TABLE Weekdays
ADD CONSTRAINT PK_Weekday PRIMARY KEY (WeekdayId); 

ALTER TABLE ClassTypes
ADD CONSTRAINT PK_ClassType PRIMARY KEY (ClassTypeId); 

ALTER TABLE Rooms
ADD CONSTRAINT PK_Room PRIMARY KEY (RoomId); 

ALTER TABLE Trainers
ADD CONSTRAINT PK_Trainer PRIMARY KEY (TrainerId); 

ALTER TABLE FitnessCenters
ADD CONSTRAINT PK_FitnessCenter PRIMARY KEY (FitnessCenterId); 
GO

---- Foreign Keys
--Members FK
ALTER TABLE Members
ADD CONSTRAINT FK_MemberAddress
FOREIGN KEY (AddressId) REFERENCES Addresses(AddressId); 

ALTER TABLE Members
ADD CONSTRAINT FK_MemberSubscription
FOREIGN KEY (SubscriptionId) REFERENCES Subscriptions(SubscriptionId); 

ALTER TABLE Members
ADD CONSTRAINT FK_MemberFitnessCenter
FOREIGN KEY (FitnessCenterId) REFERENCES FitnessCenters(FitnessCenterId); 

--FitnessClassBookings FK
ALTER TABLE FitnessClassBookings
ADD CONSTRAINT FK_FitnessClassBookingFitnessClass
FOREIGN KEY (FitnessClassId) REFERENCES FitnessClasses(FitnessClassId); 

ALTER TABLE FitnessClassBookings
ADD CONSTRAINT FK_FitnessClassBookingMember
FOREIGN KEY (MemberId) REFERENCES Members(MemberId); 

--FitnessClasses FK
ALTER TABLE FitnessClasses
ADD CONSTRAINT FK_FitnessClassWeekday
FOREIGN KEY (WeekdayId) REFERENCES Weekdays(WeekdayId); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT FK_FitnessClassClassType
FOREIGN KEY (ClassTypeId) REFERENCES ClassTypes(ClassTypeId); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT FK_FitnessClassTrainer
FOREIGN KEY (TrainerId) REFERENCES Trainers(TrainerId); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT FK_FitnessClassRoom
FOREIGN KEY (RoomId) REFERENCES Rooms(RoomId); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT FK_FitnessClassFitnessCenter
FOREIGN KEY (FitnessCenterId) REFERENCES FitnessCenters(FitnessCenterId); 

--Trainers FK
ALTER TABLE Trainers
ADD CONSTRAINT FK_TrainerFitnessCenter
FOREIGN KEY (FitnessCenterId) REFERENCES FitnessCenters(FitnessCenterId); 
GO


---- 5. Definer andre constraints (NOT NULL, NULL, UNIQUE, CHECK, DEFAULT)
---- NOT NULL constraints
-- Members table NOT NULL
ALTER TABLE Members ALTER COLUMN FirstName NVARCHAR(50) NOT NULL;
ALTER TABLE Members ALTER COLUMN LastName NVARCHAR(50) NOT NULL;
ALTER TABLE Members ALTER COLUMN [Password] NVARCHAR(100) NOT NULL;
ALTER TABLE Members ALTER COLUMN MemberPhoneNumber INT NOT NULL;
ALTER TABLE Members ALTER COLUMN CreatedDate DATETIME NOT NULL;
ALTER TABLE Members ALTER COLUMN Email NVARCHAR(50) NOT NULL;
ALTER TABLE Members ALTER COLUMN MaxClassesPerMonth INT NOT NULL;
ALTER TABLE Members ALTER COLUMN Birthdate DATETIME NOT NULL;
ALTER TABLE Members ALTER COLUMN AddressId INT NOT NULL;
ALTER TABLE Members ALTER COLUMN SubscriptionId INT NOT NULL;
ALTER TABLE Members ALTER COLUMN FitnessCenterId INT NOT NULL;

-- Addresses table NOT NULL
ALTER TABLE Addresses ALTER COLUMN StreetName NVARCHAR(100) NOT NULL;
ALTER TABLE Addresses ALTER COLUMN HouseNumber NVARCHAR(50) NOT NULL;
ALTER TABLE Addresses ALTER COLUMN PostalCode INT NOT NULL;
ALTER TABLE Addresses ALTER COLUMN City NVARCHAR(50) NOT NULL;

-- Subscriptions table NOT NULL
ALTER TABLE Subscriptions ALTER COLUMN SubscriptionName NVARCHAR(50) NOT NULL;

-- FitnessClassBookings table NOT NULL
ALTER TABLE FitnessClassBookings ALTER COLUMN BookingDate DATETIME NOT NULL;
ALTER TABLE FitnessClassBookings ALTER COLUMN FitnessClassId INT NOT NULL;
ALTER TABLE FitnessClassBookings ALTER COLUMN MemberId INT NOT NULL;

-- FitnessClasses table NOT NULL
ALTER TABLE FitnessClasses ALTER COLUMN ClassName NVARCHAR(50) NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN StartTime DATETIME NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN Duration INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN MaxParticipants INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN WeekdayId INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN ClassTypeId INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN TrainerId INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN RoomId INT NOT NULL;
ALTER TABLE FitnessClasses ALTER COLUMN FitnessCenterId INT NOT NULL;

-- Weekdays table NOT NULL
ALTER TABLE Weekdays ALTER COLUMN WeekdayName NVARCHAR(50) NOT NULL;

-- ClassTypes table NOT NULL
ALTER TABLE ClassTypes ALTER COLUMN [Description] NVARCHAR(500) NOT NULL;

-- Rooms table NOT NULL
ALTER TABLE Rooms ALTER COLUMN RoomName NVARCHAR(50) NOT NULL;

-- Trainers table NOT NULL
ALTER TABLE Trainers ALTER COLUMN FirstName NVARCHAR(50) NOT NULL;
ALTER TABLE Trainers ALTER COLUMN LastName NVARCHAR(50) NOT NULL;
ALTER TABLE Trainers ALTER COLUMN TrainerPhoneNumber INT NOT NULL;
ALTER TABLE Trainers ALTER COLUMN CreatedDate DATETIME NOT NULL;
ALTER TABLE Trainers ALTER COLUMN Email NVARCHAR(50) NOT NULL;
ALTER TABLE Trainers ALTER COLUMN [Password] NVARCHAR(100) NOT NULL;
ALTER TABLE Trainers ALTER COLUMN FitnessCenterId INT NOT NULL;

-- FitnessCenters table NOT NULL
ALTER TABLE FitnessCenters ALTER COLUMN [Location] NVARCHAR(50) NOT NULL;
GO
---- NULL Constrains
-- FitnessClasses table NULL
ALTER TABLE FitnessClasses ALTER COLUMN EndTime DATETIME NULL;
GO
---- UNIQUE constraint 
--Members UNIQUE
ALTER TABLE Members
ADD CONSTRAINT UC_MemberPhoneNumber UNIQUE (MemberPhoneNumber); 

ALTER TABLE Members
ADD CONSTRAINT UC_MemberEmail UNIQUE (Email);  

--Subscriptions UNIQUE
ALTER TABLE Subscriptions
ADD CONSTRAINT UC_SubscriptionSubscriptionName UNIQUE (SubscriptionName); 

--FitnessClassBookings UNIQUE
ALTER TABLE FitnessClassBookings
ADD CONSTRAINT UC_FitnessClassBookingComboFitnessClassIdMemberId UNIQUE (FitnessClassId, MemberId); 

--FitnessClasses UNIQUE
ALTER TABLE FitnessClasses
ADD CONSTRAINT UC_FitnessClassComboStartTimeEndTimeWeekdayIdRoomIdFitnessCenterId UNIQUE (StartTime, EndTime, WeekdayId, RoomId, FitnessCenterId); 

--Weekdays UNIQUE
ALTER TABLE Weekdays
ADD CONSTRAINT UC_WeekdayWeekdayName UNIQUE (WeekdayName); 

--ClassTypes UNIQUE
ALTER TABLE ClassTypes
ADD CONSTRAINT UC_ClassTypeDescription UNIQUE (Description);

--Rooms UNIQUE
ALTER TABLE Rooms
ADD CONSTRAINT UC_RoomRoomName UNIQUE (RoomName);

--Trainers UNIQUE
ALTER TABLE Trainers
ADD CONSTRAINT UC_TrainerEmail UNIQUE (Email); 

ALTER TABLE Trainers
ADD CONSTRAINT UC_TrainerPhoneNumber UNIQUE (TrainerPhoneNumber); 

--Rooms UNIQUE
ALTER TABLE FitnessCenters
ADD CONSTRAINT UC_FitnessCenterLocation UNIQUE ([Location]);
GO

---- CHECK constraints som sikrer valid data
--Members CHECK
ALTER TABLE Members
ADD CONSTRAINT CHK_MemberBirthdateOlderThan18
CHECK (DATEDIFF(YEAR, Birthdate, GETDATE()) >= 18);

ALTER TABLE Members
ADD CONSTRAINT CHK_MemberCreatedDateGreaterThanBirthdate
CHECK (CreatedDate > Birthdate); 

ALTER TABLE Members
ADD CONSTRAINT CK_MemberEmailValid
CHECK (CHARINDEX('@', Email) > 0);

ALTER TABLE Members
ADD CONSTRAINT CK_MemberMaxClassesPerMonthQtyMin0
CHECK (MaxClassesPerMonth > 0);

--FitnessClassBookings CHECK
ALTER TABLE FitnessClassBookings
ADD CONSTRAINT CHK_FitnessClassBookingBookingDateGreaterThanNow
CHECK (BookingDate > GETDATE()); 

--FitnessClasses CHECK
ALTER TABLE FitnessClasses
ADD CONSTRAINT CHK_FitnessClassesStartTimeLessThanEndTime
CHECK (StartTime < EndTime); 

ALTER TABLE FitnessClasses
ADD CONSTRAINT CHK_FitnessClassesMaxParticipants
CHECK (MaxParticipants <= 25);

ALTER TABLE FitnessClasses
ADD CONSTRAINT CHK_FitnessClassesCurrentParticipants
CHECK (CurrentParticipants <= 25);
GO
---- DEFAULT constraints
--Members DEFAULT
ALTER TABLE Members
ADD CONSTRAINT DF_MemberMaxClassesPerMonth
DEFAULT 10 FOR MaxClassesPerMonth;

--FitnessClasses DEFAULT
ALTER TABLE FitnessClasses
ADD CONSTRAINT DF_FitnessClassDuration
DEFAULT 60 FOR Duration;

ALTER TABLE FitnessClasses
ADD CONSTRAINT DF_FitnessClassCurrentParticipants
DEFAULT 0 FOR CurrentParticipants;

ALTER TABLE FitnessClasses
ADD CONSTRAINT DF_FitnessClassMaxParticipants
DEFAULT 25 FOR MaxParticipants;
GO


---- 6. Opret triggers 
-- Members AFTER UPDATE Trigger: Til at nulstille antal træninger når en Member opdatere sit medlemskab
CREATE TRIGGER trg_UpdateMaxClassesPerMonthBasedOnSubscription
ON Members
AFTER UPDATE
AS
BEGIN
	IF UPDATE(SubscriptionId) -- Tjekker hvis en subscription er opdateret
	BEGIN
		UPDATE Members
		SET MaxClassesPerMonth = 
			CASE 
				WHEN (SELECT SubscriptionName 
                      FROM Subscriptions 

					  /* Scanner hvis SubscriptionId er premium*/
                      WHERE SubscriptionId = (SELECT SubscriptionId 
                                              FROM INSERTED 
                                              WHERE INSERTED.MemberId = Members.MemberId)) = 'Premium'	
				THEN NULL -- Hvis subscription er premium bliver MaxClassesPerMonth sat til Null
				ELSE 10 -- Ellers er den sat til 10
			END
		WHERE MemberId IN (SELECT MemberId FROM INSERTED);
	END;
END;
GO


--FitnessClasses INSTEAD OF INSERT trigger: Til at forhindre overlappende træningshold
CREATE TRIGGER trg_PreventOverlappingClasses
ON FitnessClasses
INSTEAD OF INSERT
AS
BEGIN
    /*Tjekker overlappende træningshold*/
    IF EXISTS (
        SELECT 1
        FROM INSERTED NewClass -- Variabel navn NewClass
        JOIN FitnessClasses ExistingClass -- Variabel navn ExistingClass
		-- Samligner data fra Insert (NewClass) og existerende data (ExistingClass)
        ON NewClass.RoomId = ExistingClass.RoomId
           AND NewClass.FitnessCenterId = ExistingClass.FitnessCenterId
           AND (
               NewClass.StartTime < ExistingClass.EndTime -- Tjekker om tidspunktet på den nye træningshold starter før den existerende træningshold er afsluttet
               AND NewClass.EndTime > ExistingClass.StartTime -- Tjekker hvis den nye træningshold afslutter hvor den existerende træningshold stadig er i gang
           )
    )
    BEGIN
        PRINT 'Cannot create overlapping classes in the same room and location.';
    END
    ELSE
    BEGIN
        INSERT INTO FitnessClasses (ClassName, StartTime, EndTime, Duration, CurrentParticipants, MaxParticipants, WeekdayId, ClassTypeId, TrainerId, RoomId, FitnessCenterId)
        SELECT ClassName, StartTime, EndTime, Duration, CurrentParticipants, MaxParticipants, WeekdayId, ClassTypeId, TrainerId, RoomId, FitnessCenterId
        FROM INSERTED;
    END
END;
GO

-- FitnessClasses AFTER DELETE Trigger: Til at fjerne alle bookings fra en aflyst FitnessClass
CREATE TRIGGER trg_FitnessClassCanceled
ON FitnessClasses
AFTER DELETE
AS
BEGIN
    DELETE FROM FitnessClassBookings
    WHERE FitnessClassId IN (SELECT FitnessClassId FROM DELETED);
END;
GO

-- FitnessClassBookings INSTEAD OF INSERT Trigger: Til at forhindre en Member for at booke hvis de overskrider deres månedlige træninger, mens premium-abonnement har ubegrænset adgang
CREATE TRIGGER trg_PreventMemberFromBookingMoreThanAllowedLimit
ON FitnessClassBookings
INSTEAD OF INSERT
AS
BEGIN
    DECLARE @MemberId INT, @MaxClassesPerMonth INT, @ClassCount INT;

    SELECT @MemberId = MemberId
    FROM INSERTED;

    SELECT @MaxClassesPerMonth = MaxClassesPerMonth
    FROM Members
    WHERE MemberId = @MemberId;

	/*Tæller hvor mange bookings den enkelte member har i denne måned*/
    SELECT @ClassCount = COUNT(*)
    FROM FitnessClassBookings
    WHERE MemberId = @MemberId 
      AND MONTH(BookingDate) = MONTH(GETDATE())
      AND YEAR(BookingDate) = YEAR(GETDATE());

    IF @MaxClassesPerMonth IS NOT NULL AND @ClassCount >= @MaxClassesPerMonth -- Checker hvis en member har booket mere end den totale tilladt holdtræninger
    BEGIN
        PRINT 'Booking limit exceeded. The member cannot book more classes this month.';
    END
    ELSE
    BEGIN
        INSERT INTO FitnessClassBookings (BookingDate, FitnessClassId, MemberId)
        SELECT BookingDate, FitnessClassId, MemberId
        FROM INSERTED;
    END
END;
GO

-- FitnessClasses AFTER INSERT triggers: Til at forhindre en træningshold bliver overbooket
CREATE TRIGGER trg_PreventOverbookingOnFitnessClassBookings
ON FitnessClassBookings
AFTER INSERT
AS
BEGIN
    DECLARE @FitnessClassId INT, @MaxParticipants INT, @CurrentParticipants INT;

    SELECT @FitnessClassId = FitnessClassId
    FROM INSERTED;

    SELECT @MaxParticipants = MaxParticipants, @CurrentParticipants = CurrentParticipants
    FROM FitnessClasses
    WHERE FitnessClassId = @FitnessClassId;

    IF @CurrentParticipants >= @MaxParticipants -- Tjekekr hvis træningshold er fuld
    BEGIN
        PRINT 'Class is fully booked. Removing the most recent booking.';

        -- Sletter den overbooket FitnessClassBooking
        DELETE FROM FitnessClassBookings
        WHERE FitnessClassBookingiD IN (
            SELECT FitnessClassBookingiD
            FROM INSERTED
        );
    END
    ELSE
    BEGIN
        -- Else øge CurrentParticipants med +1
        UPDATE FitnessClasses
        SET CurrentParticipants = CurrentParticipants + 1
        WHERE FitnessClassId = @FitnessClassId;
    END
END;
GO

-- FitnessClassBookings AFTER DELETE Trigger: Til at trække fra antal tilmeldte diltagere med -1 i en FitnessClass, når en booking er slettet
CREATE TRIGGER trg_DecrementParticipantsOnCancel
ON FitnessClassBookings
AFTER DELETE
AS
BEGIN
    DECLARE @FitnessClassId INT;

	/*Får FitnessClassId fra den slettede booking*/
    SELECT @FitnessClassId = FitnessClassId 
    FROM DELETED;

    UPDATE FitnessClasses
    SET CurrentParticipants = CurrentParticipants - 1
    WHERE FitnessClassId = @FitnessClassId;
END;
GO

---- 7. Indsæt startdata
---- INSERT nødvendige data
INSERT INTO FitnessCenters ([Location])
VALUES 
('Ringsted'), 
('Køge'), 
('Roskilde'), 
('Holbæk');
GO

INSERT INTO Subscriptions (SubscriptionName, Benefit)
VALUES 
('Basis', 'Adgang til 10 træninger om måneden'), 
('Premium', 'Ubegrænset adgang til træninger');
GO

INSERT INTO ClassTypes ([Description])
VALUES 
('Intensiv Hop'), 
('Begynder Hop'), 
('Familie Hop');
GO

INSERT INTO Weekdays (WeekdayName)
VALUES 
('Monday'), 
('Tuesday'), 
('Wednesday'), 
('Thursday'), 
('Friday'), 
('Saturday'), 
('Sunday');
GO

INSERT INTO Rooms (RoomName)
VALUES 
('Room 1'), 
('Room 2');
GO

---- 8. Opret brugere og afprøv
-- Trainers INSERT
INSERT INTO Trainers (FirstName, LastName, TrainerPhoneNumber, CreatedDate, Email, [Password], FitnessCenterId)
VALUES 
('Camilla', 'Ryskjær', 11223344, GETDATE(), 'cr@zealand.dk', 'datamatiker1', 1), 
('Vibeke', 'Sandau', 55667788, GETDATE(), 'vs.johnson@zealand.dk', 'datamatiker2', 2);
GO

-- Addresses INSERT
INSERT INTO Addresses (StreetName, HouseNumber, PostalCode, City)
VALUES 
('Maglegårdsvej', '2', 4000, 'Roskilde'), 
('Femøvej', '3', 4700, 'Næstved');
GO

-- Members INSERT
INSERT INTO Members (FirstName, LastName, MemberPhoneNumber, [Password], CreatedDate, Email, Birthdate, AddressId, SubscriptionId, FitnessCenterId)
VALUES 
('Kevin', 'Waweru', 99112233, 'kæmpePassword', GETDATE(), 'kw@gmail.com', '2000-02-20', 1, 1, 1), 
('Ana', 'Petrova', 87654321, 'lillePassword', GETDATE(), 'ap@gmail.com', '1995-05-12', 2, 2, 2);
GO


-- FitnessClasses INSERT
INSERT INTO FitnessClasses (ClassName, StartTime, EndTime, WeekdayId, ClassTypeId, TrainerId, RoomId, FitnessCenterId)
VALUES 
('Morgen Hop', '2024-12-02 08:00:00', '2024-12-02 09:00:00', 1, 1, 1, 1, 1), 
('Middag Hop', '2024-12-04 12:00:00', '2024-12-04 14:00:00', 3, 2, 2, 2, 2), 
('Aften Hop', '2024-12-06 18:00:00', '2024-12-06 19:00:00', 5, 3, 1, 2, 3);
GO

-- FitnessClassBookings INSERT
INSERT INTO FitnessClassBookings (BookingDate, FitnessClassId, MemberId)
VALUES 
(DATEADD(SECOND, 1, GETDATE()), 1, 100000), -- Kevin booker Morgen hop som er en mandag, Intensiv hop, træneren er Camilla, rum 1, i Ringested
(DATEADD(SECOND, 1, GETDATE()), 2, 100001);  -- Ana booker middag hop som er en onsdag, hop for begyndere, træneren er Vibeke, rum 2, i Køge
GO
