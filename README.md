# entra-id-lab
Hands-on Microsoft Entra ID administration lab simulating real world IT tasks

## Environment
- Platform: Microsoft Azure Free Tier
- Directory: Microsoft Entra ID (Free)
- Simualted Org: MyLawFirm

## Labs
- Lab 1 - User Lifecycle Managment 
- Lab 2 - Groups and Dynamic Membership 
- Lab 3 - Conditional Access and MFA 
- Lab 4 - Roles and Permissions 
- Lab 5 - Audit and Sign-In Logs

### Lab 1 - User Lifecycle Managment 

### Task 1 — Create a New User
Scenario: A new paralegal is joining MyLawFirm and needs an account provisioned.

Steps:
1. Navigated to Microsoft Entra ID > Users
2. Selected New User > Create New User
3. Configured the following:
   - Display Name: Jane Smith
   - UPN: jsmith@mtthwnunez.onmicrosoft.com
   - Job Title: Paralegal
   - Department: Legal
   - Company: MyLawFirm
   - Employee ID: 1001
   - Employee Type: Employee
   - Hire Date: 04/03/2026
4. Reviewed and created the account

Result: User account successfully provisioned and visible in the directory.

### Task 2 — Disable a User Account
Scenario: Jane Smith is going on indefinite leave. Her account must be blocked immediately without deleting it.

Steps:
1. Navigated to Entra ID > Users > Jane Smith
2. Clicked the Settings tab on her profile
3. Toggled "Account enabled" to No
4. Saved changes

Result: Account disabled. Jane can no longer sign in but her account and data remain intact for when she returns.

Why not delete? Disabling preserves mailbox data, group memberships, and audit history. Deletion is permanent and done only during offboarding.

### Task 3 — Delete a User (Offboarding)
Scenario: A terminated associate attorney needs to be fully offboarded and their account removed.

Steps:
1. Created test user John Doe (jdoe@mtthwnunez.onmicrosoft.com), Associate Attorney
2. Navigated to his profile in Entra ID > Users
3. Clicked Delete and confirmed removal

Result: Account removed from active directory.

Key Detail — Soft Delete:
Entra ID retains deleted users for 30 days in a "Deleted users" bin before permanent removal. During this window an admin can restore the account if the deletion was made in error. 

Restore vs. Permanent Delete:
- Restore: Account comes back with all properties and group memberships intact
- Permanent delete: Irreversible, done after 30 days or manually by an admin

### Task 4 — Reset a User Password
Scenario: Jane Smith forgot her password and called the MyLawFirm 
help desk. Account needs a temporary password with a forced reset on 
next sign-in.

Steps:
1. Navigated to Entra ID > Users > Jane Smith
2. Clicked "Reset password" in the top toolbar
3. Selected Auto-generate password
4. Checked "Require this user to change their password when they 
   first sign in"
5. Clicked Reset password
6. Copied temporary password to securely communicate to user

Result: Temporary password generated. Jane will be forced to set 
a new password on her next sign-in.

Key Concept — Secure Password Delivery:
Temporary passwords should never be sent via plain text email. 
Best practices include:
- Communicating verbally over phone
- Sending via encrypted email
- Delivering through a secure IT ticketing system
- Using a password manager with secure share features

Key Concept — Forced Reset:
Always require users to change their temporary password on first 
sign-in. This ensures the IT admin never knows the user's final 
password, maintaining privacy and security hygiene.

### Lab 2 - Groups and Dynamic Membership 

### Objective
Create and manage security groups to control access to firm resources.
Simulate real-world group administration including membership changes.

### Task 1 — Create a Security Group (Paralegals)
Scenario: MyLawFirm needs a group to manage access for all paralegal staff.

Steps:
1. Navigated to Entra ID > Groups > New Group
2. Configured:
   - Group type: Security
   - Group name: Paralegals
   - Description: All Paralegal staff at MyLawFirm
   - Membership type: Assigned
3. Added self as owner
4. Added Jane Smith (jsmith@mtthwnunez.onmicrosoft.com) as member
5. Clicked Create

Result: Security group created with one active member.

### Task 2 — Create a Second Security Group (Attorneys)
Scenario: MyLawFirm needs a separate group for attorney staff.

Steps:
1. Created second Security group named Attorneys
2. Description: All attorney staff at MyLawFirm
3. Added self as owner
4. Left membership empty pending attorney hires

Result: Security group created and ready for member assignment.

