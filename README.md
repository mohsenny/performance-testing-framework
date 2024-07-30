# Performance Testing Framework

This performance testing framework uses K6 to perform load tests on specified endpoints. The framework reads a JSON file to determine the load profile, including endpoints, request types, expected status codes, and expected response times. 

This framework requires almost no coding, as long as the load profile follows the [accepted format](https://github.com/mohsenny/performance-testing-framework/tree/main?tab=readme-ov-file#load-profile-explanation). That said, some [minor script adjustments](https://github.com/mohsenny/performance-testing-framework/tree/main?tab=readme-ov-file#adjusting-the-script) are still needed.

## Prerequisites
- [K6](https://k6.io/docs/getting-started/installation/)

## Installation

### MacOS

1. **Install Homebrew (if not already installed):**
   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

2. **Install K6**
    ```sh
    brew install k6
    ```

### Windows

1. **Install Chocolatey (if not already installed):**
    ```sh
    @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. **Install K6**
    ```sh
    choco install k6
    ```

## Project Structure

- `load_profile.json`: Defines the endpoints and load profile for the performance tests.
- `test_script.js`: K6 test script that reads the load profile and executes the tests.
- `run_tests.sh`: Shell script to execute the K6 test script.

## Usage

1. **Clone the repository:**
    ```sh
    git clone https://github.com/your-repository/performance-testing-framework.git
    cd performance-testing-framework
    ```

2. **Update `load_profile.json` with your endpoints and expected results:**

Check the `load_profile.json`

3. **Make the `run_tests.sh` script executable (macOS/Linux only):**
    ```sh
    chmod +x run_tests.sh
    ```

4. **Run the tests:**
    ```sh
    ./run_tests.sh
    ```
    On Windows, you can directly execute the `run_tests.sh` script using Git Bash or run the K6 command directly in Command Prompt or PowerShell:

    ```sh
    k6 run test_script.js
    ```

## Adjusting the Script

The script needs to be adjusted to include functions for each step defined in your `load_profile.json`. You need to add and export functions for each step in your `test_script.js`.

Example:

```javascript
export function GetPosts() {
    executeStep('GetPosts');
}

export function CreatePost() {
    executeStep('CreatePost');
}

export function GetComments() {
    executeStep('GetComments');
}
```

## Output
The test results will be displayed in the console, including:

- Status checks and response times for each request.
- Detailed information on any failed requests, including status codes and response times.

Example: 
<img width="888" alt="Screenshot 2024-07-30 at 10 21 43 AM" src="https://github.com/user-attachments/assets/e460e884-a145-4e28-a62d-984c1015c403">

## Load Profile explanation
```json

{
    "steps": [
      {
        "name": "GetPosts",                                     // Test scenario's name
        "location": "us-west-2",                                // The location from which the requests are sent from
        "endpoint": {                                           
          "url": "https://jsonplaceholder.typicode.com/posts",  // The URL under test
          "method": "GET",                                      // The method to call the API with
          "params": {},                                         // The request's payload
          "headers": {                                          // Any required headers
            "Authorization": "Bearer YOUR_AUTH_TOKEN"
          },
          "expectedStatusCodes": [200]                          // A list of accepted status codes
        },
        "vus": 1,                                               // The number of parallel Virtual Users; When "1" requsts are sent sequentially
        "iterations": 10,                                       // The number of requests per VU
        "expectedResponseTime": 200                             // Maximum resposne time 
      }
    ]
  }
```
