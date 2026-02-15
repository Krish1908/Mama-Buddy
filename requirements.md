# MamaBuddy - Maternal Health Monitoring System
## Requirements Specification

### 1. Project Overview

**Project Name:** MamaBuddy (Maternal Health Monitoring System)
**Problem Statement:** India accounts for 20% of global maternal deaths (35,000+ annually), with 63% of rural pregnant women missing essential antenatal checkups due to accessibility, awareness, and infrastructure gaps.

**Vision:** To create an accessible, scalable maternal health monitoring system that works on basic mobile phones, overcoming literacy and technology barriers in rural India.

**Mission:** Reduce preventable maternal deaths by 40% through early detection of high-risk pregnancies and timely intervention.

### 2. Functional Requirements

#### 2.1 IVR System
- **FR-1:** System must support DTMF (Dual-tone multi-frequency) input for yes/no responses
- **FR-2:** IVR must support 8 major Indian languages with regional dialect options
- **FR-3:** Call scheduling must be based on pregnancy week with optimal timing (10 AM - 6 PM)
- **FR-4:** System must implement 3 retry attempts with 2-hour intervals for unanswered calls
- **FR-5:** SMS fallback must be sent if all call attempts fail

#### 2.2 Risk Assessment Engine
- **FR-6:** Initial implementation must use rule-based risk scoring (Red/Yellow/Green)
- **FR-7:** Risk calculation must consider: symptoms, pregnancy week, medical history, compliance
- **FR-8:** System must flag specific symptom combinations as critical (bleeding + pain, etc.)
- **FR-9:** Risk scores must be stored with timestamps for trend analysis
- **FR-10:** System must support future ML model integration for improved prediction

#### 2.3 Alert & Escalation System
- **FR-11:** Multi-channel alerts must be sent for Red cases (voice call, SMS, dashboard, email)
- **FR-12:** Escalation hierarchy: Mother → ASHA → PHC → Family member
- **FR-13:** Response time targets: <2 hours for Red alerts, 48-72 hours for Yellow alerts
- **FR-14:** System must track alert delivery status and ASHA action confirmation
- **FR-15:** Follow-up protocol must ensure case closure and outcome logging

#### 2.4 ASHA Dashboard
- **FR-16:** Web interface must be responsive and work on low-bandwidth connections
- **FR-17:** Priority queue must show Red alerts at top with real-time updates
- **FR-18:** Mother profiles must include complete health history and call logs
- **FR-19:** Action tracking must include visit completion, status updates, and notes
- **FR-20:** Reporting must generate weekly summaries and compliance analytics

#### 2.5 Data Management
- **FR-21:** System must store user data in PostgreSQL with proper indexing
- **FR-22:** Call logs must be stored in MongoDB for performance and analytics
- **FR-23:** Data must be encrypted at rest and in transit (HIPAA compliance)
- **FR-24:** Role-based access control must restrict data visibility by ASHA assignment
- **FR-25:** Data retention policy must comply with Indian healthcare regulations

### 3. Non-Functional Requirements

#### 3.1 Performance Requirements
- **NFR-1:** System must handle 10,000+ concurrent IVR calls with <1 second response time
- **NFR-2:** Dashboard must load within 3 seconds on 2G networks
- **NFR-3:** Risk assessment must complete within 5 seconds of call completion
- **NFR-4:** Alert delivery must have 99.9% reliability rate

#### 3.2 Scalability Requirements
- **NFR-5:** Architecture must support horizontal scaling for IVR call processing
- **NFR-6:** Database must support partitioning by district for performance
- **NFR-7:** System must handle peak loads during morning hours (10 AM - 12 PM)
- **NFR-8:** Cloud infrastructure must auto-scale based on call volume

#### 3.3 Security Requirements
- **NFR-9:** End-to-end encryption for all health data transmission
- **NFR-10:** Phone numbers must be masked in dashboard displays
- **NFR-11:** Audit logging for all data access and modifications
- **NFR-12:** Regular security vulnerability assessments and penetration testing

