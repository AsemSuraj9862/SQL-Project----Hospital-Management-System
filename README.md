# SQL-Project----Hospital-Management-System
Schema specification — 11 tables, precise data types, constraints, audit fields, and IsActive on every table

# Contents
  U. User Table (foundation)
  1.Department Table
  2.Doctor Table
  3.Patient Table
  4.Appointment Table
  5.Ward Table
  6.Bed Table
  7.Inpatient Admission Table
  8.Service Master Table
  9.IP Service Table
  10.Billing Table

## 1.User Table(foundation)
 #   Field	             Data Type	           Constraint
    DepartmentID	      INT                   PKIDENTITY(1,1), NOT NULL
    DepartmentName    	VARCHAR(100)          NOT NULL
    Description	        VARCHAR(500)	        NULL
    IsActive	          BIT                  	DEFAULT 1
    CreatedBy	          INT	                  AUDITFKNOT NULL → User.UserID
    CreatedDate        	DATETIME	            AUDITDEFAULT GETDATE()
    ModifiedBy	        INT	                  AUDITFKNULL → User.UserID
    ModifiedDate	      DATETIME	            AUDITNULL
    
## 2.Doctor Table
#   Field	                Data Type            	  Constraint
 DoctorID	              INT	                  PKIDENTITY(1,1), NOT NULL
DoctorCode             	VARCHAR(20)           UQNOT NULL, UNIQUE
DoctorName            	VARCHAR(150)          NOT NULL
DepartmentID	          INT	                  FKNOT NULL → Department.DepartmentID
Specialization        	VARCHAR(100)          NULL
Qualification	          VARCHAR(200)        	NULL
MobileNo	              VARCHAR(20)          	NULL
Email	                  VARCHAR(150)	        NULL
RegistrationNo        	VARCHAR(50)          	NULL
IsActive	              BIT	                  DEFAULT 1
CreatedBy              	INT	                  AUDITFKNOT NULL → User.UserID
CreatedDate	            DATETIME	            AUDITDEFAULT GETDATE()
ModifiedBy	            INT	                  AUDITFKNULL → User.UserID
ModifiedDate	          DATETIME	            AUDITNULL
  # Relationship
           DepartmentID references Department(DepartmentID).
  
## 3.Patient Table
#   Field	                Data Type	            Constraint
PatientID	              BIGINT              	PKIDENTITY(1,1), NOT NULL
UHID                   	VARCHAR(30)	          UQNOT NULL, UNIQUE
PatientName            	VARCHAR(150)          NOT NULL
Gender	                CHAR(1)              	NULL — 'M' / 'F' / 'O'
DateOfBirth            	DATE	                NULL
BloodGroup	            VARCHAR(5)          	NULL
MobileNo	              VARCHAR(20)          	NULL
Email                  	VARCHAR(150)          NULL
Address	                VARCHAR(500)          NULL
City	                  VARCHAR(100)	        NULL
State	                  VARCHAR(100)	        NULL
PinCode                	VARCHAR(10)          	NULL
EmergencyContactName   	VARCHAR(150)	        NULL
EmergencyContactNo	    VARCHAR(20)	          NULL
RegistrationDate	      DATETIME	            DEFAULT GETDATE()
IsActive	              BIT                	  DEFAULT 1
CreatedBy              	INT                  	AUDITFKNOT NULL → User.UserID
CreatedDate	            DATETIME	            AUDITDEFAULT GETDATE()
ModifiedBy	            INT                  	AUDITFKNULL → User.UserID
ModifiedDate           	DATETIME	            AUDITNULL

