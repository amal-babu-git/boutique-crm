# Boutique CRM System Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [Appointment Management Module](#appointment-management-module)
3. [Lead Management Module](#lead-management-module)
4. [Customer Management Module](#customer-management-module)
5. [Material Management Module](#material-management-module)
6. [Sales Management Module](#sales-management-module)
7. [Service Management Module](#service-management-module)
8. [Order Management Module](#order-management-module)
9. [Employee Management Module](#employee-management-module)
10. [Simple Accounting Module](#simple-accounting-module)
11. [Reports Module](#reports-module)
12. [User Roles & Permissions](#user-roles--permissions)

## System Overview

The Boutique CRM is a comprehensive customer relationship and order management system designed specifically for fashion boutiques and custom tailoring businesses. The system manages the entire customer journey from lead generation through final delivery, with integrated appointment scheduling, material management, and production pipeline tracking.

### Key Features
- **Lead-to-Customer Conversion Pipeline**
- **Appointment Management from Website Integration**
- **Service Catalog Management with Pricing**
- **Custom Order Processing with Visual Pipeline**
- **Material and Inventory Management**
- **Employee Task Assignment and Tracking**
- **Invoice Generation and Payment Tracking**
- **Comprehensive Reporting and Analytics**
- **Basic Accounting Integration**

---

## Appointment Management Module

### Purpose
Basic appointment management system for viewing and managing appointment details collected from the client web landing page. This module provides essential CRUD (Create, Read, Update, Delete) operations for appointment data with no additional features.

### Core Features
- **Appointment CRUD Operations**: Complete Create, Read, Update, Delete functionality for appointments
- **Website Form Integration**: View appointments collected from the client web landing page
- **Basic Appointment Management**: Simple interface to manage appointment details

### Data Model
```
Appointment {
  id: UUID (Primary Key)
  customer_name: String (Required)
  phone: String (Required)
  email: String (Optional)
  appointment_date: DateTime (Required)
  appointment_time: Time (Required)
  service_type: String (Optional)
  message: Text (Optional)
  status: Enum [New, Contacted, confirmed, unqualified, lost]
  source: String (Default: "Website Landing Page")
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Website Landing Page Form Submission → Appointment Creation → 
Basic CRUD Operations (View/Edit/Delete) → Status Updates
```

### Details
This module provides basic appointment management functionality for appointments submitted through the client web landing page. Staff can:

1. **Create**: Add new appointments manually if needed
2. **Read**: View all appointment details and information
3. **Update**: Edit appointment details, update status, and add notes
4. **Delete**: Remove appointments that are no longer relevant

The module focuses solely on data management without advanced features like scheduling conflicts, notifications, or employee assignments. It serves as a simple database interface for appointment information collected from the website.

---

## Lead Management Module

### Purpose
Comprehensive lead management system with automatic Meta integration and manual lead entry capabilities. This module handles the complete lead lifecycle from capture to conversion, ensuring no potential customer is lost in the process.

### Core Features
- **CRUD Operations**: Complete Create, Read, Update, Delete functionality for leads
- **Meta Ads Integration**: Automatic lead capture from Facebook/Instagram advertising campaigns
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
  custom_note: String 
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
- **Invoice Generation**: Automated invoice creation and management
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
  invoice_number: String (Unique, Auto-generated)
  invoice_date: DateTime (Auto-set when invoice generated)
  invoice_status: Enum [Draft, Sent, Paid, Overdue, Cancelled]
  invoice_due_date: DateTime (Optional)
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
Quote Approval → Order Creation → Invoice Generation → Payment Collection → 
Production Assignment → Order Tracking → Delivery → Final Payment → Analytics Update
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
9. **Invoice Generation**: Create professional invoice with all details and payment terms
10. **Advance Payment**: Collect advance payment as per policy
11. **Production Assignment**: Assign to designer and tailor based on skills and workload
12. **Order Tracking**: Monitor progress through production pipeline
13. **Trial Scheduling**: Schedule fitting appointment when garment is ready
14. **Final Adjustments**: Make any necessary alterations after trial
15. **Quality Check**: Final quality inspection before delivery
16. **Delivery**: Hand over completed order to customer
17. **Final Payment**: Collect balance payment and update invoice status
18. **Feedback Collection**: Customer satisfaction survey
19. **Analytics Update**: Update sales metrics and performance indicators

---

## Service Management Module

### Purpose
Comprehensive service catalog management system for defining, pricing, and managing all boutique services. This module maintains a centralized repository of services offered, enabling consistent pricing, service descriptions, and performance tracking across the business.

### Core Features
- **Service CRUD Operations**: Complete Create, Read, Update, Delete functionality for services
- **Service Categorization**: Organize services by type, complexity, and department
- **Pricing Management**: Flexible pricing structures with cost and profit margin tracking
- **Service Templates**: Pre-defined service templates for common offerings
- **Duration Estimation**: Standard time estimates for service completion
- **Skill Requirements**: Define required skills and expertise for each service
- **Service Analytics**: Track service popularity, profitability, and performance
- **Seasonal Pricing**: Support for seasonal and promotional pricing
- **Service Bundling**: Create service packages and combination offerings
- **Quality Standards**: Define quality benchmarks and completion criteria

### Data Models

#### Service Model
```
Service {
  id: UUID (Primary Key)
  service_code: String (Unique, Auto-generated)
  name: String (Required)
  description: Text
  category: Enum [Stitching, Alteration, Design, Consultation, Repair, Embellishment]
  subcategory: String (e.g., "Wedding Dress", "Formal Suit", "Casual Wear")
  service_type: Enum [New_Creation, Alteration, Repair, Consultation, Design_Only]
  complexity_level: Enum [Simple, Medium, Complex, Highly_Complex]
  base_price: Decimal
  cost_price: Decimal (Internal cost calculation)
  profit_margin: Decimal (Calculated as percentage)
  pricing_type: Enum [Fixed, Per_Hour, Per_Piece, Custom_Quote]
  estimated_hours: Decimal
  min_duration_days: Integer
  max_duration_days: Integer
  required_skills: JSON [String] (e.g., ["Tailoring", "Embroidery", "Design"])
  required_materials: JSON [String] (Common materials used)
  department: Enum [Design, Production, Alteration, Consultation]
  is_active: Boolean (Default: true)
  is_seasonal: Boolean (Default: false)
  seasonal_markup: Decimal (Optional)
  quality_standards: Text
  completion_criteria: Text
  customer_instructions: Text
  internal_notes: Text
  popularity_score: Integer (1-10, Auto-calculated)
  average_rating: Decimal (Customer satisfaction rating)
  total_orders: Integer (Auto-calculated)
  revenue_generated: Decimal (Auto-calculated)
  created_at: DateTime
  updated_at: DateTime
}
```

#### Service Bundle Model
```
ServiceBundle {
  id: UUID (Primary Key)
  bundle_name: String (Required)
  description: Text
  bundle_services: JSON [
    {
      service_id: UUID,
      quantity: Integer,
      discount_percentage: Decimal
    }
  ]
  bundle_price: Decimal
  individual_total: Decimal (Sum of individual service prices)
  bundle_discount: Decimal (Calculated)
  estimated_total_hours: Decimal (Sum of service hours)
  estimated_completion_days: Integer
  is_active: Boolean (Default: true)
  valid_from: Date
  valid_until: Date (Optional)
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Service Definition → Pricing Setup → Skill Requirement Mapping → 
Quality Standards Definition → Service Activation → Performance Monitoring → 
Pricing Optimization
```

**Detailed Process Steps:**
1. **Service Creation**: Define new service with complete specifications
2. **Pricing Strategy**: Set competitive pricing based on cost analysis and market research
3. **Skill Mapping**: Identify required skills and assign to qualified employees
4. **Quality Definition**: Establish quality standards and completion criteria
5. **Template Creation**: Create reusable templates for common service configurations
6. **Bundle Creation**: Combine services into attractive package offerings
7. **Performance Tracking**: Monitor service popularity, profitability, and customer satisfaction
8. **Continuous Improvement**: Regular review and optimization of service offerings

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
- **Basic Employee Operations**: Create, read, update employee information
- **Simple Role Assignment**: Assign basic roles (Admin, Manager, Employee)
- **Department Organization**: Organize employees by departments
- **Contact Information Management**: Maintain employee contact details
- **Basic Task Assignment**: Assign orders and tasks to employees
- **Simple Performance Tracking**: Basic productivity monitoring

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
  department: Enum [Sales, Design, Production, Administration]
  designation: String
  role_id: Foreign Key -> Role (Required)
  reporting_manager: Foreign Key -> Employee (Optional)
  skills: JSON [String] (e.g., ["Tailoring", "Design", "Sales"])
  specializations: JSON [String] (e.g., ["Wedding Dresses", "Formal Suits"])
  is_active: Boolean (Default: true)
  notes: Text
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
  is_active: Boolean (Default: true)
  created_at: DateTime
  updated_at: DateTime
}
```

### Process Flow
```
Employee Recruitment → Employee Profile Creation → Role Assignment → 
Task Assignment → Basic Performance Monitoring
```

**Simplified Process Steps:**
1. **Employee Profile Creation**: Create basic employee profile with contact information
2. **Role Assignment**: Assign simple role (Admin, Manager, or Employee)
3. **Department Assignment**: Assign to appropriate department
4. **Task Assignment**: Begin assigning orders and tasks based on role
5. **Basic Monitoring**: Simple tracking of assigned orders and completion

---

## Reports Module

### Purpose
Comprehensive reporting and analytics system providing business intelligence across all modules. This module generates detailed reports for performance analysis, business insights, and strategic decision-making with customizable parameters and automated scheduling.

### Core Features
- **Lead vs Conversion Reports**: Track lead conversion rates and sales funnel performance
- **Financial Reports**: Total sales, expenses, and profitability analysis
- **Staff Performance Reports**: Employee productivity, attendance, and quality metrics
- **Customer Analytics**: Customer behavior, billing patterns, and loyalty analysis
- **Purchase History Reports**: Supplier and material purchasing analysis
- **Custom Report Builder**: Create custom reports with flexible parameters
- **Automated Report Scheduling**: Schedule reports for automatic generation and delivery
- **Data Export**: Export reports in multiple formats (PDF, Excel, CSV)
- **Dashboard Views**: Interactive dashboards for real-time business monitoring
- **Comparative Analysis**: Period-over-period and year-over-year comparisons

### Data Models

#### Report Configuration Model
```
ReportConfiguration {
  id: UUID (Primary Key)
  report_name: String (Required)
  report_type: Enum [
    Lead_Conversion, 
    Sales_Analysis, 
    Expense_Analysis, 
    Staff_Performance, 
    Customer_Analysis, 
    Purchase_History,
    Financial_Summary,
    Custom
  ]
  parameters: JSON {
    date_range: {start_date: Date, end_date: Date},
    filters: JSON,
    grouping: String,
    sorting: String,
    columns: [String]
  }
  is_scheduled: Boolean (Default: false)
  schedule_frequency: Enum [Daily, Weekly, Monthly, Quarterly, Yearly] (Optional)
  recipients: JSON [String] (Email addresses)
  format: Enum [PDF, Excel, CSV, Dashboard]
  created_by: Foreign Key -> Employee
  is_active: Boolean (Default: true)
  created_at: DateTime
  updated_at: DateTime
}
```

#### Report Data Models

##### Lead Conversion Report
```
LeadConversionReport {
  id: UUID (Primary Key)
  report_date: Date
  period_start: Date
  period_end: Date
  total_leads: Integer
  leads_contacted: Integer
  leads_qualified: Integer
  leads_converted: Integer
  conversion_rate: Decimal (Percentage)
  qualification_rate: Decimal (Percentage)
  contact_rate: Decimal (Percentage)
  average_conversion_time: Decimal (Days)
  lead_source_breakdown: JSON {
    Meta: {leads: Integer, conversions: Integer, rate: Decimal},
    Google: {leads: Integer, conversions: Integer, rate: Decimal},
    Website: {leads: Integer, conversions: Integer, rate: Decimal},
    Referral: {leads: Integer, conversions: Integer, rate: Decimal}
  }
  employee_performance: JSON [
    {
      employee_id: UUID,
      leads_assigned: Integer,
      leads_converted: Integer,
      conversion_rate: Decimal
    }
  ]
  revenue_from_conversions: Decimal
  cost_per_conversion: Decimal
  roi_analysis: JSON
  created_at: DateTime
}
```

##### Staff Performance Report
```
StaffPerformanceReport {
  id: UUID (Primary Key)
  report_date: Date
  period_start: Date
  period_end: Date
  employee_id: Foreign Key -> Employee
  attendance_data: JSON {
    total_working_days: Integer,
    days_present: Integer,
    days_absent: Integer,
    attendance_percentage: Decimal,
    late_arrivals: Integer,
    early_departures: Integer
  }
  work_execution: JSON {
    orders_assigned: Integer,
    orders_completed: Integer,
    orders_in_progress: Integer,
    completion_rate: Decimal,
    average_completion_time: Decimal,
    on_time_delivery: Integer,
    delayed_deliveries: Integer
  }
  quality_metrics: JSON {
    total_work_done: Integer,
    rework_count: Integer,
    rework_percentage: Decimal,
    customer_satisfaction_avg: Decimal,
    quality_rating_avg: Decimal,
    complaints_received: Integer
  }
  productivity_score: Decimal (Calculated)
  revenue_generated: Decimal
  target_achievement: Decimal (Percentage)
  skill_utilization: JSON [String]
  improvement_areas: JSON [String]
  created_at: DateTime
}
```

##### Customer Analytics Report
```
CustomerAnalyticsReport {
  id: UUID (Primary Key)
  report_date: Date
  period_start: Date
  period_end: Date
  customer_segmentation: JSON {
    high_value_customers: {
      count: Integer,
      criteria: String,
      total_revenue: Decimal,
      avg_order_value: Decimal
    },
    repeat_customers: {
      count: Integer,
      reorder_rate: Decimal,
      avg_reorder_count: Integer,
      retention_rate: Decimal
    },
    new_customers: {
      count: Integer,
      acquisition_cost: Decimal,
      first_order_avg: Decimal
    }
  }
  billing_analysis: JSON {
    highest_billing_customers: [
      {
        customer_id: UUID,
        total_spent: Decimal,
        order_count: Integer,
        avg_order_value: Decimal
      }
    ],
    billing_distribution: JSON,
    payment_behavior: JSON
  }
  customer_lifecycle: JSON {
    acquisition_trends: JSON,
    churn_rate: Decimal,
    lifetime_value: Decimal,
    satisfaction_scores: JSON
  }
  geographic_analysis: JSON
  service_preferences: JSON
  created_at: DateTime
}
```

##### Purchase History Report
```
PurchaseHistoryReport {
  id: UUID (Primary Key)
  report_date: Date
  period_start: Date
  period_end: Date
  total_purchases: Decimal
  purchase_count: Integer
  average_purchase_value: Decimal
  supplier_analysis: JSON [
    {
      supplier_id: UUID,
      total_purchased: Decimal,
      order_count: Integer,
      avg_order_value: Decimal,
      on_time_delivery_rate: Decimal,
      quality_rating: Decimal
    }
  ]
  material_analysis: JSON [
    {
      material_category: String,
      total_spent: Decimal,
      quantity_purchased: Decimal,
      price_trends: JSON
    }
  ]
  cost_analysis: JSON {
    material_cost_percentage: Decimal,
    inventory_turnover: Decimal,
    wastage_percentage: Decimal
  }
  seasonal_trends: JSON
  budget_variance: JSON
  created_at: DateTime
}
```

### Report Types and Features

#### 1. Lead vs Conversion Reports
- **Conversion Funnel Analysis**: Visual representation of lead progression
- **Source Performance**: ROI analysis by lead source (Meta, Google, Website, etc.)
- **Employee Performance**: Individual conversion rates and lead handling efficiency
- **Time-based Analysis**: Conversion trends over time
- **Cost per Conversion**: Marketing spend efficiency analysis

#### 2. Sales and Financial Reports
- **Total Sales Analysis**: Revenue breakdown by period, service, customer
- **Expense Analysis**: Cost categorization and trend analysis
- **Profit & Loss**: Comprehensive P&L statements
- **Cash Flow**: Inflow and outflow analysis
- **Budget vs Actual**: Variance analysis and forecasting

#### 3. Staff Performance Reports
- **Attendance Tracking**: Punctuality, absence rates, and attendance patterns
- **Work Execution**: Order completion rates, productivity metrics
- **Quality Metrics**: Rework count, customer satisfaction, quality ratings
- **Skill Utilization**: Expertise deployment and development needs
- **Target Achievement**: Goal setting and performance measurement

#### 4. Customer Reports
- **High-Value Customer Analysis**: Top customers by revenue and profitability
- **Reorder Analysis**: Customer loyalty and repeat business patterns
- **Customer Segmentation**: Behavioral and demographic grouping
- **Satisfaction Analysis**: Service quality and customer experience metrics
- **Churn Analysis**: Customer retention and loss patterns

#### 5. Purchase History Reports
- **Supplier Performance**: Delivery, quality, and pricing analysis
- **Material Cost Analysis**: Price trends and cost optimization opportunities
- **Inventory Efficiency**: Stock turnover and wastage analysis
- **Budget Management**: Spend analysis and variance reporting
- **Seasonal Planning**: Demand forecasting and inventory planning

### Process Flow
```
Report Request → Parameter Configuration → Data Collection → 
Data Processing → Report Generation → Distribution → Analysis → 
Action Planning
```

**Detailed Process Steps:**
1. **Report Definition**: Define report type, parameters, and frequency
2. **Data Collection**: Gather data from relevant modules and time periods
3. **Data Processing**: Apply filters, calculations, and formatting
4. **Report Generation**: Create visual reports with charts and analysis
5. **Automated Distribution**: Send reports to designated recipients
6. **Performance Analysis**: Review insights and identify trends
7. **Action Planning**: Develop strategies based on report findings
8. **Continuous Monitoring**: Track progress and adjust strategies

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

The system implements a simple 3-tier role-based access control to keep complexity minimal while ensuring essential security.

### Admin Role
**Purpose**: Complete system access and user management

**Access**:
- Full access to all modules and features
- Can create, edit, and deactivate employee accounts
- Can assign roles to employees
- Access to all reports and analytics
- Can modify system settings

## Appointment Management Module

### Purpose
The Appointment Management Module is strictly for viewing the details of appointments that are collected from the landing page of the client website. No other features or actions are available in this module.

### Features
- View all appointments collected from the client web landing page

### Details
This module displays appointment details submitted by clients through the website's landing page. It does not support creating, editing, or deleting appointments, nor does it provide notifications, reminders, or employee assignments. The module is strictly read-only and intended for informational purposes only.

----
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