# HOME-TAKEAWAY-TASK-FOR-QA-INTERNSHIT-AT-VRITECH

--Code Explanation--

This script automates the initial registration flow of the Authorized Partner platform using Selenium WebDriver in Python.
The automation focuses on filling the registration form and handling OTP verification until the user is redirected to the Agency Details page.

1. Importing Required Libraries

The script starts by importing necessary Selenium modules:
webdriver → Launches and controls the browser.
By--Used to locate elements (XPath, ID, etc.).
WebDriverWait - Implements explicit waiting.
expected_conditions (EC) - Defines wait conditions like visibility or clickability.
TimeoutException - Handles timeout errors.
traceback - Prints detailed error logs.
time - Adds small execution delays when needed.
These imports ensure the script can interact with the browser reliably and handle errors properly.

2. Driver Setup and Initialization

Inside the test_signup_flow() function:

driver = webdriver.Chrome()
driver.maximize_window()
wait = WebDriverWait(driver, 30)


A Chrome browser instance is launched.
The window is maximized for better element visibility.
An explicit wait of 30 seconds is configured to handle dynamic loading.
The script then opens the registration URL:
driver.get("https://authorized-partner.vercel.app/register?step=setup")

3. Step 1 – Registration Form Automation

The script locates input fields using XPath and waits until they are visible before interacting:
Example:
first_name = wait.until(
    EC.visibility_of_element_located(
        (By.XPATH, "//label[contains(text(),'First')]/following::input[1]")
    )
)

This ensures:

-The element is present in DOM
-The element is visible
-The script does not fail due to loading delays

The script then enters test data using:

first_name.send_keys("Pradip")

This process is repeated for:

Last Name
Email
Phone
Password
Confirm Password

After filling all fields, it clicks the Next button using JavaScript execution:
driver.execute_script("arguments[0].click();", next_button)
JavaScript click is used to avoid issues such as overlapping elements or hidden UI layers.

4. OTP Verification Handling

Once Step 1 is submitted, the script waits for the OTP input field:
otp_input = wait.until(
    EC.visibility_of_element_located(
        (By.XPATH, "//input[@maxlength='6']")
    )
)
OTP is intentionally handled manually because:
It is dynamically generated.
It is sent externally (email or SMS).
Automating it would require backend/email access.
The script waits until 6 digits are entered:
wait.until(lambda d: len(otp_input.get_attribute("value")) == 6)

This ensures:
The script continues only after full OTP entry.
Then it clicks the Verify button.

5. Navigation Verification

After clicking Verify, the script confirms successful navigation:
wait.until(EC.url_contains("step=details"))

This validates that:

OTP verification was successful.
User is redirected to the next step (Agency Details).

6. Exception Handling

The script includes error handling using:
except TimeoutException:
This prevents abrupt crashes and prints detailed debugging information.
Using traceback.print_exc() provides a full error stack trace, making debugging easier.

7. Cleanup

In the finally block:
driver.quit()
The browser closes automatically after execution, ensuring no background processes remain active.
Automation Design Approach
Uses Explicit Waits instead of implicit waits for stability.
Avoids hardcoded delays where possible.
Uses XPath locators suitable for dynamic React-based UI.
Separates logical steps clearly (Step 1 → OTP → Redirect).
Implements basic exception handling for reliability.

This Approach Is Reliable Beacuase of following reasons:

Handles dynamic page loading properly.
Prevents timing-related failures.
Waits for specific UI conditions before interacting.
Uses JavaScript click to handle UI rendering issues.
