# Boutique CRM System Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [Appointment Management Module](#appointment-management-module)
3. [Lead Management Module](#lead-management-module)
4. [Customer Management Module](#customer-management-module)
5. [Material Management Module](#material-management-module)
6. [Sales Management Module](#sales-management-module)
7. [Order Management Module](#order-management-module)
8. [Employee Management Module](#employee-management-module)
9. [Simple Accounting Module](#simple-accounting-module)
10. [User Roles & Permissions](#user-roles--permissions)
11. [Technical Requirements](#technical-requirements)

## System Overview

The Boutique CRM is a comprehensive customer relationship and order management system designed specifically for fashion boutiques and custom tailoring businesses. The system manages the entire customer journey from lead generation through final delivery, with integrated appointment scheduling, material management, and production pipeline tracking.

### Key Features
- **Lead-to-Customer Conversion Pipeline**
- **Appointment Management from Website Integration**
- **Custom Order Processing with Visual Pipeline**
- **Material and Inventory Management**
- **Employee Task Assignment and Tracking**
- **Sales Analytics and Reporting**
- **Basic Accounting Integration**

---

## Appointment Management Module

### Purpose
Manages appointments captured from the boutique's website landing page and manual entries. This module serves as the initial touchpoint for customer engagement and handles scheduling for consultations, measurements, trials, and deliveries.

### Core Features
- **Website Form Integration**: Seamless capture of appointment requests from the boutique's landing page
- **Appointment Scheduling Calendar**: Interactive calendar for scheduling and managing appointments
- **Multi-Service Support**: Handle different types of appointments (consultation, measurement, trial, delivery)
- **Customer Notification System**: Automated SMS/email reminders and confirmations
- **Status Tracking**: Real-time status updates (Scheduled, Confirmed, Completed, Cancelled)
- **Employee Assignment**: Assign appointments to specific staff members
- **Conflict Management**: Prevent double-booking and scheduling conflicts
- **Walk-in Management**: Handle walk-in customers and emergency appointments

### Data Model
```
Appointment {
  id: UUID (Primary Key)
  customer_name: String (Required)
  phone: String (Required)
  email: String (Optional)
  appointment_date: DateTime (Required)
  appointment_time: Time (Required)
  service_type: Enum [Consultation, Measurement, Trial, Delivery, Alteration]
  status: Enum [Scheduled, Confirmed, In_Progress, Completed, Cancelled, No_Show]
  notes: Text (Optional)
  source: Enum [Website, Phone, Walk_in, Referral]
  assigned_employee: Foreign Key -> Employee
  duration_minutes: Integer (Default: 60)
  reminder_sent: Boolean (Default: false)
  cancellation_reason: String (Optional)
  rescheduled_from: Foreign Key -> Appointment (Optional)
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Website Form Submission/Manual Entry → Appointment Creation → Employee Assignment → 
Customer Confirmation → Reminder Notifications → Appointment Execution → 
Status Update → Follow-up Actions
```

**Detailed Process Steps:**
1. **Appointment Request**: Customer submits request via website or phone call
2. **Availability Check**: System checks employee availability and slot conflicts
3. **Appointment Creation**: New appointment record created with all details
4. **Employee Assignment**: Based on service type and availability
5. **Customer Confirmation**: Automated confirmation sent via preferred method
6. **Reminder System**: 24-hour and 2-hour reminders sent automatically
7. **Appointment Execution**: Staff updates status during the appointment
8. **Completion Actions**: Post-appointment follow-up and potential lead conversion

---

## Lead Management Module

### Purpose
Comprehensive lead management system with automatic Meta integration and manual lead entry capabilities. This module handles the complete lead lifecycle from capture to conversion, ensuring no potential customer is lost in the process.

### Core Features
- **CRUD Operations**: Complete Create, Read, Update, Delete functionality for leads
- **Meta Ads Integration**: Automatic lead capture from Facebook/Instagram advertising campaigns
- **Google Ads Integration**: Capture leads from Google advertising platforms
- **Manual Lead Entry**: Sales staff can manually add leads from various sources
- **Lead Scoring System**: Automated scoring based on engagement and profile data
- **Lead Qualification Workflow**: Structured process to qualify leads effectively
- **Follow-up Scheduling**: Automated and manual follow-up reminders
- **Lead-to-Customer Conversion**: Seamless conversion tracking and process
- **Source Attribution**: Track lead sources for marketing ROI analysis
- **Bulk Lead Import**: Import leads from external sources and spreadsheets

### Data Model
```
Lead {
  id: UUID (Primary Key)
  name: String (Required)
  phone: String (Required)
  email: String (Optional)
  source: Enum [Meta, Google, Website, Referral, Walk_in, Exhibition, Social_Media]
  status: Enum [New, Contacted, Qualified, Interested, Converted, Lost, Do_Not_Contact]
  score: Integer (1-100, Auto-calculated)
  interest_area: String (e.g., "Wedding Dress", "Casual Wear", "Party Wear")
  budget_range: Enum [Under_10k, 10k_25k, 25k_50k, 50k_100k, Above_100k]
  preferred_contact_method: Enum [Phone, Email, WhatsApp, SMS]
  notes: Text
  assigned_employee: Foreign Key -> Employee
  follow_up_date: DateTime
  last_contact_date: DateTime
  contact_attempts: Integer (Default: 0)
  conversion_probability: Integer (1-100)
  utm_source: String (Optional)
  utm_campaign: String (Optional)
  converted_to_customer: Foreign Key -> Customer (Optional)
  conversion_date: DateTime (Optional)
  loss_reason: String (Optional)
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Lead Capture → Lead Assignment → Initial Contact → Lead Qualification → 
Interest Assessment → Follow-up Scheduling → Appointment Setting → 
Conversion/Loss Decision
```

**Detailed Process Steps:**
1. **Lead Capture**: Automatic from ads or manual entry by sales staff
2. **Lead Scoring**: System automatically scores based on source, budget, and engagement
3. **Assignment**: Leads assigned to sales employees based on workload and expertise
4. **Initial Contact**: First contact attempt within 24 hours of lead creation
5. **Qualification**: Assess genuine interest, budget, and timeline
6. **Interest Assessment**: Determine specific requirements and preferences
7. **Follow-up Management**: Scheduled follow-ups based on customer response
8. **Appointment Conversion**: Convert qualified leads to appointments
9. **Status Updates**: Regular status updates throughout the lead lifecycle
10. **Conversion Tracking**: Monitor conversion rates and optimize processes

---

## Customer Management Module

### Purpose
Complete customer lifecycle management with reorder tracking and potential customer identification. This module serves as the central repository for all customer information and relationship history, enabling personalized service and retention strategies.

### Core Features
- **CRUD Customer Operations**: Complete customer data management
- **Reorder Count Tracking**: Automatic tracking of customer repeat orders
- **Customer History Management**: Complete order and interaction history
- **Measurement Storage**: Secure storage of customer measurements
- **Preference Management**: Track style preferences and special requirements
- **Potential Customer Database**: Separate entity for prospects not yet converted
- **Customer Segmentation**: Categorize customers based on various criteria
- **Loyalty Program Integration**: Track and manage customer loyalty points
- **Communication History**: Complete log of all customer interactions
- **Customer Analytics**: Individual customer performance metrics

### Data Models

#### Customer Model
```
Customer {
  id: UUID (Primary Key)
  customer_code: String (Unique, Auto-generated)
  name: String (Required)
  phone: String (Required, Unique)
  email: String (Optional)
  address: Text
  city: String
  state: String
  pincode: String
  date_of_birth: Date (Optional)
  anniversary_date: Date (Optional)
  gender: Enum [Male, Female, Other]
  reorder_count: Integer (Default: 0, Auto-increment)
  total_spent: Decimal (Auto-calculated)
  average_order_value: Decimal (Auto-calculated)
  preferred_style: String
  fabric_preferences: JSON
  color_preferences: JSON
  measurements: JSON {
    chest: Decimal,
    waist: Decimal,
    hip: Decimal,
    shoulder: Decimal,
    sleeve_length: Decimal,
    shirt_length: Decimal,
    trouser_length: Decimal,
    custom_measurements: JSON
  }
  special_instructions: Text
  notes: Text
  status: Enum [Active, Inactive, VIP, Blacklisted]
  customer_since: DateTime (Auto-set on first order)
  last_order_date: DateTime (Optional)
  next_followup_date: DateTime (Optional)
  referral_source: String
  assigned_sales_person: Foreign Key -> Employee
  created_at: DateTime
  updated_at: DateTime
}
```

#### Potential Customer Model
```
PotentialCustomer {
  id: UUID (Primary Key)
  name: String (Required)
  phone: String (Required)
  email: String (Optional)
  source: String
  interest_level: Enum [High, Medium, Low]
  interest_category: String (Wedding, Party, Casual, Formal)
  preferred_contact_method: Enum [Phone, Email, WhatsApp, SMS]
  budget_indication: String
  event_date: Date (Optional)
  notes: Text
  last_contact_date: DateTime
  next_follow_up: DateTime
  contact_attempts: Integer (Default: 0)
  conversion_probability: Integer (1-100)
  assigned_employee: Foreign Key -> Employee
  status: Enum [New, Contacted, Interested, Not_Interested, Converted]
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Lead Conversion/New Customer → Customer Profile Creation → Measurement Recording → 
Order Processing → Reorder Tracking → Relationship Management → 
Retention Activities
```

**Detailed Process Steps:**
1. **Customer Creation**: Either from lead conversion or direct walk-in
2. **Profile Setup**: Complete customer information and preferences recording
3. **Measurement Session**: Detailed measurements taken and stored securely
4. **Order Association**: Link all orders to customer profile
5. **Reorder Tracking**: Automatic increment of reorder count with each new order
6. **Preference Learning**: System learns and updates customer preferences over time
7. **Relationship Management**: Regular follow-ups and relationship building activities
8. **Retention Strategies**: Targeted campaigns based on customer behavior
9. **Loyalty Rewards**: Points accumulation and redemption tracking

---

## Material Management Module

### Purpose
Comprehensive inventory management system for raw materials, fabrics, and accessories used in the boutique operations. This module ensures optimal stock levels, cost control, and material traceability throughout the production process.

### Core Features
- **Raw Material Inventory Tracking**: Complete inventory management for all materials
- **Cloth Type Categorization**: Systematic categorization of different fabric types
- **Supplier Management**: Maintain supplier database with contact and pricing information
- **Stock Level Monitoring**: Real-time stock tracking with automated alerts
- **Material Cost Tracking**: Track purchase costs and calculate material costs per order
- **Low Stock Alerts**: Automated notifications when stock falls below minimum levels
- **Purchase Order Management**: Create and track purchase orders to suppliers
- **Material Usage Tracking**: Track material consumption per order
- **Quality Control**: Track material quality ratings and supplier performance
- **Seasonal Planning**: Plan inventory based on seasonal demand patterns

### Data Model
```
Material {
  id: UUID (Primary Key)
  material_code: String (Unique, Auto-generated)
  name: String (Required)
  type: Enum [Fabric, Thread, Button, Zipper, Lining, Interfacing, Trim, Accessory]
  category: String (e.g., "Silk", "Cotton", "Polyester", "Wool", "Linen")
  sub_category: String (e.g., "Raw Silk", "Printed Cotton")
  color: String
  pattern: String (Solid, Printed, Striped, Checked)
  width: Decimal (in inches/cm)
  weight: Decimal (GSM for fabrics)
  composition: String (e.g., "100% Cotton", "65% Polyester 35% Cotton")
  quantity_in_stock: Decimal
  reserved_quantity: Decimal (Reserved for confirmed orders)
  available_quantity: Decimal (Calculated: stock - reserved)
  unit: Enum [Meters, Yards, Pieces, Kg, Grams]
  cost_per_unit: Decimal
  selling_price_per_unit: Decimal
  supplier_id: Foreign Key -> Supplier
  supplier_material_code: String
  minimum_stock_level: Decimal
  maximum_stock_level: Decimal
  reorder_point: Decimal
  reorder_quantity: Decimal
  location: String (Warehouse location)
  quality_grade: Enum [A, B, C]
  care_instructions: Text
  seasonal_category: Enum [Summer, Winter, Monsoon, All_Season]
  is_active: Boolean (Default: true)
  notes: Text
  last_purchased_date: Date
  last_purchased_rate: Decimal
  created_at: DateTime
  updated_at: DateTime
}

Supplier {
  id: UUID (Primary Key)
  name: String (Required)
  contact_person: String
  phone: String (Required)
  email: String
  address: Text
  city: String
  state: String
  gst_number: String
  payment_terms: String
  credit_limit: Decimal
  rating: Integer (1-5)
  is_active: Boolean (Default: true)
  notes: Text
  created_at: DateTime
  updated_at: DateTime
}

MaterialTransaction {
  id: UUID (Primary Key)
  material_id: Foreign Key -> Material
  transaction_type: Enum [Purchase, Usage, Adjustment, Return]
  quantity: Decimal
  rate: Decimal
  reference_id: String (Order ID, Purchase Order ID)
  reference_type: Enum [Sales_Order, Purchase_Order, Stock_Adjustment]
  transaction_date: DateTime
  notes: Text
  created_by: Foreign Key -> Employee
  created_at: DateTime
}
```

### Process Flow
```
Inventory Planning → Supplier Selection → Purchase Order Creation → 
Material Receipt → Quality Check → Stock Update → Usage Tracking → 
Reorder Process
```

**Detailed Process Steps:**
1. **Inventory Assessment**: Regular review of stock levels and demand patterns
2. **Reorder Identification**: System identifies materials below reorder point
3. **Supplier Evaluation**: Compare supplier prices, quality, and delivery times
4. **Purchase Order Creation**: Generate PO with specifications and delivery dates
5. **Order Tracking**: Monitor supplier delivery schedules and follow up
6. **Material Receipt**: Physical verification and quality check upon delivery
7. **Stock Update**: Update inventory levels and material costs
8. **Usage Allocation**: Reserve materials for confirmed orders
9. **Cost Calculation**: Calculate material costs for each order
10. **Performance Monitoring**: Track supplier performance and material quality

---

## Sales Management Module

### Purpose
Comprehensive sales order processing system with detailed tracking, analytics, and customer conversion capabilities. This module handles the complete sales process from quote generation to order completion with integrated analytics and performance monitoring.

### Core Features
- **Sales Order Creation and Management**: Complete order lifecycle management
- **Customer Conversion from Leads**: Seamless conversion of qualified leads
- **Measurement Recording**: Detailed customer measurements with history
- **Cloth Type Selection**: Choose between purchased materials or customer-owned fabric
- **Trial Date Scheduling**: Schedule and manage fitting appointments
- **Cost Calculation Engine**: Automated cost calculation based on materials and labor
- **Advance Payment Tracking**: Manage advance payments and balance calculations
- **Employee Assignment**: Assign orders to specific employees or teams
- **Sales Analytics Dashboard**: Comprehensive analytics and reporting
- **Quote Management**: Generate and manage quotations
- **Order Status Tracking**: Real-time order status updates
- **Revenue Analytics**: Daily, weekly, monthly, and yearly sales analytics

### Data Models

#### Sales Order Model
```
SalesOrder {
  id: UUID (Primary Key)
  order_number: String (Unique, Auto-generated)
  quote_number: String (Optional, if converted from quote)
  customer_id: Foreign Key -> Customer (Required)
  order_date: DateTime
  expected_delivery_date: DateTime
  actual_delivery_date: DateTime (Optional)
  service_type: Enum [New_Stitching, Alteration, Repair, Design_Consultation]
  garment_type: Enum [Shirt, Trouser, Suit, Dress, Blouse, Saree_Blouse, Lehenga, Other]
  style_details: Text
  occasion: String (Wedding, Party, Formal, Casual)
  urgency: Enum [Normal, Urgent, Express]
  cloth_source: Enum [Boutique_Material, Customer_Material, Combination]
  cloth_details: JSON {
    material_id: UUID (if boutique material),
    customer_material_description: String,
    quantity_required: Decimal,
    wastage_percentage: Decimal
  }
  measurements: JSON {
    chest: Decimal,
    waist: Decimal,
    hip: Decimal,
    shoulder: Decimal,
    sleeve_length: Decimal,
    shirt_length: Decimal,
    neck: Decimal,
    custom_measurements: JSON
  }
  special_instructions: Text
  trial_required: Boolean (Default: true)
  trial_date: DateTime (Optional)
  trial_completed: Boolean (Default: false)
  trial_notes: Text
  design_complexity: Enum [Simple, Medium, Complex, Highly_Complex]
  material_cost: Decimal
  labor_cost: Decimal
  design_cost: Decimal
  additional_charges: Decimal
  total_cost: Decimal (Calculated)
  discount_percentage: Decimal (Default: 0)
  discount_amount: Decimal (Calculated)
  final_amount: Decimal (Calculated)
  advance_amount: Decimal
  balance_amount: Decimal (Calculated)
  payment_status: Enum [Pending, Advance_Paid, Partial_Paid, Fully_Paid]
  assigned_designer: Foreign Key -> Employee (Optional)
  assigned_tailor: Foreign Key -> Employee (Optional)
  sales_person: Foreign Key -> Employee
  status: Enum [Draft, Confirmed, In_Progress, Trial_Pending, Ready_for_Delivery, Delivered, Cancelled]
  cancellation_reason: Text (Optional)
  customer_satisfaction: Integer (1-5, Post delivery)
  notes: Text
  created_at: DateTime
  updated_at: DateTime
}
```

#### Sales Analytics Model
```
SalesAnalytics {
  id: UUID (Primary Key)
  date: Date
  period_type: Enum [Daily, Weekly, Monthly, Yearly]
  total_orders: Integer
  orders_completed: Integer
  orders_in_progress: Integer
  orders_cancelled: Integer
  new_customers: Integer
  returning_customers: Integer
  total_revenue: Decimal
  material_revenue: Decimal
  service_revenue: Decimal
  average_order_value: Decimal
  total_cost: Decimal
  material_cost: Decimal
  labor_cost: Decimal
  profit: Decimal (Calculated)
  profit_margin: Decimal (Calculated as percentage)
  top_selling_service: String
  top_performing_employee: Foreign Key -> Employee
  customer_acquisition_cost: Decimal
  customer_retention_rate: Decimal
  order_fulfillment_time: Decimal (Average days)
  trial_success_rate: Decimal (Percentage)
  employee_performance: JSON {
    employee_id: UUID,
    orders_handled: Integer,
    revenue_generated: Decimal,
    customer_satisfaction: Decimal
  }
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Lead/Customer Consultation → Requirements Analysis → Quote Generation → 
Quote Approval → Order Creation → Payment Collection → Production Assignment → 
Order Tracking → Delivery → Final Payment → Analytics Update
```

**Detailed Process Steps:**
1. **Customer Consultation**: Detailed discussion of requirements and preferences
2. **Measurement Session**: Accurate measurements taken and recorded
3. **Material Selection**: Choose fabrics, colors, and accessories
4. **Design Discussion**: Style details, special features, and customizations
5. **Cost Calculation**: System calculates costs based on materials, labor, and complexity
6. **Quote Generation**: Professional quote with all details and terms
7. **Quote Approval**: Customer approval and any negotiations
8. **Order Confirmation**: Convert quote to confirmed order
9. **Advance Payment**: Collect advance payment as per policy
10. **Production Assignment**: Assign to designer and tailor based on skills and workload
11. **Order Tracking**: Monitor progress through production pipeline
12. **Trial Scheduling**: Schedule fitting appointment when garment is ready
13. **Final Adjustments**: Make any necessary alterations after trial
14. **Quality Check**: Final quality inspection before delivery
15. **Delivery**: Hand over completed order to customer
16. **Final Payment**: Collect balance payment
17. **Feedback Collection**: Customer satisfaction survey
18. **Analytics Update**: Update sales metrics and performance indicators

---

## Order Management Module

### Purpose
Visual pipeline-style order tracking system with drag-and-drop functionality for managing orders through different production stages. This module provides real-time visibility into the production process and enables efficient workflow management with automated notifications.

### Core Features
- **Kanban-Style Order Pipeline**: Visual drag-and-drop interface for order management
- **Multi-Stage Workflow**: 10 distinct production stages from design to delivery
- **Real-Time Status Updates**: Instant status updates when orders move between stages
- **Employee Notifications**: Automatic notifications to assigned employees and administrators
- **Stage-Wise Progress Tracking**: Detailed tracking of time spent in each stage
- **Priority Management**: Set and adjust order priorities (Normal, High, Urgent)
- **Bottleneck Identification**: Identify stages where orders are getting delayed
- **Capacity Planning**: View workload distribution across employees and stages
- **Automated Escalations**: Automatic escalations for orders exceeding stage time limits
- **Production Analytics**: Stage-wise performance metrics and efficiency analysis

### Data Models

#### Order Pipeline Model
```
OrderPipeline {
  id: UUID (Primary Key)
  sales_order_id: Foreign Key -> SalesOrder (Required)
  current_stage: Enum [
    Sketch_Design,
    Sketch_Approval, 
    Material_Sourcing, 
    Dyeing_Process, 
    Pattern_Creation,
    Paper_Cutting, 
    Handwork_Embellishment, 
    Fabric_Cutting, 
    Stitching_Tailoring, 
    Trial_Fitting,
    Final_Finishing,
    Quality_Check,
    Ready_For_Delivery, 
    Delivered,
    Final_Payment_Collected
  ]
  priority: Enum [Low, Normal, High, Urgent, Rush]
  estimated_completion_date: DateTime
  actual_completion_date: DateTime (Optional)
  total_estimated_hours: Decimal
  total_actual_hours: Decimal (Optional)
  is_delayed: Boolean (Calculated based on current date vs estimated dates)
  delay_reason: Text (Optional)
  customer_trial_required: Boolean (Default: true)
  trial_scheduled_date: DateTime (Optional)
  trial_completed_date: DateTime (Optional)
  rework_required: Boolean (Default: false)
  rework_stage: String (Optional)
  rework_reason: Text (Optional)
  quality_issues: Text (Optional)
  stage_history: JSON [
    {
      stage: String,
      started_at: DateTime,
      completed_at: DateTime,
      assigned_employee_id: UUID,
      estimated_hours: Decimal,
      actual_hours: Decimal,
      notes: Text,
      quality_rating: Integer (1-5),
      moved_by_employee_id: UUID
    }
  ]
  current_stage_assigned_to: Foreign Key -> Employee (Optional)
  current_stage_started_at: DateTime (Optional)
  current_stage_estimated_completion: DateTime (Optional)
  notes: Text
  created_at: DateTime
  updated_at: DateTime
}
```

#### Stage Configuration Model
```
StageConfiguration {
  id: UUID (Primary Key)
  stage_name: String (Required)
  stage_order: Integer
  estimated_duration_hours: Decimal
  max_duration_hours: Decimal (For escalation)
  required_skills: JSON [String]
  is_customer_facing: Boolean
  requires_approval: Boolean
  can_work_parallel: Boolean
  dependencies: JSON [String] (Other stages that must be completed first)
  escalation_enabled: Boolean
  escalation_threshold_hours: Decimal
  escalation_recipients: JSON [UUID] (Employee IDs)
  is_active: Boolean (Default: true)
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Order Confirmation → Pipeline Entry → Stage Assignment → 
Work Execution → Stage Completion → Next Stage Assignment → 
Progress Monitoring → Quality Checks → Final Delivery
```

**Detailed Stage Process:**

1. **Sketch & Design Stage**
   - Designer creates initial concept and technical drawings
   - Customer consultation for design approval
   - Design modifications if required
   - Final design approval and documentation

2. **Sketch Approval & Upload**
   - Customer reviews and approves final design
   - Digital design files uploaded to system
   - Technical specifications documented
   - Material requirements finalized

3. **Material Sourcing**
   - Required materials identified from design specs
   - Inventory checked for material availability
   - Purchase orders created for unavailable materials
   - Materials reserved for the order

4. **Dyeing Process** (If Required)
   - Color matching for specific requirements
   - Fabric dyeing process execution
   - Quality control for color accuracy
   - Drying and preparation for next stage

5. **Pattern Creation & Paper Cutting**
   - Create paper patterns based on measurements
   - Pattern verification and adjustments
   - Test fitting with paper patterns if needed
   - Pattern finalization and documentation

6. **Handwork & Embellishment**
   - Embroidery, beadwork, or decorative elements
   - Hand-stitched details and special features
   - Quality checks for handwork precision
   - Preparation for main construction

7. **Fabric Cutting**
   - Fabric layout and marking based on patterns
   - Precision cutting of all garment pieces
   - Piece labeling and organization
   - Cutting verification against patterns

8. **Stitching & Tailoring**
   - Main garment construction and assembly
   - Progressive fitting checks during construction
   - Detail work and finishing touches
   - Internal construction completion

9. **Trial Fitting**
   - Customer appointment for initial fitting
   - Adjustments identification and marking
   - Customer feedback and modification requests
   - Scheduling for final alterations

10. **Final Finishing**
    - Implementation of trial fitting adjustments
    - Final pressing and finishing touches
    - Hardware attachment (buttons, zippers)
    - Final quality inspection

11. **Quality Check & Packaging**
    - Comprehensive quality inspection
    - Final measurements verification
    - Professional packaging and presentation
    - Delivery preparation

12. **Ready for Delivery**
    - Customer notification for pickup/delivery
    - Delivery scheduling and coordination
    - Final payment processing
    - Order completion documentation

### Pipeline Management Process
```
Daily Pipeline Review → Workload Distribution → Priority Adjustments → 
Employee Assignments → Progress Monitoring → Bottleneck Resolution → 
Performance Analysis
```

**Management Activities:**
1. **Daily Standup**: Review pipeline status and identify issues
2. **Capacity Planning**: Distribute workload based on employee skills and availability
3. **Priority Management**: Adjust priorities based on customer requirements and deadlines
4. **Bottleneck Resolution**: Identify and resolve workflow bottlenecks
5. **Quality Monitoring**: Track quality metrics at each stage
6. **Performance Analytics**: Analyze stage-wise performance and efficiency
7. **Customer Communication**: Proactive updates to customers on order progress

---

## Employee Management Module

### Purpose
Comprehensive employee configuration and management system with role-based access control, permission management, and performance tracking. This module manages the complete employee lifecycle and ensures proper access controls across all system modules.

### Core Features
- **CRUD Employee Operations**: Complete employee data management
- **Role-Based Access Control**: Define roles with specific permissions
- **Permission Management**: Granular control over module and feature access
- **Task Assignment Tracking**: Monitor tasks assigned to each employee
- **Performance Monitoring**: Track employee productivity and performance metrics
- **Attendance Management**: Basic attendance tracking and reporting
- **Skill Matrix Management**: Track employee skills and specializations
- **Department Organization**: Organize employees by departments and teams
- **Employee Onboarding**: Structured onboarding process for new employees
- **Performance Reviews**: Periodic performance evaluation system

### Data Models

#### Employee Model
```
Employee {
  id: UUID (Primary Key)
  employee_id: String (Unique, Auto-generated)
  name: String (Required)
  phone: String (Required)
  email: String (Optional)
  emergency_contact: String
  address: Text
  date_of_birth: Date
  hire_date: Date (Required)
  probation_end_date: Date (Optional)
  employment_type: Enum [Full_Time, Part_Time, Contract, Intern]
  department: Enum [Sales, Design, Production, Administration, Management]
  designation: String
  role_id: Foreign Key -> Role (Required)
  reporting_manager: Foreign Key -> Employee (Optional)
  salary: Decimal (Optional, Encrypted)
  salary_type: Enum [Monthly, Daily, Hourly, Piece_Rate]
  bank_account: String (Optional, Encrypted)
  pan_number: String (Optional, Encrypted)
  aadhar_number: String (Optional, Encrypted)
  skills: JSON [
    {
      skill_name: String,
      proficiency_level: Enum [Beginner, Intermediate, Advanced, Expert],
      years_of_experience: Decimal
    }
  ]
  specializations: JSON [String] (e.g., "Wedding Dresses", "Formal Suits")
  languages_known: JSON [String]
  shift_timings: JSON {
    start_time: Time,
    end_time: Time,
    break_duration: Integer (minutes)
  }
  working_days: JSON [String] (Days of week)
  max_concurrent_orders: Integer (Default: 5)
  current_workload: Integer (Calculated)
  performance_rating: Decimal (1-5, Updated quarterly)
  is_active: Boolean (Default: true)
  termination_date: Date (Optional)
  termination_reason: Text (Optional)
  notes: Text
  profile_image_url: String (Optional)
  documents: JSON [
    {
      document_type: String,
      document_url: String,
      uploaded_date: DateTime
    }
  ]
  created_at: DateTime
  updated_at: DateTime
}
```

#### Role Model
```
Role {
  id: UUID (Primary Key)
  role_name: String (Unique, Required)
  role_description: Text
  is_admin: Boolean (Default: false)
  permissions: JSON {
    appointments: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      assign_to_others: Boolean
    },
    leads: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      view_all: Boolean,
      assign_to_others: Boolean
    },
    customers: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      view_sensitive_data: Boolean
    },
    materials: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      view_costs: Boolean,
      create_purchase_orders: Boolean
    },
    sales_orders: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      approve_discounts: Boolean,
      view_all_orders: Boolean,
      modify_costs: Boolean
    },
    order_pipeline: {
      view: Boolean,
      move_orders: Boolean,
      assign_tasks: Boolean,
      update_status: Boolean,
      view_all_stages: Boolean
    },
    employees: {
      create: Boolean,
      read: Boolean,
      update: Boolean,
      delete: Boolean,
      manage_permissions: Boolean,
      view_salary_info: Boolean
    },
    accounting: {
      view_reports: Boolean,
      record_expenses: Boolean,
      process_payments: Boolean,
      view_profit_loss: Boolean
    },
    analytics: {
      view_sales_analytics: Boolean,
      view_employee_performance: Boolean,
      view_customer_analytics: Boolean,
      view_financial_reports: Boolean,
      export_reports: Boolean
    }
  }
  is_active: Boolean (Default: true)
  created_at: DateTime
  updated_at: DateTime
}
```

#### Employee Performance Model
```
EmployeePerformance {
  id: UUID (Primary Key)
  employee_id: Foreign Key -> Employee (Required)
  evaluation_period: String (e.g., "Q1 2024", "Annual 2024")
  evaluation_date: Date
  evaluator_id: Foreign Key -> Employee (Manager/Admin)
  orders_completed: Integer
  orders_assigned: Integer
  completion_rate: Decimal (Calculated percentage)
  average_completion_time: Decimal (Days)
  quality_rating: Decimal (1-5, Average from order feedback)
  customer_satisfaction: Decimal (1-5, Average from customer ratings)
  punctuality_score: Decimal (1-5)
  teamwork_score: Decimal (1-5)
  skill_improvement: Text
  achievements: Text
  areas_for_improvement: Text
  training_recommendations: Text
  salary_revision: Decimal (Optional)
  promotion_recommended: Boolean (Default: false)
  overall_rating: Decimal (1-5, Calculated)
  manager_comments: Text
  employee_self_assessment: Text
  goals_next_period: Text
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Employee Recruitment → Onboarding → Role Assignment → Training → 
Task Assignment → Performance Monitoring → Evaluation → 
Development Planning
```

**Detailed Process Steps:**
1. **Recruitment Process**: Identify skill requirements and hire suitable candidates
2. **Employee Onboarding**: Create employee profile with all necessary information
3. **Role Assignment**: Assign appropriate role based on skills and department
4. **Permission Setup**: Configure system permissions based on role requirements
5. **Initial Training**: Provide system training and skill development
6. **Task Assignment**: Begin assigning tasks based on expertise and capacity
7. **Performance Monitoring**: Continuous tracking of productivity and quality
8. **Regular Evaluation**: Periodic performance reviews and feedback sessions
9. **Skill Development**: Identify training needs and provide development opportunities
10. **Career Progression**: Plan promotions and role changes based on performance

---

## Simple Accounting Module

### Purpose
Basic financial tracking and reporting system to monitor business revenue, expenses, and profitability. This module provides essential accounting features without the complexity of full-scale accounting software, focusing on boutique-specific financial needs.

### Core Features
- **Revenue Tracking**: Track all income from sales orders and services
- **Expense Recording**: Record business expenses by category
- **Payment Status Monitoring**: Track advance payments, pending payments, and collections
- **Cash Flow Management**: Monitor daily cash inflows and outflows
- **Basic Financial Reports**: Generate essential financial statements
- **Tax Calculation**: Basic tax calculations for business compliance
- **Vendor Payment Tracking**: Track payments to material suppliers
- **Employee Salary Management**: Basic payroll tracking
- **Profit/Loss Calculation**: Real-time profitability analysis
- **Financial Dashboard**: Quick overview of financial health

### Data Models

#### Revenue Record Model
```
RevenueRecord {
  id: UUID (Primary Key)
  transaction_date: Date (Required)
  revenue_type: Enum [Sales_Order, Alteration_Service, Consultation_Fee, Material_Sale, Other]
  reference_id: String (Order Number, Invoice Number)
  reference_type: Enum [Sales_Order, Direct_Sale, Service]
  customer_id: Foreign Key -> Customer (Optional)
  description: Text
  base_amount: Decimal
  tax_percentage: Decimal
  tax_amount: Decimal (Calculated)
  discount_amount: Decimal (Default: 0)
  total_amount: Decimal (Calculated)
  payment_method: Enum [Cash, Card, UPI, Bank_Transfer, Cheque]
  payment_status: Enum [Pending, Partial, Paid, Refunded]
  received_amount: Decimal
  pending_amount: Decimal (Calculated)
  received_date: Date (Optional)
  created_by: Foreign Key -> Employee
  notes: Text
  created_at: DateTime
  updated_at: DateTime
}
```

#### Expense Record Model
```
ExpenseRecord {
  id: UUID (Primary Key)
  transaction_date: Date (Required)
  expense_category: Enum [
    Material_Purchase,
    Employee_Salary,
    Rent,
    Utilities,
    Marketing,
    Transportation,
    Equipment,
    Maintenance,
    Insurance,
    Professional_Fees,
    Other
  ]
  subcategory: String (Optional)
  vendor_supplier: String (Optional)
  description: Text (Required)
  base_amount: Decimal
  tax_percentage: Decimal
  tax_amount: Decimal (Calculated)
  total_amount: Decimal (Calculated)
  payment_method: Enum [Cash, Card, Bank_Transfer, Cheque]
  payment_status: Enum [Pending, Paid]
  paid_date: Date (Optional)
  reference_number: String (Invoice/Bill Number)
  is_recurring: Boolean (Default: false)
  recurring_frequency: Enum [Monthly, Quarterly, Annually] (Optional)
  next_due_date: Date (Optional)
  created_by: Foreign Key -> Employee
  approved_by: Foreign Key -> Employee (Optional)
  notes: Text
  receipt_url: String (Optional)
  created_at: DateTime
  updated_at: DateTime
}
```

#### Financial Summary Model
```
FinancialSummary {
  id: UUID (Primary Key)
  summary_date: Date
  period_type: Enum [Daily, Weekly, Monthly, Quarterly, Yearly]
  total_revenue: Decimal
  total_expenses: Decimal
  gross_profit: Decimal (Calculated: revenue - direct_costs)
  net_profit: Decimal (Calculated: revenue - total_expenses)
  profit_margin: Decimal (Calculated as percentage)
  cash_inflow: Decimal
  cash_outflow: Decimal
  net_cash_flow: Decimal (Calculated)
  outstanding_receivables: Decimal
  outstanding_payables: Decimal
  tax_liability: Decimal
  employee_cost: Decimal
  material_cost: Decimal
  operational_cost: Decimal
  marketing_cost: Decimal
  order_count: Integer
  average_order_value: Decimal
  cost_per_order: Decimal (Calculated)
  created_at: DateTime
  updated_at: DateTime
}
```

#### Payment Tracking Model
```
PaymentTracking {
  id: UUID (Primary Key)
  payment_type: Enum [Customer_Payment, Supplier_Payment, Salary_Payment, Expense_Payment]
  reference_id: String (Order ID, Employee ID, Supplier ID)
  reference_type: Enum [Sales_Order, Purchase_Order, Salary, Expense]
  amount: Decimal
  payment_date: Date
  payment_method: Enum [Cash, Card, UPI, Bank_Transfer, Cheque]
  transaction_id: String (Optional)
  payment_status: Enum [Pending, Completed, Failed, Cancelled]
  received_by: Foreign Key -> Employee (For customer payments)
  paid_to: String (For supplier/expense payments)
  bank_account: String (Optional)
  cheque_number: String (Optional)
  cheque_date: Date (Optional)
  notes: Text
  receipt_generated: Boolean (Default: false)
  created_by: Foreign Key -> Employee
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Transaction Recording → Categorization → Payment Tracking → 
Tax Calculation → Report Generation → Financial Analysis → 
Decision Making
```

**Detailed Process Steps:**
1. **Transaction Recording**: Record all revenue and expense transactions
2. **Automatic Integration**: Sales orders automatically create revenue records
3. **Expense Categorization**: Categorize expenses for proper reporting
4. **Payment Reconciliation**: Match payments with invoices and orders
5. **Tax Calculation**: Calculate applicable taxes on transactions
6. **Report Generation**: Generate daily, monthly, and yearly financial reports
7. **Cash Flow Monitoring**: Track cash inflows and outflows
8. **Profitability Analysis**: Analyze profit margins and cost structures
9. **Outstanding Tracking**: Monitor pending payments and collections
10. **Financial Planning**: Use reports for business planning and decision making

**Key Reports Generated:**
- Daily Sales Report
- Monthly Revenue Summary
- Expense Analysis by Category
- Profit & Loss Statement
- Cash Flow Statement
- Outstanding Receivables Report
- Tax Liability Report
- Employee Cost Analysis
- Material Cost Analysis
- Business Performance Dashboard

---

## User Roles & Permissions

### Admin Role
**Purpose**: Complete system administration and oversight

**Permissions**:
- **Full System Access**: All modules and operations without restrictions
- **User Management**: Create, modify, and deactivate employee accounts
- **System Configuration**: Modify system settings, workflows, and configurations
- **Analytics Access**: All reports, analytics, and business intelligence
- **Financial Data**: Complete accounting access including sensitive financial information
- **Data Export**: Export all data types for backup and analysis
- **Audit Trail**: Access to complete system audit logs
- **Role Management**: Create and modify roles and permissions

### Manager Role
**Purpose**: Operational oversight and team management

**Permissions**:
- **Operational Oversight**: All customer and order operations
- **Employee Management**: Assign tasks, monitor performance, conduct evaluations
- **Sales Analytics**: Access to comprehensive sales and performance reports
- **Customer Management**: Full customer data access and relationship management
- **Order Pipeline**: Complete pipeline visibility and management
- **Financial Reports**: View-only access to financial reports and profitability
- **Inventory Management**: Monitor stock levels and approve purchase orders
- **Quality Control**: Monitor quality metrics and customer satisfaction

### Sales Employee Role
**Purpose**: Customer acquisition and order management

**Permissions**:
- **Lead Management**: CRUD operations on assigned leads and prospects
- **Customer Management**: CRUD operations on customers with data privacy restrictions
- **Appointment Scheduling**: Manage appointments and customer interactions
- **Order Creation**: Create and modify sales orders with approval limits
- **Quote Generation**: Generate quotes within authorized discount limits
- **Pipeline Visibility**: View orders assigned to them or their team
- **Performance Metrics**: Access to personal performance dashboards
- **Customer Communication**: Send notifications and updates to customers

### Designer Role
**Purpose**: Creative design and technical specification development

**Permissions**:
- **Design Pipeline**: Access to sketch design and approval stages
- **Customer Consultation**: View customer requirements and measurements
- **Material Selection**: View material inventory and make recommendations
- **Design Documentation**: Upload designs, specifications, and technical drawings
- **Order Details**: View assigned order specifications and customer preferences
- **Design History**: Access to design archives and previous work
- **Creative Tools**: Access to design tools and resources
- **Client Interaction**: Participate in design consultations and approvals

### Production Employee (Tailor) Role
**Purpose**: Garment production and quality execution

**Permissions**:
- **Production Pipeline**: Access to assigned production stages
- **Order Specifications**: View detailed order specifications and measurements
- **Status Updates**: Update production stage status and progress
- **Material Usage**: Record material consumption and wastage
- **Quality Reporting**: Report quality issues and production challenges
- **Time Tracking**: Log time spent on different orders and stages
- **Work Instructions**: Access to detailed work instructions and guidelines
- **Equipment Management**: Report equipment issues and maintenance needs

### Accountant Role (If Applicable)
**Purpose**: Financial management and reporting

**Permissions**:
- **Financial Records**: Full access to accounting module
- **Revenue Management**: Record and track all revenue transactions
- **Expense Management**: Record and categorize business expenses
- **Payment Processing**: Process customer payments and supplier payments
- **Financial Reporting**: Generate all financial reports and statements
- **Tax Management**: Calculate and track tax liabilities
- **Bank Reconciliation**: Reconcile bank statements with system records
- **Audit Support**: Provide audit trail and documentation

---


## Technical Requirements (Summary)

- **Database**: PostgreSQL 13+ (preferred) or MySQL 8+, scalable to 100GB+, daily/weekly backups, 7-year retention for key data.
- **Integrations**: REST API for website, Meta/Google Ads, WhatsApp, SMS, Email (SMTP), Payment Gateway, Cloud Storage (AWS S3/Google Cloud).
- **Security**: Multi-factor authentication, RBAC, encryption (data in transit & at rest), GDPR compliance, audit logging, regular security reviews.
- **Performance**: Fast response (<1s API, <3s page load), supports 100+ users, 100k+ orders, scalable via load balancing and CDN.
- **Infrastructure**: Linux server, 8+ CPU cores, 16GB+ RAM, SSD storage, Nginx, Node.js/Django backend, monitoring & alerts, redundant backups.

### Technology Stack Options

Two options:
1. **Next.js + Django + PostgreSQL**: Full control, advanced features, scalable, requires more setup/maintenance.
2. **Next.js + Appwrite BaaS**: Rapid development, managed backend, less customization, vendor lock-in risk.

Choose based on business complexity, timeline, and technical resources.

#### Option 1: Next.js + Django + PostgreSQL (Traditional Full-Stack)

**Architecture**:
- **Frontend**: Next.js 14+ with TypeScript
- **Backend**: Django 4+ with Django REST Framework (DRF)
- **Database**: PostgreSQL 15+
- **Deployment**: Self-hosted or cloud servers (AWS, DigitalOcean, etc.)

**Pros**:
- ✅ **Complete Control**: Full control over backend logic, database schema, and business rules
- ✅ **Customization**: Unlimited customization capabilities for complex business requirements
- ✅ **Scalability**: Can handle high-volume operations and complex integrations
- ✅ **Data Ownership**: Complete control over data storage and privacy
- ✅ **No Vendor Lock-in**: Can migrate or change hosting providers easily
- ✅ **Advanced Features**: Support for complex analytics, reporting, and custom workflows
- ✅ **Integration Flexibility**: Easy integration with any third-party service or API
- ✅ **Cost Predictable**: Predictable hosting costs that scale with actual usage
- ✅ **Performance**: Optimized database queries and custom caching strategies
- ✅ **Security**: Implement custom security measures and compliance requirements

**Cons**:
- ❌ **Development Time**: Longer initial development time (8-12 weeks)
- ❌ **Infrastructure Management**: Requires server management and maintenance
- ❌ **Technical Expertise**: Need experienced Django developers for maintenance
- ❌ **DevOps Requirements**: Need to set up CI/CD, monitoring, and backup systems
- ❌ **Initial Setup Complexity**: More complex initial deployment and configuration

**Best For**:
- Boutiques with complex business processes
- Plans for significant scaling (multiple locations)
- Need for advanced analytics and reporting
- Custom integrations with existing systems
- Long-term growth and feature expansion plans

**Estimated Timeline**: 10-14 weeks
**Estimated Cost**: Higher initial investment, lower long-term operational costs

---

#### Option 2: Next.js + Appwrite BaaS (Backend-as-a-Service)

**Architecture**:
- **Frontend**: Next.js 14+ with TypeScript
- **Backend**: Appwrite Cloud (Backend-as-a-Service)
- **Database**: Built-in Appwrite database (based on MariaDB)
- **Deployment**: Frontend on Vercel, Backend managed by Appwrite

**Pros**:
- ✅ **Rapid Development**: Faster development and deployment (4-6 weeks)
- ✅ **Built-in Features**: Authentication, file storage, real-time updates out-of-the-box
- ✅ **No Infrastructure Management**: Appwrite handles all backend infrastructure
- ✅ **Automatic Scaling**: Automatic scaling based on usage
- ✅ **Built-in Security**: Robust security features and compliance built-in
- ✅ **Real-time Capabilities**: Built-in real-time subscriptions for live updates
- ✅ **Easy Deployment**: Simple deployment and hosting setup
- ✅ **Backup & Recovery**: Automated backups and disaster recovery
- ✅ **Multi-platform**: Easy mobile app development in the future
- ✅ **Quick Prototyping**: Faster time-to-market for MVP

**Cons**:
- ❌ **Vendor Lock-in**: Dependent on Appwrite's ecosystem and pricing
- ❌ **Limited Customization**: Some limitations in complex business logic implementation
- ❌ **Query Limitations**: Less flexible database querying compared to raw SQL
- ❌ **Cost Scaling**: Costs can increase significantly with high usage
- ❌ **Feature Dependencies**: Limited by Appwrite's feature set and roadmap
- ❌ **Data Migration**: More complex to migrate away from Appwrite if needed
- ❌ **Advanced Analytics**: May require additional tools for complex reporting
- ❌ **Integration Constraints**: Some third-party integrations may be more challenging

**Best For**:
- Small to medium boutiques
- Quick market entry requirements
- Limited technical team for maintenance
- Standard CRM workflows without heavy customization
- Budget-conscious projects with predictable scaling

**Estimated Timeline**: 6-8 weeks
**Estimated Cost**: Lower initial investment, potentially higher long-term operational costs

---

### Technology Stack Comparison Matrix

| Feature | Option 1 (Django) | Option 2 (Appwrite) |
|---------|-------------------|---------------------|
| **Development Speed** | Slower (10-14 weeks) | Faster (6-8 weeks) |
| **Initial Cost** | Higher | Lower |
| **Long-term Cost** | Lower | Potentially Higher |
| **Customization** | Unlimited | Limited |
| **Scalability** | High | Medium-High |
| **Maintenance** | Requires Team | Minimal |
| **Data Control** | Complete | Shared |
| **Third-party Integration** | Unlimited | Good |
| **Performance** | Optimized | Good |
| **Security** | Custom | Built-in |
| **Mobile App Future** | Moderate Effort | Easy |
| **Offline Capabilities** | Custom Implementation | Built-in |
| **Real-time Features** | Custom Implementation | Built-in |
| **Complex Reporting** | Unlimited | Limited |

---

### Recommended Decision Framework

**Choose Option 1 (Django) if you**:
- Have complex, unique business processes
- Plan to scale to multiple locations
- Need advanced analytics and custom reporting
- Have budget for longer development timeline
- Want complete control over your data and infrastructure
- Plan significant feature expansions in the future
- Have or can hire technical team for maintenance

**Choose Option 2 (Appwrite) if you**:
- Need to launch quickly (within 2 months)
- Have standard boutique CRM requirements
- Prefer lower initial investment
- Want minimal technical maintenance overhead
- Plan to add mobile apps in the future
- Need real-time features like live order tracking
- Have a smaller technical team or budget

---

### Development & Deployment (Both Options)

**Common Elements**:
- **Version Control**: Git with feature branch workflow
- **Frontend Deployment**: Vercel (recommended) or Netlify
- **CI/CD Pipeline**: Automated testing and deployment
- **Testing**: Unit tests, integration tests, and user acceptance testing
- **Monitoring**: Application performance monitoring and error tracking
- **Documentation**: API documentation, user manuals, and technical documentation

**Option 1 Specific**:
- **Backend Deployment**: AWS, DigitalOcean, or similar cloud providers
- **Database Management**: PostgreSQL with automated backups
- **Server Monitoring**: Custom monitoring and alerting setup
- **SSL Management**: Custom SSL certificate management

**Option 2 Specific**:
- **Backend Management**: Fully managed by Appwrite
- **Database**: Automatically managed with built-in backups
- **Monitoring**: Built-in monitoring and analytics
- **SSL**: Automatically managed

Both options will deliver a fully functional Boutique CRM system. The choice depends on your specific business requirements, technical preferences, budget considerations, and long-term growth plans. We can provide detailed cost estimates and implementation timelines for either option upon request.

This comprehensive documentation provides a complete foundation for developing the Boutique CRM system with all modules structured as requested, including purpose, core features, data models, and detailed process flows for each module.