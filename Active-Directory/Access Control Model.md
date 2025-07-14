The **Access Control Model** is a security framework used to regulate access to objects and resources in **Active Directory** or other systems. Ite determines **who** can access **what** based on specific permissions and security rules.

---

### Key Components

#### 1. Access Tokens 

Access tokens define the **security context** of a process and include

* **Identity** of the user
* **Privileges** (permissions assigned to the user)

#### 2. Security Desciptors

Security descriptors control access through

* **SID (Security Identifier)** : Identifies the owner of an object
* **DACL (Discretionary Access Control List)** : Defines who is allowed or denied access
* **SACL (System access control List)** : Used for auditing access attempts

---

### Access Control List (ACL)

* A list of **Access Control Entries(ACE)**, where each ACE corresponds to an individual permission or audit access
* Determines who has permission and what actions can be performed on an object.

#### Two Types of ACLs:

1. DACL (Disscretionary ACL)
   1. Defines the permissions that **trustees** (users or group) have on an object
2. SACL (System ACL)
   1. Logs success and failure audit messages when an object is accessed

* ACLs are vital to the security architecture of Active Directory.

---

### Table of Permissions

| **Permission Type** | **Object** | **Permissions** |
|---------------------|-----------|------------------|
| **Generic All** | **Group** | - Add/Remove Members     <br>  - Add Ownership <br> - Reset Password of Members <br> - Grant Full control    |
|           |   **User** | - Reset Password (ForceChangePassword) <br> - Shadow Credentials (AddKeyCredentialLink) <br> - Targeted Kerberoasting (WriteSPN) <br> - Grant Full Control |
|   |   **Computer** |  - Read LAPS password (ReadLAPSPassword) <br> - Shadow Credentials(AddKeyCredentialLink) <br> - Kerberos RBCD (AllowedToAct) <br> - Grant Full Control |
|   |   **Domain Object** | - DCSync (DS-Replication-Get-Changes-All) <br> - Descendant Object Takeover <br> - Read LAPS password (ReadLAPSPassword) <br> - Write gPlink (Compromise Policies) <br> - Grant Full Control |
|   |   **Organizational Unit** | - Generic/Targeted Descendant Object Takeover  <br> - Write gPlink (Compromise Policies) <br> - Grant Full Control | 
|   |   **AdminSD Holder** | - Reset Password <br> - Write Members <br> - Grant Full Control |
|   |   **Group Policy** | - Evil GPOs (immediate Scheduled Task) <br> - Modify Group Policy <br> - Add Local Admin <br> - Grant Full Control |
|   |   **CertTemplate** | - ESC4 Attack (Modify Template) <br> - Grant Full Control |
|   |   **EnterpriseCA** | - Publish malicious templates <br> - ADCS escalation <br> - Grant Full Control |
|   |   **RootCA** | - Trust Rogue Certificate (modify cACertificate) <br> - Grant Full Control |
|   |   **NTAuthStore** | - Modify trust for NT Authentication <br> - Grant Full Control |
|   |   **IssuancePolicy** | - ADCS ESC13 (Modify msDS-OIDToGroupLink) <br> - Grant Full Control |
|   |   **Security Descriptor** | -WMI <br> - Powershell Remoting <br> - Remote Registry <br>- Grant Ownership |
|   **GenerticWrite**   |   **Group** | - Add/Remove Members |
|   |   **User** | - Shadow Credentials (AddKeyCredentialLink) <br> - Targeted Kerberoasting (WriteSPN) <br> - reset password <br> - Logon Scripts |
|   |   **Computer** |  - Shadow Credentials (AddKeyCredentialLink) <br> - Kerberos RBCD (AllowedToAct) <br> - SPN Jacking |
|   |   **AdminSD Holder** | - Reset Password <br> - Write Memberds |
|   |   **Domain Object** |  - Write gPlink (Compromise Policies) |
|   |   **Organizational Unit** | - Write gPlink (Compromise Policies) |
|   |   **Group Policy** | - Evil GPOs (immediate Scheduled Task) <br> - Modify Group Policy <br> - Add Local Admin |
|   |   **CertTemplate** | - ESC4 Attack (Modify Template) |
|   |   **EnterpriseCA** | - Publish Malicious Templates (Modify Certificate Templates) |
|   |   **RootCA** |     - Trust Rogue Certificate (modify cACertificate) |
|   |   **NTAuthStore** |   - Modify trust for NT Authentication |
|   |   **IssuancePolicy**  |   - ADCS ESC13 (Modify msDS-OIDToGroupLink) |
|   |   **Security Descriptor** | -WMI <br> - PowerShell Remoting <br> - Remote Registry |
| **WriteDacl** | **Group** | - Grant Any Privilege(WriteMembers)|
|   |   **User** | - Grant Any Privilege(GenericAll)|
|   |   **Computer** | - Grant Any Privilege(GenericAll)|
|   |   **Domain Object** | - DCSync (DS-Replication-Get-Changes-All) <br> - Grant Any Privilege(GenericAll)|
|   |   **Organizational unit** | - Grant Any Privilege(GenericAll)|
|   |   **Group Policy** | - Grant Any Privilege(GenericAll)|
|   |   **CertTemplate** | - Grant Any Privilege(GenericAll)|
|   |   **EnterpriseCA** | - Grant Any Privilege(GenericAll)|
|   |   **RootCA** | - Grant Any Privilege(GenericAll)|
|   |   **NTAuthStore** | - Grant Any Privilege(GenericAll)|
|   |   **IssuancePolicy** | - Grant Any Privilege(GenericAll)|
|   |   **Security Descriptor** | - Grant Rights(GenericAll)|
|   **AllExtendedRights** | **Group** | - Add/Remove Members |
|   |   **User**    |   -Reset Password(ForceChangePassword) |
|   |   **Computer** |  - Read LAPS Password (ReadLAPSPassword) |
|   **WriteOwner**  |   **Group**   |   -change Object Owner |
|   |   **user**    |   -change Object Owner |
|   |   **Computer**    |   -change Object Owner |
|   |   **Domain Object**    |   -change Object Owner |
|   |   **Organizational unit**    |   -change Object Owner |
|   |   **Group Policy**    |   -change Object Owner |
|   |   **Security Descriptor**    |   -change Object Owner |


