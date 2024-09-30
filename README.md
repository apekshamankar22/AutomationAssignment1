Automation Testing Framework for E-commerce Website
Objective
This document outlines the steps to create an automation framework/test suite to automate the search and add-to-cart functionalities for a sample e-commerce website. The tests will be executed using the latest Chrome browser.
Step 1: Test Scenarios
1. Search for a Non-Existing Product
•	Input: "abcd123"
•	Expected Outcome: "No results found" message should be displayed.
2. Search for an Existing Product
•	Input: "Monitor"
•	Expected Outcome: Product results should display "Monitor" on the page.
Step 2: Add to Cart Functionality
3. Add a Product to the Cart
•	Select the 4th product from the results.
•	Expected Outcome: The product should be added to the cart with correct quantity and price details.
4. Update Product Quantity
•	Change quantity to 4.
•	Expected Outcome: The cart should reflect the updated quantity and price.
Step 3: Remove Product from Cart
5. Remove Product
•	Expected Outcome: The cart should be empty.
Technology Stack
•	Programming Language: Python
•	Dependency Management: Pip using requirements.txt
•	Test Library: Pytest
•	UI Automation Tool: Selenium
Framework Requirements
1.	Environment Setup:
o	Use the latest version of Chrome.
o	Web URL: https://www.amazon.in/
2.	Framework Design:
o	Modular and reusable architecture.
o	Implement Page Object Model (POM).
o	Reporting mechanism for capturing test results.
3.	Code Quality:
o	Follow best practices for readability and maintainability.
o	Include comments.
o	Implement exception handling.
4.	Execution:
o	Automated execution of all tests.
o	Generate a report with pass/fail status.
5.	Submission:
o	Push code to a public GitHub repository.
o	Provide a README file with setup and execution instructions.
Sample Code Structure
bash
Copy code
/ecommerce_tests
│
├── /tests
│   ├── test_search.py
│   ├── test_cart.py
│
├── /pages
│   ├── base_page.py
│   ├── search_page.py
│   ├── cart_page.py
│
├── /utils
│   ├── report.py
│
├── requirements.txt
└── README.md
Sample Code
requirements.txt
plaintext
Copy code
selenium
pytest
base_page.py
python
Copy code
from selenium import webdriver

class BasePage:
    def __init__(self, driver):
        self.driver = driver
search_page.py
python
Copy code
from selenium.webdriver.common.by import By
from base_page import BasePage

class SearchPage(BasePage):
    SEARCH_INPUT = (By.ID, "twotabsearchtextbox")
    SEARCH_BUTTON = (By.ID, "nav-search-submit-button")
    NO_RESULTS_MESSAGE = (By.XPATH, "//span[contains(text(), 'no results')]")
    
    def search_product(self, product_name):
        self.driver.find_element(*self.SEARCH_INPUT).send_keys(product_name)
        self.driver.find_element(*self.SEARCH_BUTTON).click()

    def get_no_results_message(self):
        return self.driver.find_element(*self.NO_RESULTS_MESSAGE).text
cart_page.py
python
Copy code
from selenium.webdriver.common.by import By
from base_page import BasePage

class CartPage(BasePage):
    ADD_TO_CART_BUTTON = (By.ID, "add-to-cart-button")
    CART_QUANTITY = (By.XPATH, "//input[@name='quantity']")
    
    def add_product_to_cart(self):
        self.driver.find_element(*self.ADD_TO_CART_BUTTON).click()

    def update_quantity(self, quantity):
        quantity_input = self.driver.find_element(*self.CART_QUANTITY)
        quantity_input.clear()
        quantity_input.send_keys(quantity)
test_search.py
python
Copy code
import pytest
from selenium import webdriver
from pages.search_page import SearchPage
from pages.cart_page import CartPage

class TestEcommerce:
    @pytest.fixture(scope="class")
    def setup_class(self):
        self.driver = webdriver.Chrome()
        self.driver.get("https://www.amazon.in/")
        yield
        self.driver.quit()

    def test_search_non_existing_product(self, setup_class):
        search_page = SearchPage(self.driver)
        search_page.search_product("ld345tsxslfer")
        assert "no results" in search_page.get_no_results_message().lower()

    def test_search_existing_product(self, setup_class):
        search_page = SearchPage(self.driver)
        search_page.search_product("Laptop")
        # Additional assertions can be added here
README.md
markdown
Copy code
# E-commerce Automation Tests

## Setup Instructions

1. Clone the repository.
2. Install the dependencies:
   ```bash
   pip install -r requirements.txt
3.	Run the tests:
bash
Copy code
pytest tests
Test Cases
•	Search for a non-existing product.
•	Search for an existing product.
•	Add product to cart.
•	Update product quantity in cart.
•	Remove product from cart.
CI/CD Integration
For bonus points, consider integrating with Jenkins or GitHub Actions for automated execution on code push.
Conclusion

This framework provides a solid foundation for automating the search and add-to-cart functionalities of an e-commerce website. By following the outlined structure and code samples, you can create a scalable and maintainable test suite.