### Task 3 — Dynamic Membership Groups (P1 Feature)
Scenario:Automatically populate a group based on user attributes
(e.g., all users where Department = "Legal").

How it works:
1. Create a new Security group
2. Set Membership type to "Dynamic User"
3. Build a rule using user attributes:
   - Property: department
   - Operator: Equals
   - Value: Legal
4. Entra evaluates all users against the rule and auto-populates the group
5. Any new user created with Department = Legal is added automatically

Licensing Note: Dynamic membership requires Microsoft Entra ID P1
or higher. Not available on the free tier. In a production environment
this would eliminate manual group management as the firm grows.

Why it matters:In a 50-person law firm, adding a new paralegal
automatically inherits the correct access without IT intervention.

### Task 4 — Managing Group Membership
Scenario: Onboard a new paralegal, correct an accidental membership
removal.

Steps:
1. Created user Michael Torres (mtorres@mtthwnunez.onmicrosoft.com),
   Paralegal, Legal Department, MyLawFirm
2. Added Michael to Paralegals group during account creation
3. Removed Jane Smith from Paralegals group (simulating accidental removal)
4. Re-added Jane Smith to Paralegals group (simulating correction)

Result: Demonstrated full membership management cycle including
error recovery.

## Lab 3 - Roles and Permissions

### Objective
Assign and manage built-in Entra ID roles following least privilege 
principles. Simulate a real-world scenario where a junior IT technician 
needs scoped access without full admin rights.

### Task 1 — Assign Helpdesk Administrator Role
Scenario: Junior IT tech Alex Rivera needs to reset passwords and 
manage basic user accounts without full admin access.

Steps:
1. Created user Alex Rivera (arivera@mtthwnunez.onmicrosoft.com),
   IT Support Specialist, IT Department, MyLawFirm
2. Navigated to Entra ID > Roles and administrators
3. Searched for and selected Helpdesk Administrator
4. Clicked Add assignments and added Alex Rivera
5. Confirmed assignment

Result: Alex can reset passwords and manage basic user accounts 
but cannot assign roles or access Global Admin settings.

### Task 2 — Review Role Permissions
Scenario: Verify what Helpdesk Administrator can and cannot do.

Helpdesk Administrator can:
- Reset passwords for non-admin users
- Manage basic user account properties
- View audit logs

Helpdesk Administrator cannot:
- Assign roles to other users
- Access Global Admin settings
- Manage billing or subscriptions

Key Concept — Least Privilege: Alex receives exactly the permissions 
needed for his role and nothing more. This limits the blast radius if 
his account is ever compromised.

### Task 3 — Compare Helpdesk Administrator vs User Administrator
Scenario: Understand when to assign each role based on job 
responsibilities.

| Role | Can Reset Passwords | Can Create/Delete Users | Can Manage Groups | Can Assign Roles |
**********************************************
| Helpdesk Administrator | Yes | No | No | No |
| User Administrator | Yes | Yes | Yes | No |

### Task 4 — Assign Additional Role (User Administrator)
Scenario: Alex is promoted and now handles full user and group 
management for MyLawFirm.

Steps:
1. Navigated to Roles and administrators > User Administrator
2. Clicked Add assignments and added Alex Rivera
3. Confirmed assignment

Result: Alex now holds both Helpdesk Administrator and User 
Administrator roles reflecting his expanded responsibilities.

### Task 5 — Remove a Role Assignment
Scenario: Alex is moving to a non-IT role. User Administrator 
access must be revoked immediately while retaining Helpdesk 
Administrator for basic support tasks.

Steps:
1. Navigated to Roles and administrators > User Administrator
2. Located Alex Rivera in the assignments list
3. Selected his assignment and clicked Remove assignments
4. Confirmed removal
 

Result: User Administrator role revoked. Alex retains only 
Helpdesk Administrator reflecting his new limited responsibilities.

Key Concept — Role Hygiene: Promptly removing unnecessary role 
assignments reduces attack surface and maintains least privilege 
as staff roles change.

## Lab 4 - Conditional Access and MFA

### Objective
Enforce Multi-Factor Authentication and access policies to protect 
MyLawFirm's sensitive client data.

### Licensing Note
Conditional Access policies require Microsoft Entra ID P1.
MFA Registration policy requires Microsoft Entra ID P2.
The following documents how these would be configured in a 
production environment.

### Task 1 — Enable Security Defaults (Free Tier)
Scenario: MyLawFirm wants baseline MFA enforcement without a 
P1 license.

