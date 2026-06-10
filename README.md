#############################################################################
#############################################################################
### JMeter Performance Testing Suite for SPE Role Date Created: 09-06-2026###
#############################################################################
#############################################################################

This repository contains the performance and load testing suites built with [Apache JMeter](https://jmeter.apache.org/). It provides a ready-to-use framework for local performance runs. This project was created and executed on windows machine so instrcutions are based for Windows OS.

## 📂 Project Structure

```Assurity
├── Scripts/
│   └── Jmeter_AssuritySPE.jmx                          # Primary JMeter test plan
├── Config/
│   └── user.properties                                 # Environment-specific variables
├── Test_Data/
│   └── users.csv                                       # Test data parameters
├── Test_Results/                                       # Local execution targets
│   └── Test_Results.jtl                                # Test results files.
│   └── Test_Response_Captured_Parameters.csv           # Capture parameters files.
│   └── Test_Results_HTML Folder                        # HTML report.
└── README.md                                           # Documentation
```

## 🛠️ Prerequisites

Before executing tests locally, ensure you have the following installed:

*   **Java Runtime Environment (JRE)** or **Java Development Kit (JDK)** (Version 8 or higher).
*   **Apache JMeter** (Version 4.0+ recommended). Add the `jmeter/bin` folder to your system's `PATH` variable (Optional step if absolute path is provided).

## 🚀 Local Execution (CLI Mode)

> ⚠️ **Important:** Never run performance load tests using the JMeter GUI. Only use the GUI for script creation, debugging, and initial validation.

Navigate to the apache bin directory and run the command matching your operating system:

### Windows
```cmd
jmeter -n ^
  -t .\scripts\main_load_test.jmx ^
  -p .\config\user.properties ^
  -l .\results\output.jtl ^
  -e -o .\results\html-report\
```

### Command Flags Breakdown
*   `-n`: Specifies that JMeter will run in non-GUI (CLI) mode.
*   `-t`: Path to the source JMeter Test Plan (`.jmx` file).
*   `-p`: Path to the environment configuration properties file.
*   `-l`: Path to the output log file (`.jtl`) to record test sample data.
*   `-e`: Automatically generates a web-ready dashboard after load execution.
*   `-o`: Target folder path where the dashboard HTML report will be generated.

---

## ⚙️ Overriding Parameters Dynamically

To run tests with different loads or target environments without modifying the `.jmx` file directly, use **JMeter properties** (`-J` flag) inside your test plan execution line:

```bash
jmeter -n -t ./scripts/main_load_test.jmx -l ./results/output.jtl \
  -Jhost=://example.com \
  -Jthreads=20 \
  -Jrampup=20 \
  -Jduration=1200
```

---

### Examples of CLI runlines: 

  -- Basic command line for running the test. Download the folder and replace the [MainFolderPath] in following command line.
jmeter -n -t "[MainFolderPath]\Assurity\Scripts\Jmeter_AssuritySPE.jmx" -q "[MainFolderPath]\Assurity\Config\user.properties" -l "[MainFolderPath]\Assurity\Test_Results\Test_Results.jtl"

  -- For absolute path use this commandline. Download the folder and replace the [MainFolderPath] in following command line.
jmeter -n -t "[MainFolderPath]\Assurity\Scripts\Jmeter_AssuritySPE.jmx" -q "[MainFolderPath]\Assurity\Config\user.properties" -l "[MainFolderPath]\Assurity\Test_Results\Test_Results.jtl" -e -o "<Folder>\Assurity\Test_Results\Test_Results_HTML" -Jthreads=5 -Jrampup=5 -Jduration=120 -JTPS=0.1

  -- For relative path use this commandline. Download the folder navigate to "..\apache-jmeter-4.0\bin" folder before running the following command line.
jmeter -n -t "..\..\Assurity\Scripts\Jmeter_AssuritySPE.jmx" -q "..\..\Assurity\Config\user.properties" -l "..\..\Assurity\Test_Results\Test_Results.jtl" -e -o "..\..\Assurity\Test_Results\Test_Results_HTML" -Jthreads=5 -Jrampup=5 -Jduration=60 -JTPS=0.167

  -- Clear the Results folder.
del /q /f /s "..\..\Test_Results\*"

```
📈 Perfomance Test Report

### How to Access Reports
1. Navigate to the "[MainFolderPath]\Assurity\Test_Results\Test_Results_HTML".
2. View html file for local graphic charts and breakdown matrices in any browser.
3. Performance Report & observations from the last test execution run are as below.
 - Assumptions:
        . Add this to Jmeter.properties files:
                  # Print field names as first line in CSV
                  jmeter.save.saveservice.print_field_names=true
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================================================
             API PERFORMANCE SUMMARY REPORT   - Test Execution 10.06.2026                        
================================================================================
Test Duration: 1 Minute
Concurrent Users: 5 Virtual Users
Ramp-up Duration: 5 seconds - 1 user per second
Total Requests: 11
Throuput: 10 Calls per minute
API: https://api.tmsandbox.co.nz/v1/Categories/6327/Details.json?catalogue=false 
--------------------------------------------------------------------------------

Label                  # Samples  Avg(ms)  Min(ms)  Max(ms)  Median STD.Dev   Err%   Throughput  Recv_KB/s  Sent_KB/s  90% Line  95% Line
------------------------------------------------------------------------------------------------------------------------------------------
GET 01_Txn_API             11      1796.91   736     3655    1746    803.12   9.09%    16.4/min       0.80      0.10       2689     2689  
------------------------------------------------------------------------------------------------------------------------------------------
TOTAL (Aggregate)          11      1796.91   736     3655    1746    803.12   9.09%    16.4/min       0.80      0.10       2689     2689  

================================================================================
                               KEY OBSERVATIONS                                 
================================================================================
1. CRITICAL BOTTLENECK: 
   - Endpoint: GET 01_Txn_API
   - Issue: Unacceptable response times (Avg: 1796.91ms, 90%ile: 2689ms).
   - High Standard Deviation (803.12) indicates heavy performance instability.
   - Action required: Fine tune API and re-validate the performance till acceptable levels are achieved.

2. SERVICE FAILURE ERROR:
   - Endpoint: GET 01_Txn_API
   - Issue: 9.09% error rate detected. 
   - Action Required: Inspect response for Category ID 6331 as text check failed by returning  "CanRelist": false. 

3. BANDWIDTH CONSUMPTION:
   - Endpoint: GET 01_Txn_API
   - Issue: No concerns
   - Action Required: NA.

4. NFRs VALIDATION:
   - NFR-01 met expectation. Default value is used from the script is parameter is omitted.
   - NFR-02 met expectation. Default value is used from the script is parameter is omitted.
   - NFR-03 met expectation. Default value is used from the script is parameter is omitted.
   - NFR-04 expectation not met. 90%ile is not under 500ms.

4. SCRIPT VALIDATION:
   - Response status asserted.
   - Parameter Category check implemented.
   - Text Check: "CanRelist": true is validated in every request.
   - All the Promotion IDs and respective Prices per Category ID are printed in Test_Response_Captured_Parameters.csv under Test_Results folder.
================================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