---

### Explainations of Key Permissions

#### 1. GenericAll (Full Control)

*   Grants **Complete control** over the target object, allowing modifications of any attribute, membership changes, and ownership transfers

* **Abuse Potential**
  * **Users** : Reset Passwords, create shadow Credentials, perform Kerberoasting.
  * **Group** : Add/Remove members.
  * **Computers** : Read LAPS password, conduct Resource-Based Constrained Delegation(RBCD)
  * **Domains/OU** : Apply inherited control, modify group policies.
  * **Certificate Infrastructure** : Exploit Active Directory Certificate Services (ADCS)attacks.

#### 2. GenericWrite

* Allows modificationof **non-protected** attributes on the target object.

* **Abuse Potential**
  * **Users** :  create shadow Credentials, Kerberoasting via ``servicePrincipalNames``.
  * **Group** : Add/Remove members.
  * **Computers** : Enable RBCD attacks.
  * **GPOs (Group Policy Objects)** : Modify policies to execute malicious tasks.
  * **CertTemplates/EnterpriseCA/RootCA** : Modify certificate attributes to escalates privileges.

#### 3. WriteDACL

* Grants the ability to modify the **Discretionary Access Control List (DACL)** of an object
* **Abuse Potential**
  * **Users/Group/Computers** : Grant yourself full control (``GenericAll``)
  * **Domain** : Enable **DCSync** attacks to extract NTLM hashes.
  * **OUs** : Take over child objects.
  * **GPOs** : Modify policies to control targeted users and computers

#### 4. AllExtendedRights

* Provides **special privileges** to perform actions beyond basic read/write operations.
* **Abuse Potential**:
  * **users** : Reset Passwords (``ForceChangePassword``)
  * **Computers** : Read LAPS passwords (``ReadLAPSPassword``)
  * **Domains** : Perform **DCSync** attacks (during NTLM hashes)
  * **CertTemplates** : Enroll Certificates (potential ADCS attacks)

#### 5. WriteOwner

*   Allows changing the **ownership** of an object, granting unrestricted control over its security descriptor
*   **Abuse Potential**
    *   Once ownership is taken, the attacker can **modify the DACL** to grant full control (``GenericAll``)
    *   Used in combination with ``WriteDACL`` to elevate privileges silently.
  