# SPEC Scan Database

## Database (MongoDB?) & HTTP Server (Go)
- Database will hold ~1e7 records over its lifetime (this estimate accounts for ~10 revisions per record)
- Records will be revisable, and a complete history of each record will be preserved
- Some endpoints:
  - `/`: Login with CLASSE ID
  - `/add`: POST new records to add to the database
    - Only the user of the python process described below may use this endpoint
    - GET: not allowed
  - `/edit?id=<record ID>`: Edit a record
    - Only some fields should be editable
    - Not every user may edit any record
    - GET: Respond with a form to edit the record
    - POST: "Update" the record in the database with JSON provided in the body of the request
      - To preserve revision history, "update" might really mean "add a new record that's the same as the old record except for the updated fields and the `version` field, which has been incremented".
  - `/search`: Search for records 
    - GET: Respond with a form so users with no knowledge of the database's query syntax can perform "advanced" searches. For example:
      - Find records whose `startTime` field is within a certain range and whose `btr` field contains a certain string
      - Find records whose `btr` field exactly match a certain string and whose `specCommand` field exactly matches either one of two strings
    - POST: Perform search using parameters from the form
      - Respond with matching records presented in a table 
      - Columns should be interactive: add, remove, re-order, sort-by

## Automatic Record Injection (python)
- Periodically look for new SPEC scans on the CHESS DAQ, generate appropriate records for them, and submit them to the database by POST-ing them as JSON in the body of a request to `/add` on the server sketched above. 