## 4.Appointment Table
# Field                  	Data Type             	Constraint
AppointmentID          	BIGINT	                 PKIDENTITY(1,1), NOT NULL
PatientID              	BIGINT	                 FKNOT NULL → Patient.PatientID
DoctorID	              INT	                     FKNOT NULL → Doctor.DoctorID
AppointmentDate        	DATE	                   NOT NULL
AppointmentTime	        TIME                     NOT NULL
AppointmentType        	VARCHAR(50)              NULL
Reason	                VARCHAR(500)            	NULL
Status                	VARCHAR(30)	              DEFAULT 'Scheduled'
Remarks	                VARCHAR(500)	            NULL
IsActive	              BIT	                      DEFAULT 1  
CreatedBy              	INT	                      AUDITFKNOT NULL → User.UserID
CreatedDate            	DATETIME	                AUDITDEFAULT GETDATE()
ModifiedBy	            INT	                      AUDITFKNULL → User.UserID
ModifiedDate	          DATETIME	                AUDITNULL

# Relationships
              PatientID → Patient(PatientID)
              DoctorID → Doctor(DoctorID)

## 5.Ward Table
# Field	                  Data Type	                 Constraint
WardID	                INT                      	PKIDENTITY(1,1), NOT NULL
WardName	              VARCHAR(100)            	NOT NULL
WardType               	VARCHAR(50)	              NULL — e.g. General, ICU, Private
FloorNo	                INT                      	NULL
Capacity	              INT                       NOT NULL
IsActive	              BIT	                      DEFAULT 1
CreatedBy              	INT	                      AUDITFKNOT NULL → User.UserID
CreatedDate            	DATETIME                 	AUDITDEFAULT GETDATE()
ModifiedBy	            INT	                      AUDITFKNULL → User.UserID
ModifiedDate          	DATETIME	                AUDITNULL

## 6.Bed Table
# Field	                  Data Type	                Constraint
BedID	                  INT	                      PKIDENTITY(1,1), NOT NULL
WardID	                INT	                        FKNOT NULL → Ward.WardID
BedNo	                  VARCHAR(30)	                NOT NULL
BedStatus	              VARCHAR(30)	                DEFAULT 'Available'
IsActive	              BIT                        	DEFAULT 1
CreatedBy	              INT                        	AUDITFKNOT NULL → User.UserID
CreatedDate	            DATETIME                    AUDITDEFAULT GETDATE()
ModifiedBy	            INT	                        AUDITFKNULL → User.UserID
ModifiedDate            DATETIME                  	AUDITNULL
  #    Relationship
          WardID → Ward(WardID)

## 7.Inpatient Admission Table
# Field	                  Data Type                    Constraint
IPID	                  BIGINT	                      PKIDENTITY(1,1), NOT NULL
IPNo                  	VARCHAR(30)                      	UQNOT NULL, UNIQUE
PatientID	              BIGINT	                      FKNOT NULL → Patient.PatientID
DoctorID	              INT	                          FKNOT NULL → Doctor.DoctorID
BedID	                  INT	                          FKNOT NULL → Bed.BedID
AdmissionDate	          DATETIME	                    NOT NULL
DischargeDate          	DATETIME	                    NULL — must be ≥ AdmissionDate when set
AdmissionType	          VARCHAR(50)	                  NULL — e.g. Emergency, Planned, Referral
AdmissionReason	        VARCHAR(500)                  ULL
Diagnosis	              VARCHAR(500)	                NULL
Status	                VARCHAR(30)                  	DEFAULT 'Admitted'
IsActive	              BIT	                          DEFAULT 1
CreatedBy	              INT	                          AUDITFKNOT NULL → User.UserID
CreatedDate	            DATETIME	                    AUDITDEFAULT GETDATE()
ModifiedBy	            INT                        	  AUDITFKNULL → User.UserID
ModifiedDate          	DATETIME	                    AUDITNULL
  # Relationships
        PatientID → Patient(PatientID)
        DoctorID → Doctor(DoctorID)
        BedID → Bed(BedID)

