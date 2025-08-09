## L1-SI: High-Level Inventory of All Screens Across Platforms

1. Introduction
Below is an outline of the major screens for each platform, followed by a Screen Inventory & Flow Matrix. The matrix helps visualize navigation paths, dependencies, and cross-platform interactions. No dedicated in-app payment or meeting-scheduling screens exist in this release; these functions may be managed offline or introduced later if required by the business model.

---

## 2. Screen Catalog


The Customer app allows end users to request solar services with an optional multi-service cart, view or accept quotations, upload personal KYC documents, track project status, and create support tickets once the installation is executed. No partial or online payments are included in this release.

1. **Registration & Login Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: OTP-based login, capturing name/address/state/PIN for new signups  
   - Critical Data Elements: Phone, OTP, minimal profile info  
   - Primary API Dependencies: /auth/register, /auth/login  

2. **Services Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Catalog of available solar/renewable services, each can be added to the cart  
   - Critical Data Elements: Service name, images, short description, price range  
   - Primary API Dependencies: /services  

3. **Cart Checkout Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Displays selected (possibly multiple) services, user finalizes them into one lead request  
   - Critical Data Elements: Selected services, optional instructions, address confirmation  
   - Primary API Dependencies: POST /leads (multi-service)  

4. **My Records Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Lists all leads associated with this customer, showing statuses  
   - Critical Data Elements: Lead ID, creation date, status, next follow-up  
   - Primary API Dependencies: /leads?customerPhone=xxx  

5. **Service Detail Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: A tabbed interface for each lead—Info (read-only), Quotes (accept/reject), Docs (KYC), and Tickets (only if status=Executed)  
   - Critical Data Elements: Derived from the lead, plus any shared quotations  
   - Primary API Dependencies: /leads/{id}, /quotations, /tickets  

6. **Document Upload Screen (KYC)**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Unified KYC upload, supporting up to 7 doc types (Aadhaar, PAN, etc.)  
   - Critical Data Elements: Document type, size ≤10 MB  
   - Primary API Dependencies: /customers/{id}/kyc  

7. **Support Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Allows creation/view of support tickets for “Executed” leads  
   - Critical Data Elements: Ticket subject, status, message history, attachments  
   - Primary API Dependencies: /tickets, /tickets/{id}/messages  

8. **Profile & Settings Screen**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: Manage user details (name, email, address) and logout  
   - Critical Data Elements: Personal profile fields, phone read-only  
   - Primary API Dependencies: /user/{customerId}  

9. **Notifications Screen (Newly Introduced)**  
   - Primary User Roles: Customer  
   - Key Features/Purpose: A scrollable list of in-app notifications (e.g., lead status changes, quote updates) retrieved via manual polling  
   - Critical Data Elements: Notification text, timestamp, read/unread indicator  
   - Primary API Dependencies: /notifications?customerPhone=xxx  

---

### 2.1 Web Portal – Admin & KAM (WEBPRT)

The web portal supports both Admin and KAM roles. Certain actions (e.g., user management, commission approvals) are restricted to Admin. Others, like lead oversight or partial-quotation creation, apply to both roles but with narrower KAM permissions.

1. **Login Screen**  
   - Primary User Roles: Admin, KAM  
   - Key Features/Purpose: Email + password login, password reset link  
   - Critical Data Elements: Username/email, hashed password  
   - Primary API Dependencies: /auth/webLogin  

2. **Dashboard Screen**  
   - Primary User Roles: Admin (company-wide), KAM (territory-limited)  
   - Key Features/Purpose: High-level funnel & metrics (New, In Discussion, Physical Meeting Assigned, Won, Pending at Solarium, Under Execution, Executed)  
   - Critical Data Elements: Aggregated lead counts, time-based filters  
   - Primary API Dependencies: /dashboard/summary  

3. **Channel Partner Management Screen**  
   - Primary User Roles: Admin (full CRUD), KAM (read-only for assigned CPs)  
   - Key Features/Purpose: Approve new CP, activate/deactivate, bulk assign KAM, view CP profile  
   - Critical Data Elements: CP name, phone, status, assigned KAM, lead count  
   - Primary API Dependencies: /channelPartners  

4. **KAM Management**  
   - Primary User Roles: Admin (full CRUD)  
   - Key Features/Purpose: Create/edit KAM accounts, manage assigned CPs, set territory  
   - Critical Data Elements: KAM name, email, phone, region  
   - Primary API Dependencies: /kams  

5. **Customer Management Screen**  
   - Primary User Roles: Admin, KAM (limited to their customers)  
   - Key Features/Purpose: View/edit customer profiles, KYC doc approvals  
   - Critical Data Elements: Customer phone, leads, doc statuses  
   - Primary API Dependencies: /customers, /documents  

6. **Lead Management Screen**  
   - Primary User Roles: Admin (all leads); KAM sees only assigned leads  
   - Key Features/Purpose: View lead list, create new or bulk import, reassign leads, override statuses. “Unassigned Leads” preset filter/tab is accessible by Admin only.  
   - Critical Data Elements: Lead ID, status, assigned CP, date ranges, CSV import results  
   - Primary API Dependencies: /leads, /bulkImport  

7. **Quotation Management Screen**  
   - Primary User Roles: Admin, KAM  
   - Key Features/Purpose: Generate new quotations, partial-save (“Draft”) functionality only for Admin/KAM, filter by “Draft,” “Created,” “Shared”  
   - Critical Data Elements: Quotation ID, capacity, fees, subsidies, “sharedWithCustomer” toggle  
   - Primary API Dependencies: /quotations, /quotationWizard  

