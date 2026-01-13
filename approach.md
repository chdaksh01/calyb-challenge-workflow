# My Approach to Reverse Engineering

To solve this challenge, I focused entirely on the network traffic. My goal was to bypass the manual interface and understand the raw data structure. I just wanted what APIs are called.

### 1. The Discovery Phase (Network Analysis)
I didn't guess the field names. I performed the task manually in the UI while watching the **Network Tab** in Chrome DevTools.
* I filtered for `Fetch/XHR` requests to ignore static assets.
* I filled out the form with unique values (like "$100.00") and hit save.
* I intercepted the `createPromotion` mutation in the payload. This revealed exactly how the server expects data (e.g., that it wanted `10000` instead of `100`).

### 2. The Standardization Strategy
Once I had the raw JSON payload, I had to make it reusable. I designed a simple JSON structure based on a **Producer-Consumer** model:
* **Step 1 (Producer):** Queries the API to find dynamic IDs (like the VIP Group ID). It extracts the result into a variable using a simple path selector (`items[0].id`).
* **Step 2 (Consumer):** Uses that variable (`{{find_vip_group.vip_group_id}}`) to create the promotion.

### 3. Generalization
This same logic applies to any platform (Salesforce, HubSpot, Jira).
1.  **Action:** Perform the task in the UI.
2.  **Intercept:** Catch the API call in the Network tab.
3.  **Parameterize:** Replace hard-coded IDs with variables from previous steps.
