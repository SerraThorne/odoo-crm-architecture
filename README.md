# odoo-crm-architecture
End-to-end CRM architecture, workflows, documentation, and reporting built in Odoo v19
Architecture Summary
##  System Architecture

### **Core Components**
- **Azure OAuth2 Email Integration** 
- **CRM Pipeline Reconstruction** 
- **QWeb PDF Engine**  
- **Stage-Based Automations** 
- **SLA Timers**  
- **Custom Studio Fields**  
- **Internal Channels** 
- **Property Linking System** 



Features Overview
## Major Features

### **1. Azure OAuth2 Email System**
- Registered and configured an Azure App  
- Set IMAP + SMTP with OAuth2  
- Fully authenticated incoming/outgoing email  
- Enabled alias-based case creation

### **2. Full Pipeline Rebuild**
Created a 14-stage CRM lifecycle:

New → Property Search → Property Data Sheet → Sent for Approval →
Updates → Home Showing → Property Accepted →
Lease In Progress → Lease Executed →
Funds Out → Move-In → Extensions →
Vacate / Move-Out → Closed


### **3. SLA Automations**
Built and validated:

- Updates → 24h follow-up  
- Sent for Approval → 24h  
- Approved → Lease Prep Reminder  
- Home Showing → Walk-through reminder  
- Lease In Progress → 24h  
- Lease Executed → Owner Docs Reminder  
- Funds Out → Owner Payment Reminder  

### **4. QWeb PDF: Property Data Sheet**
- Created a custom QWeb PDF  
- Linked to selected property model  
- Null-safe rendering  
- Field mapping for utilities, fees, beds, baths, address, amenities  
- Ready for Smart Button integration

### **5. VA Workflow Tab**
Added fields for:

- Owner contact  
- Policyholder contact  
- Walk-throughs  
- Property acceptance  
- Lease details  
- Move-in steps  
- Move-out steps  
- Deposit tracking  
- Funds tracking  

### **6. Internal Communication System**
Channels created for:
- VAs  
- Corey  
- Operators  
- System Alerts  

Ready for future stage-based automation.



Code Example

## QWeb Code Example (Property Data Sheet)

```xml
<t t-name="property_data_sheet.report_template">
  <t t-call="web.html_container">
    <t t-set="lead" t-value="o"/>
    <t t-set="prop" t-value="o.x_studio_selected_property or False"/>

    <main class="o_report">
      <style>
        body {
          font-family: Arial, sans-serif;
          font-size: 11px;
          padding: 0 20px;
        }
      </style>

      <h1>Property Data Sheet</h1>

      <div>
        <strong>Address:</strong>
        <t t-esc="prop.x_studio_property_address or ''"/>
      </div>

      <div>
        <strong>Beds:</strong>
        <t t-esc="prop.x_studio_bedrooms or ''"/>
      </div>

      <div>
        <strong>Baths:</strong>
        <t t-esc="prop.x_studio_bathrooms or ''"/>
      </div>

      <div>
        <strong>Utilities Included:</strong>
        <t t-esc="prop.x_studio_utilities_included or ''"/>
      </div>
    </main>
  </t>
</t>


Challenges & Solutions

1. SaaS Restrictions
No custom Python modules

No server access

No environment (env) write calls

Sandbox blocks all attribute assignments during record creation

Solution:

Engineered 100% of the system using Studio + Automations + QWeb.



2. Case Number Sequence Failures
Every attempt failed due to SaaS constraints:

Computed fields

Automations

Python code

AI actions

env['ir.sequence']

Solution:

Manual or lead-name embedded sequence system.


3. PDF Background Image Limitations
wkhtmltopdf restrictions prevent:

background PNG imports

fixed-position layers

Solution:

Clean, minimalist inline CSS layout.
