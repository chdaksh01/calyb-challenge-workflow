# Archaeology Log

During my analysis, I discovered several specific quirks that differed from expectations.

### 1. Currency is stored in Cents
The UI displays prices as dollars (e.g., $100.00), but the API requires integers representing cents.
* **Observation:** Sending `100` results in a $1.00 discount.
* **Fix:** The value must be multiplied by 100 before sending (e.g., send `10000` for $100).

### 2. Booleans are Strings
In the `conditions` arguments, boolean flags are not sent as actual JSON booleans (`true`/`false`).
* **Observation:** The API expects the string literal `"false"` or `"true"`.
* **Gotcha:** Sending a raw boolean `false` might cause a type error or be ignored.

### 3. UI vs. API Discrepancy (The Hidden VIP Group)
When creating a promotion, the UI dropdown for "Customer Group" did not populate the existing "VIP" group, even though it existed in the database (ID: 1).
* **Workaround:** I could not rely on the UI to find the ID.
* **Solution:** I added a `customerGroups` query step in my workflow to programmatically search for the group by name ("VIP") and retrieve its ID directly, bypassing the UI limitation.

### 4. Nullable Field Handling - Fields not defined in instructions.
I observed that optional constraints like `startsAt`, `endsAt`, and `perCustomerUsageLimit` accept explicit `null` values in the mutation payload.
* **Observation:** The API does not enforce default values for these fields if they are sent as `null`.
* **Decision:** In the workflow map, I explicitly pass `null` for these fields to ensure the promotion is created without unintended time restrictions, rather than omitting the keys which might trigger server-side default behaviors.
