# **Hands-on Lab: Setting Up Azure Active Directory (Azure AD)**  

This hands-on lab will guide you through the setup and management of **Azure Active Directory (Azure AD)** using both **Azure Portal** and **Azure CLI**.

---

## **📌 Lab 1: Creating an Azure AD Tenant**
🔹 *An Azure AD tenant is an instance of Azure AD that manages identities for users, apps, and devices.*

### **Step 1: Sign in to the Azure Portal**
1. Open [Azure Portal](https://portal.azure.com) and sign in with your Azure account.  
2. Search for **Azure Active Directory** in the search bar.  
3. Click on **Manage tenants** → **Create**.  
4. Select **Azure Active Directory** and click **Next**.  
5. Enter the **Tenant Name** and a **Custom Domain Name** (e.g., `yourcompany.onmicrosoft.com`).  
6. Choose a **Region** and click **Review + Create**.  
7. After the tenant is created, click **Go to Tenant**.

✅ **Congratulations! You have created an Azure AD tenant.**

---

## **📌 Lab 2: Adding Users to Azure AD**
🔹 *Now, let's add a user who will be managed by Azure AD.*

### **Using Azure Portal**
1. Go to **Azure AD** → **Users**.  
2. Click **New user** → **Create user**.  
3. Enter the **Name**, **Username**, and **Password** for the user.  
4. Assign a role (e.g., **User** or **Global Administrator**).  
5. Click **Create**.  

✅ **Now the user can log in to Azure AD with their credentials.**

### **Using Azure CLI**
```bash
az ad user create --display-name "John Doe" \
 --user-principal-name johndoe@yourtenant.onmicrosoft.com \
 --password "P@ssw0rd!"
```

---

## **📌 Lab 3: Assigning Roles to Users**
🔹 *Roles define what actions users can perform in Azure.*

### **Using Azure Portal**
1. Go to **Azure AD** → **Roles and Administrators**.  
2. Select a role, e.g., **Global Administrator** or **User Administrator**.  
3. Click **Add Assignments** → Select the user.  
4. Click **Assign**.  

✅ **The user now has the assigned role.**

### **Using Azure CLI**
```bash
az role assignment create --assignee johndoe@yourtenant.onmicrosoft.com \
 --role "User Administrator"
```

---

## **📌 Lab 4: Enabling Multi-Factor Authentication (MFA)**
🔹 *MFA adds an extra layer of security by requiring a second verification step.*

### **Using Azure Portal**
1. Go to **Azure AD** → **Security** → **MFA**.  
2. Click **Users** → Select the user.  
3. Click **Enable MFA**.  
4. The user will now be prompted to set up **MFA using SMS or Microsoft Authenticator** on their next login.

✅ **MFA is now enforced for the selected user.**

---

## **📌 Lab 5: Configuring Conditional Access Policies**
🔹 *Restrict access based on user location, risk, or device security.*  

### **Using Azure Portal**
1. Go to **Azure AD** → **Security** → **Conditional Access**.  
2. Click **New Policy** → Name it "Require MFA for Admins".  
3. Under **Users**, select **Administrator accounts**.  
4. Under **Cloud Apps**, choose **All Cloud Apps**.  
5. Under **Conditions**, add a rule:  
   - **Location**: Block sign-in from outside your country.  
6. Under **Access Controls**, choose **Require MFA**.  
7. Click **Create Policy**.  

✅ **Now, MFA is required for admin logins outside the corporate network.**

---

## **📌 Lab 6: Enabling Single Sign-On (SSO) for a SaaS App**
🔹 *Allow users to log in once and access multiple apps without re-entering credentials.*

### **Using Azure Portal**
1. Go to **Azure AD** → **Enterprise Applications**.  
2. Click **+ New Application** → Search for an app (e.g., Salesforce, Google Workspace).  
3. Select the app and click **Add**.  
4. Open the app and click **Single Sign-On (SSO)** → Choose **SAML**.  
5. Follow the SAML configuration steps provided by the app.  
6. Assign users who can access this app.  

✅ **SSO is now enabled for the selected application.**

---

## **📌 Lab 7: Connecting On-Prem Active Directory to Azure AD (Hybrid Identity)**
🔹 *Sync on-premises users to Azure AD using Azure AD Connect.*

### **Step 1: Install Azure AD Connect on Your Server**
1. Download **Azure AD Connect** from [Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
2. Run the installer and choose **Express Settings**.  
3. Enter **Azure AD Global Admin credentials**.  
4. Enter **On-Prem AD Admin credentials**.  
5. Choose **Start Sync** → Click **Finish**.  

✅ **Your on-prem users are now synced to Azure AD.**

---

## **📌 Lab 8: Monitoring and Auditing Azure AD**
🔹 *Track sign-ins, security risks, and audit logs.*

### **Using Azure Portal**
1. Go to **Azure AD** → **Monitoring**.  
2. Click **Sign-in logs** to track user logins.  
3. Click **Audit logs** to see administrative changes.  
4. Click **Risky Sign-ins** to check unauthorized access attempts.  

✅ **Now, you can monitor Azure AD security events.**

---

## **🔹 Summary of What You Have Learned**
✅ Created an **Azure AD Tenant**  
✅ Added **Users and Roles**  
✅ Configured **MFA and Conditional Access**  
✅ Set up **SSO for SaaS apps**  
✅ Integrated **On-Prem Active Directory**  
✅ Monitored **Sign-in logs and security reports**  