#### 3.4 Usability Requirements
- **NFR-13:** ASHA dashboard must be usable with minimal training (<30 minutes)
- **NFR-14:** IVR interface must be intuitive for users with limited literacy
- **NFR-15:** System must achieve 90% user satisfaction in post-delivery surveys
- **NFR-16:** Error messages must be clear and actionable in local languages

#### 3.5 Reliability Requirements
- **NFR-17:** System must achieve 95% call completion rate
- **NFR-18:** Uptime must be 99.5% during operational hours (6 AM - 10 PM)
- **NFR-19:** Data backup must occur daily with 7-day retention
- **NFR-20:** Disaster recovery plan must ensure <4 hour restoration time

### 4. Technical Constraints

#### 4.1 Infrastructure Constraints
- **TC-1:** Must work on basic mobile phones (feature phones) with 2G connectivity
- **TC-2:** Must support SMS fallback for areas with poor voice connectivity
- **TC-3:** Must be deployable on AWS/Google Cloud with auto-scaling
- **TC-4:** Must integrate with Exotel/Twilio IVR APIs

#### 4.2 Development Constraints
- **TC-5:** Initial MVP must be developed within 48 hours for hackathon
- **TC-6:** Backend must support both Node.js and Python Flask for flexibility
- **TC-7:** Frontend must use React.js for ASHA dashboard
- **TC-8:** Database must use PostgreSQL for relational data, MongoDB for logs

#### 4.3 Regulatory Constraints
- **TC-9:** Must comply with Indian healthcare data protection regulations
- **TC-10:** Must support integration with government MCTS (Mother & Child Tracking System)
- **TC-11:** Must maintain data sovereignty within India
- **TC-12:** Must support audit requirements for government funding

### 5. Success Criteria

#### 5.1 Health Outcomes
- **SC-1:** 40% reduction in missed antenatal checkups within 6 months
- **SC-2:** 60% faster detection of high-risk pregnancies (vs. traditional methods)
- **SC-3:** 25% increase in institutional deliveries
- **SC-4:** Measurable reduction in maternal complications (preeclampsia, GDM, anemia)

#### 5.2 System Performance
- **SC-5:** 95% call completion rate
- **SC-6:** <2 hour response time for Red alerts (90th percentile)
- **SC-7:** 85% ASHA action compliance on Yellow alerts
- **SC-8:** 90% user satisfaction in post-delivery surveys

#### 5.3 Operational Metrics
- **SC-9:** Cost per mother: ₹50-100 for full pregnancy monitoring
- **SC-10:** Scale: 1,000 mothers in pilot, 100,000+ in 6 months
- **SC-11:** ASHA workload reduction: 30% less time spent on routine monitoring
- **SC-12:** System uptime: 99.5% during operational hours

### 6. Assumptions & Dependencies

#### 6.1 Assumptions
- **A-1:** Pregnant women in target areas have access to basic mobile phones
- **A-2:** ASHA workers have basic smartphone literacy for dashboard use
- **A-3:** District health departments will provide initial mother data
- **A-4:** Government will support integration with existing health systems
- **A-5:** IVR service providers (Exotel/Twilio) will provide necessary APIs

#### 6.2 Dependencies
- **D-1:** Exotel/Twilio API availability and pricing
- **D-2:** AWS/Google Cloud infrastructure costs
- **D-3:** Government partnership for pilot implementation
- **D-4:** Medical expert input for risk scoring algorithms
- **D-5:** Local language voice recording resources

### 7. Out of Scope

#### 7.1 Phase 1 (MVP) Exclusions
- Video consultations
- Prescription management
- Laboratory test integration
- Payment processing
- Mobile app for mothers
- Advanced ML models (beyond basic risk scoring)

#### 7.2 Future Considerations
- Postpartum monitoring (0-42 days after delivery)
- Infant health tracking
- Integration with hospital EHR systems
- Advanced analytics with predictive modeling
- Mobile app for ASHA workers with offline capability

---
