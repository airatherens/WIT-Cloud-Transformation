# PBMM Policies (Canada Federal Regulatory Compliance)

This document describes the **five PBMM regulatory compliance policies** we implemented in Azure for the WIT Cloud Transformation project. These ensure separation of duties, secure storage access, encrypted transmission, and database encryption.

---

## 🔑 Policy 1 – Separation of Duties (Max 3 Owners)

**Control:** “A maximum of 3 owners should be designated for your subscription.”

**Why:** Prevent excessive privilege concentration.

**Steps:**
1. Add **3 Owner users** (1 Director from Administrative, 1 Manager from IT, 1 Manager from Academic Services).  
   - Azure Portal → Subscriptions → *Your subscription* → Access control (IAM) → Add role assignment → **Owner**
2. Assign the policy:  
   - Azure Portal → Policy → Definitions → Search: *maximum of 3 owners*  
   - Select → **Assign** → Scope: Subscription → **Create**

---

## 🔑 Policy 2 – Separation of Duties (More Than One Owner)

**Control:** “There should be more than one owner assigned to your subscription.”

**Why:** Avoid a single point of failure in management.

**Steps:**
1. Policy → Definitions → Search: *more than one owner*  
2. Select policy → **Assign** → Scope: Subscription → **Create**

---

## 🌐 Policy 3 – Storage Accounts Should Restrict Network Access

**Control:** “Storage accounts should restrict network access.”

**Why:** Block unrestricted public access; only allow trusted VNets or IPs.

**Manual Hardening:**
- For each storage account:  
  - Go to **Networking** → Change *Public network access* from **Enabled from all networks** → **Selected networks**.  
  - Add the public IP of the testing VM (e.g., VMware VM for Admin, Azure VM for IT).

**Policy Enforcement:**
1. Policy → Definitions → Search: *storage accounts should restrict network access*  
2. Assign → Scope: Subscription (or specific RGs) → **Create**

---

## 🔐 Policy 4 – Secure Transfer to Storage Accounts

**Control:** “Secure transfer to storage accounts should be enabled.”

**Why:** Enforce HTTPS (TLS) to secure data in transit.

**Steps:**
1. Manually enable for existing storage accounts:  
   - Storage account → Configuration → **Secure transfer required = Enabled** → Save
2. Assign the policy:  
   - Policy → Definitions → Search: *secure transfer to storage accounts should be enabled*  
   - Assign → Scope: Subscription or RG (e.g., RG-Admin) → Enforcement = Enabled → **Create**
3. Verify compliance:  
   - Azure Policy → Compliance → Storage accounts should show ✅ Compliant  
   - Double-check via storage account → Configuration

---

## 🗄️ Policy 5 – Transparent Data Encryption (TDE) for SQL DBs

**Control:** “Transparent data encryption on SQL databases should be enabled.”

**Why:** Ensure all Azure SQL databases are encrypted at rest.

**Steps:**
1. Policy → Definitions → Search: *Transparent data encryption on SQL databases should be enabled*  
2. Assign → Scope: Subscription or RG-Academic (where the Student Registration DB lives)  
   - Assignment name: `Audit TDE on SQL DBs - PBMM`  
   - Enforcement = Enabled  
3. Verify compliance:  
   - Policy → Compliance → Database (e.g., `StudentRegistrationDB`) should be ✅ Compliant.  
   - If ❌, go to SQL Server → Transparent data encryption → set to **On**.

---

## ✅ Compliance Verification

After assignments, go to **Azure Policy → Compliance**:
- **Max 3 Owners** → shows only 3 Owners assigned.  
- **More Than One Owner** → verifies ≥2 Owners.  
- **Storage Restriction** → confirms only whitelisted IPs/VNets.  
- **Secure Transfer** → all storage accounts show “Secure transfer required = Enabled.”  
- **SQL TDE** → DBs show “Encryption = Enabled.”  

---

## 📌 Summary

- Separation of Duties enforced via **Owner count policies**.  
- Storage access locked down to trusted IPs/VNets.  
- Secure transfer enabled for all storage accounts.  
- SQL databases encrypted with TDE.  
- Compliance view provides at-a-glance auditability.

These PBMM policies align with Canadian federal standards and directly map to the WIT project rubric for **Initiatives & Policies** and **Security**.
