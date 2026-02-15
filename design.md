# MamaBuddy - Maternal Health Monitoring System
## Design Specification

### 1. System Architecture Overview

#### 1.1 High-Level Architecture
The system follows a layered architecture with clear separation of concerns:

**Client Layer:**
- Pregnant Women (Basic Phones) - IVR interface
- ASHA Workers (Web Dashboard) - Management interface
- Health Administrators (Web Dashboard) - Analytics interface

**API Gateway Layer:**
- REST API Gateway handling authentication, rate limiting, and request routing
- API versioning support
- Request validation and transformation

**Service Layer:**
- IVR Service - Call scheduling, response processing, language management
- Risk Assessment Service - Rule-based scoring, ML integration, trend analysis
- Alert Service - Multi-channel notifications, escalation management, delivery tracking
- Dashboard Service - Mother management, ASHA workflow, action tracking
- Analytics Service - Health outcomes, system metrics, predictive analytics
- Reporting Service - Weekly summaries, compliance reports, export functions

**Data Layer:**
- PostgreSQL - User profiles, medical history, ASHA assignments, system configuration
- MongoDB - Call logs, response data, analytics data, audit logs
- Redis - Session management, cache, rate limiting, queue management

#### 1.2 Technology Stack
- **IVR System:** Exotel/Twilio API
- **Backend Framework:** Node.js with Express / Python Flask
- **Frontend Framework:** React.js with TypeScript
- **Database:** PostgreSQL (primary), MongoDB (logs), Redis (cache)
- **Message Queue:** RabbitMQ / AWS SQS
- **Cloud Platform:** AWS (EC2, RDS, S3, CloudFront) or Google Cloud
- **Monitoring:** Prometheus, Grafana, ELK Stack
- **CI/CD:** GitHub Actions, Docker, Kubernetes

### 2. Component Design

#### 2.1 IVR Service Component
**Responsibilities:**
- Schedule weekly calls based on pregnancy week
- Execute IVR calls with language-specific prompts
- Process DTMF responses from mothers
- Handle call failures with retry logic
- Send SMS fallback messages when calls fail

**Key Interfaces:**
- Call scheduling interface with mother details and timing
- Response processing interface for symptom data
- Failure handling interface with retry policies

#### 2.2 Risk Assessment Engine
**Responsibilities:**
- Calculate risk scores based on symptoms and medical history
- Apply rule-based scoring for Red/Yellow/Green categorization
- Support future ML model integration
- Store risk assessments with timestamps for trend analysis

**Risk Categories:**
- **RED (80-100):** Critical risk requiring immediate intervention
- **YELLOW (50-79):** Moderate risk requiring ASHA follow-up
- **GREEN (0-49):** Normal risk with routine monitoring

**Key Rules:**
- Bleeding after 20 weeks pregnancy
- Severe headache + swelling + hypertension history
- No fetal movement for 12+ hours (after week 24)
- Missed consecutive checkups
- Other symptom combinations indicating complications

#### 2.3 Alert Service Component
**Responsibilities:**
- Trigger multi-channel alerts based on risk levels
- Manage escalation hierarchy (Mother → ASHA → PHC → Family)
- Track alert delivery status and confirmations
- Close alerts with outcome documentation

**Alert Channels:**
- SMS notifications
- Voice call alerts
- Dashboard notifications
- Email alerts
- Push notifications (if mobile app available)

**Recipient Types:**
- Mother (primary recipient)
- ASHA worker (assigned health worker)
- Primary Health Center (PHC)
- Family member (emergency contact)

### 3. Database Schema Design

#### 3.1 Core Data Entities
**Mothers:**
- Personal information (name, phone, date of birth)
- Pregnancy details (LMP, expected delivery, current week)
- Contact information (village, district, emergency contact)
- Medical history and preferences
- ASHA worker assignment

**ASHA Workers:**
- Identification details (ASHA ID, name, contact)
- Assignment information (village, district, mother count)
- Activity tracking (last login, active mothers)

**Call Schedules:**
- Scheduling details (mother, time, pregnancy week)
- Call status and retry information
- Duration and completion tracking

**Risk Assessments:**
- Risk scores and categories
- Symptoms reported and contributing factors
- Timestamps and review information

**Alerts:**
- Alert types and severity levels
- Symptoms triggering the alert
- Escalation levels and status
- Resolution information and notes

**Alert Deliveries:**
- Delivery channel and recipient information
- Message content and delivery status
- Retry attempts and confirmation tracking

#### 3.2 Analytics Data Structure
**Call Logs:**
- Call timing and duration
- Question responses and response times
- Call quality metrics (network, audio)
- Device and location metadata

**Analytics Data:**
- Daily metrics by district
- Health outcomes tracking
- System performance indicators
- User engagement statistics

### 4. API Design

#### 4.1 REST API Endpoints
**Authentication & Authorization:**
- Login, logout, token refresh
- User profile management

**Mother Management:**
- List, create, read, update, delete mothers
- Call history and risk history retrieval

**Call Management:**
- Schedule, reschedule, cancel calls
- IVR webhook endpoints for call events
- Call results retrieval

**Risk Assessment:**
- Risk assessment from symptoms
- Risk history and review management

**Alert Management:**
- Alert listing and creation
- Alert acknowledgment and resolution
- Delivery status tracking

**Dashboard & Reporting:**
- Dashboard summary and priority queue
- Weekly reports and compliance analytics
- Health outcomes analytics

#### 4.2 IVR Webhook API
**Call Events:**
- Call started events with mother details
- Response events with symptom data
- Call failed events with failure reasons

