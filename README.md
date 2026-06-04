# TP2: Hospital Management System

## Objectives
- Design a complex relational database
- Implement advanced relationships
- Manage medical data
- Perform complex queries

---

## 📊 Database Description

Create a **Hospital Management System** to manage:
- Medical specialties
- Doctors
- Patients
- Consultations
- Medications
- Prescriptions

---

## 🗂️ Tables to Create

### 1. Table: `specialties`
Medical specialties offered.

**Columns:**
- `specialty_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `specialty_name` (VARCHAR(100), NOT NULL, UNIQUE)
- `description` (TEXT)
- `consultation_fee` (DECIMAL(10, 2), NOT NULL)

---

### 2. Table: `doctors`
Doctor information.

**Columns:**
- `doctor_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `last_name` (VARCHAR(50), NOT NULL)
- `first_name` (VARCHAR(50), NOT NULL)
- `email` (VARCHAR(100), UNIQUE, NOT NULL)
- `phone` (VARCHAR(20))
- `specialty_id` (INT, NOT NULL, FOREIGN KEY → specialties)
- `license_number` (VARCHAR(20), UNIQUE, NOT NULL)
- `hire_date` (DATE)
- `office` (VARCHAR(100))
- `active` (BOOLEAN, DEFAULT TRUE)

**Constraints:**
- Foreign key: `specialty_id` references `specialties(specialty_id)`
- ON DELETE RESTRICT, ON UPDATE CASCADE

---

### 3. Table: `patients`
Patient records.

**Columns:**
- `patient_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `file_number` (VARCHAR(20), UNIQUE, NOT NULL)
- `last_name` (VARCHAR(50), NOT NULL)
- `first_name` (VARCHAR(50), NOT NULL)
- `date_of_birth` (DATE, NOT NULL)
- `gender` (ENUM('M', 'F'), NOT NULL)
- `blood_type` (VARCHAR(5))
- `email` (VARCHAR(100))
- `phone` (VARCHAR(20), NOT NULL)
- `address` (TEXT)
- `city` (VARCHAR(50))
- `province` (VARCHAR(50))
- `registration_date` (DATE, DEFAULT CURRENT_DATE)
- `insurance` (VARCHAR(100))
- `insurance_number` (VARCHAR(50))
- `allergies` (TEXT)
- `medical_history` (TEXT)

---

### 4. Table: `consultations`
Medical consultations.

**Columns:**
- `consultation_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `patient_id` (INT, NOT NULL, FOREIGN KEY → patients)
- `doctor_id` (INT, NOT NULL, FOREIGN KEY → doctors)
- `consultation_date` (DATETIME, NOT NULL)
- `reason` (TEXT, NOT NULL)
- `diagnosis` (TEXT)
- `observations` (TEXT)
- `blood_pressure` (VARCHAR(20))
- `temperature` (DECIMAL(4, 2))
- `weight` (DECIMAL(5, 2))
- `height` (DECIMAL(5, 2))
- `status` (ENUM: 'Scheduled', 'In Progress', 'Completed', 'Cancelled', DEFAULT 'Scheduled')
- `amount` (DECIMAL(10, 2))
- `paid` (BOOLEAN, DEFAULT FALSE)

**Constraints:**
- Foreign keys with appropriate DELETE/UPDATE rules

---

### 5. Table: `medications`
Pharmacy medication catalog.

**Columns:**
- `medication_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `medication_code` (VARCHAR(20), UNIQUE, NOT NULL)
- `commercial_name` (VARCHAR(150), NOT NULL)
- `generic_name` (VARCHAR(150))
- `form` (VARCHAR(50)) - Tablet, Syrup, Injection, etc.
- `dosage` (VARCHAR(50))
- `manufacturer` (VARCHAR(100))
- `unit_price` (DECIMAL(10, 2), NOT NULL)
- `available_stock` (INT, DEFAULT 0)
- `minimum_stock` (INT, DEFAULT 10)
- `expiration_date` (DATE)
- `prescription_required` (BOOLEAN, DEFAULT TRUE)
- `reimbursable` (BOOLEAN, DEFAULT FALSE)

---

### 6. Table: `prescriptions`
Medical prescriptions.

**Columns:**
- `prescription_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `consultation_id` (INT, NOT NULL, FOREIGN KEY → consultations)
- `prescription_date` (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- `treatment_duration` (INT) - Duration in days
- `general_instructions` (TEXT)

**Constraints:**
- Foreign key with CASCADE

---

### 7. Table: `prescription_details`
Details of prescribed medications.

**Columns:**
- `detail_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `prescription_id` (INT, NOT NULL, FOREIGN KEY → prescriptions)
- `medication_id` (INT, NOT NULL, FOREIGN KEY → medications)
- `quantity` (INT, NOT NULL, CHECK > 0)
- `dosage_instructions` (VARCHAR(200), NOT NULL)
- `duration` (INT, NOT NULL)
- `total_price` (DECIMAL(10, 2))

**Constraints:**
- Foreign keys with appropriate rules

---

## Required Indexes

Create indexes on:
- `patients(last_name, first_name)`
- `consultations(consultation_date)`
- `consultations(patient_id)`
- `consultations(doctor_id)`
- `medications(commercial_name)`
- `prescriptions(consultation_id)`

---

## 📝 Test Data to Insert

### Specialties (6 specialties)
Including: General Medicine, Cardiology, Pediatrics, Dermatology, Orthopedics, Gynecology

### Doctors (6 doctors)
- One per specialty
- With valid license numbers

### Patients (8 patients)
- Various ages (children, adults, seniors)
- Different blood types
- Some with allergies
- With/without insurance

### Consultations (8 consultations)
- Mix of completed and scheduled
- Different dates
- Some paid, some unpaid
- Various vital signs

### Medications (10 medications)
- Different forms and prices
- Various stock levels
- Some requiring prescription

### Prescriptions (7 prescriptions)
- Linked to consultations
- With treatment instructions

### Prescription Details (12 details)
- Multiple medications per prescription
- Dosage instructions
- Calculated prices

