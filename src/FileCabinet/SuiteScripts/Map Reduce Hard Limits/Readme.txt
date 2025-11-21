📘 How to Reproduce NetSuite Map/Reduce Persisted Data Limit (200MB)

This folder helps you reproduce the NetSuite Map/Reduce hard-limit error:

PERSISTED_DATA_LIMIT_FOR_MAPREDUCE_SCRIPT_EXCEEDED

All required assets — custom record, test scripts, and configurations — are already included in the SDF project.


--------------------------------------------------------------

✅ 1. Deploy the SDF Project

Deploy the entire SDF bundle to your sandbox or test account.

The deployment includes:

customrecord_persisted_data_record

mr_bulk_datagen.js

mr_persisteddata_pre_map_overflow.js

Supporting files & configurations

No manual setup required.

--------------------------------------------------------------

✅ 2. Run the Bulk Data Generator (3 Times)

After deployment:

Run mr_bulk_datagen three times.

This script creates bulk test data inside
customrecord_persisted_data_record.

Running it multiple times ensures:

✔ ~13–15 lakh (1.3M–1.5M) rows
✔ Enough volume + width to exceed persisted memory
✔ Consistent reproduction of the error

--------------------------------------------------------------

✅ 3. Run the Overflow Test Script

Next, execute:

mr_persisteddata_pre_map_overflow

This script:

Loads all records

Returns all columns

Passes the complete dataset to map()

At this point NetSuite calculates:

“Total size of keys & values not yet mapped”

Because the dataset exceeds 200MB, the script fails before map() runs.

🎯 Expected Outcome

Upon executing the overflow script:

map() does not execute

Script jumps directly to summarize()

You see the error:

PERSISTED_DATA_LIMIT_FOR_MAPREDUCE_SCRIPT_EXCEEDED


This confirms the persisted-data limit reproduction.

--------------------------------------------------------------

📂 Repo Files Overview
Item	                                        Purpose
customrecord_persisted_data_record	            Custom record used for generating & storing wide test data

mr_bulk_datagen.js	                            Generates 13–15 lakh rows (run 3 times)

mr_persisteddata_pre_map_overflow.js	        Loads entire dataset + all columns → triggers overflow before map()