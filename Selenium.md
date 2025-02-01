# QA Selenium Automation with Python

## Objective
Create a Selenium automation script in Python to validate search functionality on the **Selenium Playground** website.

> [!NOTE]
> **Deliverables:**
> 1. A Python script (`qa_selenium_test.py`) that:
>    - Navigates to the [Selenium Playground Table Search Demo](https://www.lambdatest.com/selenium-playground/table-sort-search-demo).
>    - Locates and interacts with the search box to search for "New York".
>    - Validates that the search results show **5 entries out of 24 total entries**.
> 2. A brief **README** explaining the approach and how to run the script.
> 3. Any additional setup instructions (e.g., local environment, dependencies, drivers etc).

> [!TIP]
> Use Python's `pytest` framework to structure your test cases.

> [!IMPORTANT]
> - **Environment Setup:** Follow good coding practices and ensure the script is compatible with the latest stable Selenium version.
> - **Browser Compatibility:** Test with at least one major browser (e.g., Chrome, Firefox).

> [!CAUTION]
> - **Assertions:** Ensure all validations use robust assertion statements.
> - **Code Quality:** Follow PEP8 standards for Python code.
>
> import time
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager

# Test class
class TestTableSearch:

    @pytest.fixture(scope="class")
    def setup(self):
        # Setup for running tests on Chrome browser
        chrome_options = Options()
        chrome_options.add_argument("--headless")  # Run in headless mode (optional)
        chrome_options.add_argument("--disable-gpu")
        
        # Setup WebDriver (using webdriver-manager to manage drivers automatically)
        driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
        driver.maximize_window()
        driver.get("https://www.seleniumeasy.com/test/table-search-demo.html")
        
        # Return driver to use in test method
        yield driver
        
        # Teardown after tests
        driver.quit()

    def test_search_for_new_york(self, setup):
        driver = setup

        # Locate search box and enter "New York"
        search_box = driver.find_element(By.ID, "task-table-filter")
        search_box.send_keys("New York")
        time.sleep(2)  # Wait for results to be updated

        # Validate 5 results are shown
        rows = driver.find_elements(By.XPATH, "//table[@id='task-table']/tbody/tr")
        assert len(rows) == 5, f"Expected 5 entries, but found {len(rows)}"

        # Validate the total number of entries is 24
        total_entries = driver.find_element(By.XPATH, "//div[@class='col-sm-12 col-md-6 text-right']").text
        assert "24" in total_entries, f"Expected total entries to be 24, but found {total_entries}"



**Good luck!**
