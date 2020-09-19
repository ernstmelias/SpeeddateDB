# Speeddate-test

Test database with a collection of tables and procedures.

## Getting Started

To begin using this template, choose one of the following options to get started:
* Clone the repo: `git clone https://github.com/ernstmelias/SpeeddateDB.git`
* Fork the repo

## Bugs and Issues

Have a bug or an issue with this template? [Open a new issue](https://github.com/ernstmelias/SpeeddateDB/issues) here on GitHub. 

## Tables

``` SQL
DROP TABLE IF EXISTS `countries`;
CREATE TABLE `countries` (
  `CountryID` int(11) NOT NULL AUTO_INCREMENT,
  `CountryCode` varchar(3) NOT NULL,
  `CountryName` varchar(150) NOT NULL,
  `phonecode` int(11) NOT NULL,
  PRIMARY KEY (`CountryID`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8;
``` 

``` SQL
DROP TABLE IF EXISTS `messages`;
CREATE TABLE `messages` (
  `MessageID` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `User_UserID` int(11) NOT NULL,
  `FromUserID` int(11) NOT NULL,
  `Preview` varchar(30) NOT NULL,
  `Message` varchar(500) NOT NULL,
  `Read` tinyint(1) NOT NULL DEFAULT '0',
  `DateSent` datetime NOT NULL,
  `DateRead` datetime NOT NULL,
  PRIMARY KEY (`MessageID`),
  KEY `fk_Messages_User1` (`User_UserID`),
  CONSTRAINT `fk_Messages_User1` FOREIGN KEY (`User_UserID`) REFERENCES `user` (`UserID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

``` SQL
DROP TABLE IF EXISTS `states`;
CREATE TABLE `states` (
  `StateName` varchar(40) NOT NULL,
  `StateShort` varchar(2) NOT NULL,
  `StateID` tinyint(2) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`StateID`),
  UNIQUE KEY `StateName_UNIQUE` (`StateName`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8;
```

``` SQL
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `UserID` int(11) NOT NULL AUTO_INCREMENT,
  `FirstName` varchar(45) NOT NULL,
  `LastName` varchar(45) NOT NULL,
  `Email` varchar(255) NOT NULL,
  `Phone` varchar(20) DEFAULT NULL,
  `Birthday` varchar(100) NOT NULL,
  `DisplayName` varchar(60) DEFAULT NULL,
  `Password` varchar(255) NOT NULL,
  `CountryID` smallint(3) NOT NULL,
  `StateID` tinyint(2) DEFAULT NULL,
  `ZipCode` int(11) DEFAULT NULL,
  `Enabled` tinyint(1) NOT NULL,
  `AccountActive` tinyint(1) DEFAULT NULL,
  `AccountType` tinyint(3) NOT NULL,
  `TurnOffTracking` BIT  NOT NULL DEFAULT 0,
  `AccessLocation` BIT NOT NULL DEFAULT 0,
  `AccessCamera` BIT NOT NULL DEFAULT 0,
  `SendNotifications` BIT NOT NULL DEFAULT 0,
  `lastVisited`  datetime NOT NULL,
  `lastActive` datetime NOT NULL,
  `HomeLocationID` int(11) NOT NULL,
  `SchoolLocationID` int(11),
  `UserActivityReportID` int(11),
  `UserFinanceReportID` int(11),
  `UserLocationReportID` int(11),
  `UserLocationRecommendationReportID` int(11),
  `UserTimeManagementReportID` int(11),
  PRIMARY KEY (`UserID`),
  UNIQUE KEY `UserID_UNIQUE` (`UserID`),
  UNIQUE KEY `Username_UNIQUE` (`DisplayName`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8;
``` 

```SQL
DROP TABLE IF EXISTS `HomeLocations`;
CREATE TABLE `HomeLocations` 
(
  `UserID` int(11) NOT NULL,
  `LocationID` int(11) NOT NULL, 
  UNIQUE KEY `UserID_UNIQUE` (`UserID`)
);

```

```SQL
DROP TABLE IF EXISTS `SchoolLocations`;
CREATE TABLE `SchoolLocations`
(
 `UserID` int(11) NOT NULL,
 `LocationID` int(11) NOT NULL,
 UNIQUE KEY `UserID_UNIQUE` (`UserID`)
);

```
```SQL
DROP TABLE IF EXISTS `WorkLocations`;
CREATE TABLE `WorkLocations`
(
 `UserID` int(11) NOT NULL,
 `LocationID` int(11) NOT NULL,
 UNIQUE KEY `UserID_UNIQUE` (`UserID`)
)

```

```SQL
DROP TABLE IF EXISTS `Locations`;
CREATE TABLE `Locations`
(
`LocationID` int(11) NOT NULL PRIMARY AUTO_INCREMENT,
`LocationName` VARCHAR(100) NOT NULL,
`State` VARCHAR(30),
`StateShort` VARCHAR(2),
`ZipCode` MEDIUMINT(9) NOT NULL,
`Description` VARCHAR(155),
`InOperation` BIT NOT NULL DEFAULT 0,
`MonthEstablished` tinyint(2),
`DayEstablished` tinyint(2),
`yearEstablished` SMALLINT(4)
`TimeOperation` VARCHAR(255),
`PeakHours` VARCHAR(100),
`Address` VARCHAR(255),
`Lat` DECIMAL(10,10),
`Lon` DECIMAL(10,10),
`CountryID` int(11),
`RegionID` int(11),
`ContinentID` int(11),
`HempisphereID` int(11),
`ipAddress`  VARCHAR(20)
)
```
```SQL
DROP TABLE IF EXISTS `UserLocations`;
CREATE TABLE `UserLocations`
(
`UserID` int(11) NOT NULL PRIMARY,
`LocationID` int(11) NOT NULL,
`VisitCounts` int(11) NOT NULL DEFAULT 0,
`FirstVisitDate` datetime ,
`LastVisitDate` datetime,
`LongestTimeSpent` int(11),
`ShortestTimeSpent` int(11),
)
```

```SQL
--- User Location Audit Table---
---- Tracks daily movement --- 
DROP TABLE IF EXISTS `UserLocationAuditTable`;
CREATE TABLE `UserLocationAuditTable`
(
`UserID` int(11) NOT NULL ,
`LocationID` int(11) NOT NULL,
`TimeArrived` datetime,
`timeleft` datetime
)
```

```SQL
DROP TABLE IF EXISTS `Friends`;
CREATE TABLE `Friends`
(
`UserID` int(11) NOT NULL,
`FriendID` int(11) NOT NULL
)
```

## Procedures

```SQL
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `getLoadProfile`(IN userid INT )
BEGIN
    SELECT
UserProfileID,
Height,
PersonalityType,
Education,
School
Salary,
Bio,
HasChildren,
WantChildren,
Drinks,
Smokes,
HardDrugs,
Gender,
Race,
BodyType,
RelationshipStatus
FROM userprofile WHERE  User_UserID = userid;

  END ;;
DELIMITER ;

```
```SQL
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `loadUser`(IN FrontEmail  VARCHAR(45))
BEGIN
    SELECT
	 UserID,
	 FirstName,
	 LastName,
	 Email,
	 Phone,
	 Birthday,
	 DisplayName,
	 Password,
	 CountryID,
	 StateID,
	 ZipCode,
	 Enabled,
	 AccountActive,
	 AccountType
	 FROM user WHERE Email =FrontEmail;
  END ;;
DELIMITER ;
```

```SQL
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `RegisterUser`(
 
 IN firstName VARCHAR(45),
 IN lastName  VARCHAR(45),
 IN FrontEmail  VARCHAR(45),
 IN Phone  VARCHAR(20),
 IN Birthday  VARCHAR(100),
 IN DisplayName VARCHAR (60),
 IN Password  VARCHAR(255),
 IN FrontCountry VARCHAR(100),
 IN State VARCHAR(2),
 IN ZipCode INT
)
BEGIN
  DECLARE accountType tinyint(1) DEFAULT 1;
  DECLARE StateID tinyint(2) ;
  DECLARE ExistingUser INT ; 
    SELECT COUNT(*) INTO ExistingUser FROM user WHERE Email= FrontEmail;
	IF  ExistingUser > 0 THEN
	SIGNAL SQLSTATE '45000'
	SET MESSAGE_TEXT = 'A user already exists with that email. Please use another email.';
	ELSE
	 IF FrontCountry = 231 THEN
	 SELECT StateID = State;
	 END IF ;
	 INSERT INTO user 
	 (`FirstName`,
	 `LastName`,
	 `Email`,
	 `Phone`,
	 `Birthday`,
	 `DisplayName`,
	 `Password`,
	 `CountryID`, 
	 `StateID`,
	 `ZipCode`,
	 `Enabled`,
	 `AccountActive`,
	 `AccountType`)
	 VALUES 
	 (firstName,
	  lastName,
	  FrontEmail,
	  Phone,
	  Birthday,
	  DisplayName,
	  Password,
	  FrontCountry,
	  StateID,
	  ZipCode,
	  0,
	  0,
	  1);
	END IF;
  END ;;
DELIMITER ;
```

```SQL
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `uptAccountNormal`(
     IN uSERID INT,
	 IN firstName VARCHAR(45),
	 IN lastName VARCHAR(45),
	 IN email VARCHAR(255),
	 IN phone VARCHAR(20),
	 IN birthday VARCHAR(100),
	 IN countryID SMALLINT(3),
	 IN stateID  TINYINT(2),
	 IN zipCode INT
    )
BEGIN
     start transaction;
	 UPDATE user SET FirstName=firstName, LastName=lastName,Email=email,Phone=phone,Birthday=birthday,CountryID=countryID,StateID=stateID,ZipCode=zipCode 
	 WHERE UserID=uSERID;
	 commit; 
   END ;;
DELIMITER ;

```

```SQL
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `uptAccountWithPass`(
     IN uSERID INT,
	 IN firstName VARCHAR(45),
	 IN lastName VARCHAR(45),
	 IN email VARCHAR(255),
	 IN phone VARCHAR(20),
	 IN birthday VARCHAR(100),
	 IN password VARCHAR(255),
	 IN countryID SMALLINT(3),
	 IN stateID  TINYINT(2),
	 IN zipCode INT
    )
BEGIN
     start transaction;
	 UPDATE user SET FirstName=firstName, LastName=lastName,Email=email,Phone=phone,Birthday=birthday,Password=password,CountryID=countryID,StateID=stateID,ZipCode=zipCode 
	 WHERE UserID=uSERID;
	 commit; 
   END ;;
DELIMITER ;
```

## FAQ
* MYSQL
* SQL