**Webhook Payloads:**
- Standardized JSON format
- Timestamp and correlation IDs
- Event-specific data structures

### 5. User Interface Design

#### 5.1 ASHA Dashboard Wireframes
**Login Screen:**
- Phone number/ASHA ID input
- Password/PIN authentication
- Language selection
- Forgot password recovery

**Dashboard Home:**
- Priority alerts section (color-coded by risk)
- Today's tasks summary
- Quick statistics display
- Notification system

**Mother List View:**
- Search and filter capabilities
- Sort by risk level, village, pregnancy week
- Color-coded risk indicators
- Quick action buttons

**Mother Profile Page:**
- Personal information display
- Pregnancy timeline visualization
- Call history and risk trends
- Action history log
- Quick action panel

**Alert Management View:**
- Alert details with symptoms
- Action buttons for workflow
- Delivery status tracking
- Notes and follow-up section

**Reporting View:**
- Date range selection
- Metric cards and charts
- Export functionality
- Compliance analytics

#### 5.2 IVR Call Flow Design
**Call Structure:**
1. Greeting with personalized name
2. Pregnancy week confirmation
3. Symptom questions (8-10 key questions)
4. Closing message with health tips
5. Emergency message for Red risk cases

**Question Examples:**
- Checkup attendance
- Bleeding symptoms
- Headache and vision problems
- Swelling in hands/feet/face
- Fetal movement regularity
- Fever or urinary symptoms
- Abdominal pain
- Medication compliance

**Response Handling:**
- DTMF input processing (1 for Yes, 2 for No)
- Timeout and retry logic
- Incomplete response handling
- Language-specific prompt management

### 6. Security Design

#### 6.1 Authentication & Authorization
**Authentication:**
- JWT-based authentication with refresh tokens
- Session management with automatic timeout
- Multi-factor authentication for administrators

**Authorization:**
- Role-based access control (RBAC)
- ASHA workers: Assigned mothers only
- Supervisors: District-level access
- Administrators: Full system access
- System accounts: IVR service access

#### 6.2 Data Protection
**Encryption:**
- End-to-end encryption for health data
- Data encryption at rest and in transit
- Secure key management using cloud KMS

**Data Privacy:**
- Phone number masking in displays
- Audit logging for data access
- Data retention policy compliance
- Regular security assessments

#### 6.3 Network Security
**Protocol Security:**
- HTTPS only for all endpoints
- API rate limiting and throttling
- IP whitelisting for webhooks
- DDoS protection mechanisms

**Monitoring:**
- Regular security audits
- Penetration testing
- Vulnerability assessments
- Compliance monitoring

### 7. Deployment Architecture

#### 7.1 Cloud Infrastructure
**AWS Example:**
- Region: ap-south-1 (Mumbai)
- VPC with public/private subnets
- Load balancer with auto-scaling groups
- RDS PostgreSQL with multi-AZ deployment
- ElastiCache Redis for caching
- S3 for voice files and backups
- CloudFront for CDN delivery
- CloudWatch for monitoring
- SQS for message queuing

**High Availability:**
- Multi-AZ database deployment
- Auto-scaling application servers
- Load-balanced traffic distribution
- Geographic redundancy planning

#### 7.2 Container Orchestration
**Development Environment:**
- Docker containers for all services
- Docker Compose for local development
- Service dependencies and networking
- Environment configuration management

**Production Environment:**
- Kubernetes cluster for orchestration
- Service mesh for communication
- Config maps and secrets management
- Horizontal pod autoscaling

### 8. Testing Strategy

#### 8.1 Testing Levels
**Unit Testing:**
- Backend service unit tests
- Frontend component tests
- Database integration tests
- 80% code coverage target

**Integration Testing:**
- API integration tests
- IVR provider integration
- End-to-end user flows
- Performance load testing

**Property-Based Testing:**
- Risk assessment consistency properties
- Risk monotonicity properties
- Alert delivery reliability properties
- Data integrity properties

#### 8.2 Test Automation
**CI/CD Pipeline:**
- Automated test execution
- Code quality checks
- Security scanning
- Performance benchmarking

**Test Environments:**
- Development environment
- Staging environment
- Production-like testing
- Disaster recovery testing

### 9. Monitoring & Observability

#### 9.1 Key Metrics
**System Health:**
- CPU, memory, disk usage
- Response times and latency
- Error rates and failures

**Business Metrics:**
- Calls completed and failed
- Alerts triggered and resolved
- ASHA actions completed
- User engagement rates

**User Experience:**
- Call completion rates
- Dashboard load times
- Alert response times
- User satisfaction scores

#### 9.2 Alerting & Logging
**Alerting Rules:**
- Critical: System downtime, database failures
- High: Performance degradation, SLA violations
- Medium: Error rate increases, capacity warnings
- Low: Backup failures, minor issues

**Logging Strategy:**
- Structured logging with correlation IDs
- Centralized log aggregation
- Audit logging for compliance
- Call detail records for analysis

### 10. Disaster Recovery & Business Continuity

#### 10.1 Backup Strategy
**Data Backups:**
- Daily automated database backups
- 7-day retention for critical data
- S3 versioning for voice files
- Configuration backup in Git

**Recovery Objectives:**
- RTO (Recovery Time Objective): 4 hours
- RPO (Recovery Point Objective): 24 hours
- Failover procedures documentation
- Communication plans for stakeholders

#### 10.2 Business Continuity
**High Availability:**
- Multi-region deployment planning
- Load balancing and failover
- Database replication strategies
- Service redundancy design

**Operational Procedures:**
- Documented runbooks for incidents
- Team training and drills
- Regular backup testing
- Compliance with regulations

---