What Security Defaults do:
- Require MFA for all users
- Block legacy authentication protocols
- Require MFA for all admin accounts
- Free and built into every Entra tenant

How to enable:
1. Navigate to Entra ID > Properties
2. Click "Manage security defaults" at the bottom
3. Toggle "Security defaults" to Enabled
4. Save

Best for: Small firms on a budget that need baseline protection 
without investing in P1 licensing.

### Task 2 — Conditional Access Policy: Require MFA for All Users (P1)
Scenario: All MyLawFirm staff must complete MFA when signing in 
from any device.

How it would be configured:
1. Navigate to Security > Conditional Access > New Policy
2. Configure:
   - Name: Require MFA - All Users
   - Users: All users
   - Cloud apps: All cloud apps
   - Conditions: Any location, any device
   - Grant: Require multi-factor authentication
3. Set policy to On and Save

Result: Every user prompted for MFA on every sign-in regardless 
of device or location.

### Task 3 — Conditional Access Policy: Block Outside US (P1)
Scenario: MyLawFirm only operates in the US. Any sign-in attempt 
from outside the country should be blocked.

How it would be configured:
1. Navigate to Security > Conditional Access > Named Locations
2. Create a named location for United States
3. Create a new policy:
   - Name: Block Non-US Sign-ins
   - Users: All users
   - Conditions: Locations > Exclude United States
   - Grant: Block access
4. Set policy to On and Save

Result: Any authentication attempt from outside the US is blocked 
automatically.

## Lab 5 - Audit and Sign-In Logs

### Objective
Use Entra ID monitoring tools to investigate suspicious account activity 
and maintain an administrative audit trail for MyLawFirm.

### Task 1 — Review Sign-In Logs
Scenario: An attorney reports their account may have been accessed 
without their knowledge. Pull sign-in logs to investigate.

Steps:
1. Navigated to Entra ID > Monitoring > Sign-in logs
2. Reviewed sign-in history including:
   - Status (Success/Failure)
   - Location and IP address
   - Device and browser used
   - Conditional Access policies applied

Findings:
- Two successful sign-ins detected
- Location showed Georgia (VPN) — flagged as potential anomaly
- Investigated and confirmed as authorized VPN usage (false positive

### Task 2 — Review Audit Logs
Scenario: Review the full administrative activity trail for MyLawFirm's 
Entra tenant.

Steps:
1. Navigated to Entra ID > Monitoring > Audit logs
2. Reviewed full log history of all administrative actions
3. Confirmed all lab activities were logged including:
   - User account creations (Jane Smith, Michael Torres, Alex Rivera)
   - Group assignments and removals
   - Role assignments and revocations
   - Account enable/disable changes

Result: Complete audit trail confirmed for all administrative 
actions performed during the lab.

### Task 3 — Filter Audit Logs
Scenario: Isolate specific administrative actions to reduce noise 
during an investigation.

Steps:
1. Applied filter for User Management category
2. Successfully isolated all user-related administrative events
3. Confirmed visibility of all user creations and modifications

Result: Filtered view showed only relevant user management events 
confirming filtering works correctly for scoped investigations.

### Task 4 — Simulate an Investigation
Scenario: Prove when and by whom Jane Smith's account was disabled, 
and what exactly changed.

Steps:
1. Filtered Audit logs by User Management
2. Located entry for Jane Smith's account disable event
3. Clicked entry and reviewed Modified Properties
4. Confirmed the following change was logged:
   - Property: accountEnabled
   - Old value: true
   - New value: false
   - Timestamp and initiating admin recorded

Result: Full evidence log confirmed. Timestamp, responsible admin, 
and exact property change all captured in the audit trail.

Key Concept — Why Audit Logs Matter:
In a law firm handling sensitive client data, audit logs are critical for:
- Proving compliance with data protection requirements
- Investigating unauthorized access or changes
- Providing evidence in the event of a security incident
- Demonstrating due diligence to clients and regulators

---

## Skills Demonstrated
- Microsoft Entra ID user provisioning
- Employee onboarding and offboarding workflows
- Security group creation and management
- Assigned vs. dynamic membership concepts
- Entra ID role assignment and removal
- Least privilege access principles
- Conditional Access and MFA concepts (P1/P2)
- Security Defaults awareness
- Sign-in log investigation and VPN false positive identification
- Audit log filtering and incident investigation
- Entra ID licensing tier awareness (Free vs. P1/P2)
- User password reset and temporary credential management
- Secure password delivery best practic