8. **Commission & Payouts Screen**  
   - Primary User Roles: Admin (approve, pay), KAM (read-only)  
   - Key Features/Purpose: Lists commissions (Pending/Approved/Paid), sets payment details (UTR)  
   - Critical Data Elements: Lead/Quotation references, commission amounts, payment status  
   - Primary API Dependencies: /commissions  

9. **Reports Screen**  
   - Primary User Roles: Admin (global), KAM (territory)  
   - Key Features/Purpose: Generate advanced analytics on leads, CP performance, funnel conversions, and export data  
   - Critical Data Elements: Time-series metrics, territory breakdown  
   - Primary API Dependencies: /reports  

10. **Global Settings Screen**  
   - Primary User Roles: Admin only  
   - Key Features/Purpose: System-level parameters (inflation %, token expiry, etc.)  
   - Critical Data Elements: Config flags, numeric thresholds  
   - Primary API Dependencies: /settings  

11. **Support Screen**  
   - Primary User Roles: Admin, KAM  
   - Key Features/Purpose: Ticket queue (filter by open/closed), respond to issues from Customer  
   - Critical Data Elements: Ticket ID, associated user or lead, status, message history  
   - Primary API Dependencies: /tickets, /tickets/{id}  

12. **My Profile Screen**  
   - Primary User Roles: Admin, KAM  
   - Key Features/Purpose: Update personal info, change password, log out  
   - Critical Data Elements: Name, email, phone, password reset  
   - Primary API Dependencies: /user/{adminOrKAMId}  

13. **Quotation Master Data Management Screen**  
   - Primary User Roles: Admin only  
   - Key Features/Purpose: Manage master data for panels, inverters, fees, BOM components, etc.  
   - Critical Data Elements: Price per unit, DCR flags, recommended items, region-phase mappings  
   - Primary API Dependencies: /masterData  

14. **Services Catalog Screen**  
   - Primary User Roles: Admin only  
   - Key Features/Purpose: Admin can add or edit available services for the CUSTAP “Services” screen  
   - Critical Data Elements: Service name, category, description, images, status (active/inactive)  
   - Primary API Dependencies: /services  

15. **Notifications Screen**  
   - Primary User Roles: Admin, KAM  
   - Key Features/Purpose: Central list of alerts (new leads, CP registrations, ticket updates), polled/manual retrieval  
   - Critical Data Elements: Text, timestamp, read/unread indicator  
   - Primary API Dependencies: /notifications?role=adminOrKAM  

---

## 3. Screen Inventory & Flow Matrix

| Screen Name                          | Platform      | Accessible From                                                                     | Next Possible Screens                                                                                       | Dependencies                                                                                                              |
|-------------------------------------|-------------- |-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
                                                                     |
| Login                               | Web Portal    | Web Portal URL (no session)                                                         | Dashboard (successful login)                                                                                | /auth/webLogin                                                                                                           |
| Dashboard                           | Web Portal    | Login → main landing                                                                | Leads, Commissions, CPs, KAMs, etc.                                                                         | /dashboard/summary                                                                                                       |
| Channel Partner Management Screen   | Web Portal    | Left sidebar “Channel Partners”                                                     | CP Detail, bulk assign KAM dialog                                                                           | /channelPartners                                                                                                          |
| KAM Management                      | Web Portal    | Left sidebar “KAMs”                                                                | KAM detail/edit                                                                                              | /kams                                                                                                                     |
| Customer Management                 | Web Portal    | Left sidebar “Customers”                                                           | Customer detail, KYC approvals                                                                              | /customers, /documents                                                                                                   |
| Lead Management       | Web Portal    | Left sidebar “Leads Management” (Admin sees all; KAM sees assigned only) | Lead Detail (sub-view), Bulk Import, CSV Export; “Unassigned Leads” tab for Admin only         | /leads, /bulkImport                                                                                                       |
| Quotation Management                | Web Portal    | Left sidebar “Quotation”                                                           | Quotation Wizard (Draft for Admin/KAM only), share toggles                                     | /quotations, /quotationWizard                                                                                             |
| Commission & Payouts                | Web Portal    | Left sidebar “Commission & Payouts”                                                | Payment detail pop-up (Approve/Pay)                                                                         | /commissions                                                                                                             |
| Reports                             | Web Portal    | Left sidebar “Reports” or top nav link                                             | Filter settings, CSV/PDF export, role-based data                                                             | /reports                                                                                                                 |
| Global Settings                     | Web Portal    | Left sidebar “Global Settings” (Admin only)                                        | Configuration page (inflation %, token expiry, etc.)                                                        | /settings                                                                                                                |
| Support                             | Web Portal    | Left sidebar “Support”                                                             | Ticket detail, replies, closure                                                                             | /tickets, /tickets/{id}                                                                                                  |
| My Profile                          | Web Portal    | Top bar → “Profile”                                                                | Edit info, password reset, log out                                                                          | /user/{adminOrKAMId}                                                                                                     |
| Quotation Master Data Mgmt          | Web Portal    | Left sidebar “Master Data”                                                         | Panels, Inverters, Fees, BOM, etc.                                                                          | /masterData                                                                                                              |
| Services Catalog                    | Web Portal    | Left sidebar “Services Catalog” or main link                                       | Add/edit service listings for CUSTAP                                                                        | /services                                                                                                                |
| Notifications Screen                | Web Portal    | Top bar bell icon or dedicated section                                             | View unread alerts, mark as read                                                                            | /notifications?role=adminOrKAM (polled/manual)                                                                           |



---