## 8.Service Master Table
# Field                  	Data Type	                     Constraint
ServiceID	              INT	                            PKIDENTITY(1,1), NOT NULL
ServiceCode	            VARCHAR(30)                    	UQNOT NULL, UNIQUE
ServiceName             VARCHAR(200)	                  NOT NULL
ServiceCategory	        VARCHAR(100)                  	NULL — e.g. Consultation, Room, Nursing, Lab, Procedure
Rate	                  DECIMAL(18,2)	                  NOT NULL, CHECK (Rate ≥ 0)
GSTPercent            	DECIMAL(5,2)	                  DEFAULT 0, CHECK (0–100)
IsActive	              BIT	                            DEFAULT 1
CreatedBy	              INT	                            AUDITFKNOT NULL → User.UserID
CreatedDate           	DATETIME	                      AUDITDEFAULT GETDATE()
ModifiedBy	            INT                            	AUDITFKNULL → User.UserID
ModifiedDate	          DATETIME	                      AUDITNULL

## 9.IP Service Table
# Field                  	Data Type                    	Constraint
IPServiceID	            BIGINT	                      PKIDENTITY(1,1), NOT NULL
IPID	                  BIGINT	                      FKNOT NULL → IPAdmission.IPID
ServiceID	              INT                          	FKNOT NULL → ServiceMaster.ServiceID
ServiceDate	            DATETIME	                    DEFAULT GETDATE()
Quantity	              DECIMAL(18,2)                	DEFAULT 1, CHECK (Quantity > 0)
Rate	                  DECIMAL(18,2)                	NOT NULL, CHECK (Rate ≥ 0)
Discount	              DECIMAL(18,2)                	DEFAULT 0, CHECK (Discount ≥ 0)
GSTAmount              	DECIMAL(18,2)                	DEFAULT 0, CHECK (GSTAmount ≥ 0)
Remarks	                VARCHAR(500)                	NULL
IsActive              	BIT	                          DEFAULT 1
CreatedBy	              INT                          	AUDITFKNOT NULL → User.UserID
CreatedDate            	DATETIME                     	AUDITDEFAULT GETDATE()
ModifiedBy	            INT	                          AUDITFKNULL → User.UserID
ModifiedDate          	DATETIME                    	AUDITNULL
  # Relationships
      IPID → IPAdmission(IPID)
      ServiceID → ServiceMaster(ServiceID)

## 10.Billing Table
# Field                  	Data Type	                    Constraint
BillID	                BIGINT                      	PKIDENTITY(1,1), NOT NULL
BillNo	                VARCHAR(30)                  	UQNOT NULL, UNIQUE
PatientID	              BIGINT	                      FKNOT NULL → Patient.PatientID
IPID	                  BIGINT	                      FKNULL → IPAdmission.IPID (outpatient bills leave this NULL)
BillDate	              DATETIME	                    DEFAULT GETDATE()
GrossAmount	            DECIMAL(18,2)                	DEFAULT 0, CHECK (≥ 0)
Discount	              DECIMAL(18,2)	                DEFAULT 0, CHECK (≥ 0)
TaxAmount	              DECIMAL(18,2)	                DEFAULT 0, CHECK (≥ 0)
NetAmount	              DECIMAL(18,2)	                DEFAULT 0 — = GrossAmount − Discount + TaxAmount
PaidAmount            	DECIMAL(18,2)                	DEFAULT 0, CHECK (≥ 0)
BalanceAmount          	DECIMAL(18,2)	                DEFAULT 0 — = NetAmount − PaidAmount
Status	                VARCHAR(30)	                  DEFAULT 'Pending'
IsActive	              BIT	                          DEFAULT 1 — flags a voided/cancelled bill as inactive
CreatedBy	              INT	                          AUDITFKNOT NULL → User.UserID
CreatedDate	            DATETIME                    	AUDITDEFAULT GETDATE()
ModifiedBy	            INT                          	AUDITFKNULL → User.UserID
ModifiedDate	          DATETIME                    	AUDITNULL
  # Relationships
      PatientID → Patient(PatientID)
      IPID → IPAdmission(IPID) — nullable, used only for inpatient bills

