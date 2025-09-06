# WIT-Cloud-Transformation

Western Institute of Technology (WIT) – Hybrid Cloud Simulation Project for our Cloud Computing course (CPSY-304). We designed, deployed, and cost-estimated a hybrid Azure cloud environment to modernize WIT’s IT infrastructure, focusing on security, fault tolerance, and seamless student/staff access.

---

##Overview

Team project for CPSY-304 (Amrit, Aira, Dylan).  
The goal was to simulate a hybrid cloud migration for **Western Institute of Technology (WIT)**, which currently operates an aging Calgary datacenter and minimal branch-site servers in Edmonton and Lethbridge.

**Objectives:**
- Reduce costs and eliminate branch servers.
- Modernize authentication & identity (Entra ID).
- Implement hybrid services: file shares, web apps, containerized apps.
- Enforce Canadian compliance (PBMM).
- Provide secure connectivity, monitoring, and disaster recovery.
- Enable global student registration.
- Test features within a capped $5,000 monthly budget.

---

##Cloud Architecture

### Departments & Resource Groups
Each WIT department received its own **Azure Resource Group (RG)** with tagging for billing:

- **RG-Admin** → Admin Services (file shares, archives)  
- **RG-Academic** → Academic Services (websites, AKS, video storage)  
- **RG-IT** → IT Department (monitoring, backups, replication, device mgmt)  
- **RG-SchoolOfIT** → Instructor VM templates  
- **RG-Facilities, RG-Business, RG-Health, RG-Hospitality** → Staff device management  
- **RG-ITTesting** → Testing/dev area ($5k cap)  

**Tagging Schema:**
- Department: IT | Academic | Admin  
- Owner: user@wit.edu  
- Environment: Production  
- Purpose: FileShare | WebApp  

### Network & Connectivity
- **VPN Gateway** in Calgary datacenter  
- **Local Gateways** for Edmonton & Lethbridge  
- **Site-to-Site VPN** (CGY-EDM, CGY-LETH)  
- Optional **Hub-Spoke** model for future expansion  

---

##Services Implemented

| Service | Azure Tool | Owner |
|---------|------------|-------|
| Forms & Docs File Share | Azure Files + Lifecycle tiering | Admin Services |
| Lecture Video Share | Azure Files + Logic App (email notify) | Academic |
| Informational Website | Azure App Service | Academic |
| Registration Website | App Service + Azure SQL | Academic |
| Marks App (custom) | Azure VM + Autoscale | Academic |
| AKS Voting App | Azure Kubernetes Service | Academic |
| Instructor VM Templates | Azure Compute Gallery | School of IT |
| Device Management | Intune + Entra ID | All Departments |
| Monitoring | Azure Monitor + Log Analytics | IT |
| Backups & Replication | Azure Backup + Site Recovery | IT |

---

##Identity & Security

- **Entra ID tenant** for all departments
- **RBAC:**
  - IT Managers = Owner  
  - IT Supervisors = Contributor  
  - IT Techs = Reader  
  - Instructors = VM Contributor  
- MFA enforced for all users  
- Canada region only deployment  
- Mandatory tagging policy  
- Encryption at rest enforced  
- Legacy SKUs blocked  

**Compliance:** 5 PBMM policies implemented  
**Backup & DR:** Recovery Services Vault + Geo-replication (Calgary → US region)  
**Monitoring:** Alerts on CPU, Log Analytics workspace linked to VMs  

---

##Costing & Budgets

- **Test/Dev budget** capped at **$5,000/month**  
- Each department billed separately using tags  
- Estimated monthly costs included: storage tiering, App Services, AKS cluster usage, and VPN bandwidth  

---

##Demo Deliverables

- **Document:** Drawings, pricing, recommendations  
- **Video Demo:** <20 min walkthrough of Azure resources, configs, and features  
- **Team Logs:** Weekly updates of contributions  

---

##Weekly Logs

| Date | Time | Who | Work Done |
|------|------|-----|-----------|
| Apr 5 | 2 hrs | Amrit | Dept/tag structure research |
| Apr 6 | 1 hr | Amrit | Created test users, RGs, RBAC |
| Apr 11 | 3 hrs | Amrit | Entra ID users, enrollment VM |
| Apr 12 | 5 hrs | Amrit | Backup vault + Monitoring setup |
| Apr 12 | 8 hrs | Aira | File shares + tiering lifecycle |
| Apr 13 | 3 hrs | Amrit | VPN setup (Edmonton/Lethbridge) |
| Apr 13 | 5 hrs | Aira | PBMM policies implementation |
| Apr 9 | 4 hrs | Dylan | Azure topology research |
| Apr 11 | 5 hrs | Dylan | AKS web app + marks app demo |
| Apr 12 | 6 hrs | Dylan | Academic file share, IT VM template |
| Apr 13 | 4 hrs | Dylan | WIT informational website |

---

##Lessons Learned

- RBAC + admin units are critical to avoid “too open” access  
- Automatic storage tiering reduces costs significantly  
- Logic Apps are useful for workflow automation (email notifications)  
- AKS deployment adds complexity but demonstrates scalability  
- VPN setup ensures secure branch connectivity without on-prem servers  
- Tagging is essential for cost tracking across departments  

---

##Rubric Coverage

- **Identities & Org Structure**   
- **RBAC Roles & Locks**  
- **Initiatives & Policies** (PBMM compliance)  
- **Subscriptions & Billing**  (per-department tagging)  
- **File Shares & Video Workflows**   
- **Web Apps (Info + Registration)**   
- **AKS Voting App**   
- **Instructor VM Templates** 
- **Connection to Cloud**  (VPN S2S)  
- **Backup & Replication**   
- **Monitoring & Security**   

---

##Team

- **Amrit Kaur Dhiman** – IT Department (Identity, RBAC, VPN, Backup/Monitoring)  
- **Aira Therens** – Admin Services (File Shares, Policies, Compliance)  
- **Dylan Yee** – Academic Services (AKS, Websites, SQL, Marks App, VM Templates)

## Detailed Walkthroughs

- **Administrative Department (RG-Admin):** Azure Files + lifecycle tiering, Entra ID groups (AD-Users, AD-Directors, AD-OfficeStaff), least-privilege RBAC, and mapping SMB shares from a joined Windows 11 VM.  
  → See: [`docs/Administrative-Department.md`](docs/Administrative-Department.md)

- **PBMM Compliance Policies (Azure Policy):** Separation of duties (≤3 Owners, >1 Owner), storage network restriction, secure transfer required, SQL TDE. Step-by-step portal assignments and compliance checks.  
  → See: [`docs/PBMM-Policies.md`](docs/PBMM-Policies.md)

