# Notes

## Locating Elements

- find_element_by_id
- find_element_by_name
- find_element_by_xpath
- find_element_by_link_text
- find_element_by_partial_link_text
- find_element_by_tag_name
- find_element_by_class_name
- find_element_by_css_selector
To find multiple elements (these methods will return a list):
- find_elements_by_name
- find_elements_by_xpath
- find_elements_by_link_text
- find_elements_by_partial_link_text
- find_elements_by_tag_name
- find_elements_by_class_name

## Explicit Waits
An explicit wait is a code you define to wait for a certain condition to occur before proceeding further in the code. The extreme case of this is time.sleep(), which sets the condition to an exact time period to wait. There are some convenience methods provided that help you write code that will wait only as long as required. WebDriverWait in combination with ExpectedCondition is one way this can be accomplished.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
try:
    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
    )
finally:
    driver.quit()
```
This waits up to 10 seconds before throwing a TimeoutException unless it finds the element to return within 10 seconds. WebDriverWait by default calls the ExpectedCondition every 500 milliseconds until it returns successfully. A successful return is for ExpectedCondition type is Boolean return true or not null return value for all other ExpectedCondition types.


**Expected Conditions**

There are some common conditions that are frequently of use when automating web browsers. Listed below are the names of each. Selenium Python binding provides some convenience methods so you don’t have to code an expected_condition class yourself or create your own utility package for them.

- title_is
- title_contains
- presence_of_element_located
- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present

**Custom Wait Conditions**

You can also create custom wait conditions when none of the previous convenience methods fit your requirements. A custom wait condition can be created using a class with __call__ method which returns False when the condition doesn’t match.

```python 
class element_has_css_class(object):
  """An expectation for checking that an element has a particular css class.

  locator - used to find the element
  returns the WebElement once it has the particular css class
  """
  def __init__(self, locator, css_class):
    self.locator = locator
    self.css_class = css_class

  def __call__(self, driver):
    element = driver.find_element(*self.locator)   # Finding the referenced element
    if self.css_class in element.get_attribute("class"):
        return element
    else:
        return False

# Wait until an element with id='myNewInput' has class 'myCSSClass'
wait = WebDriverWait(driver, 10)
element = wait.until(element_has_css_class((By.ID, 'myNewInput'), "myCSSClass"))
```

## Implicit Waits
An implicit wait tells WebDriver to poll the DOM for a certain amount of time when trying to find any element (or elements) not immediately available. The default setting is 0. Once set, the implicit wait is set for the life of the WebDriver object.
```python
from selenium import webdriver

driver = webdriver.Firefox()
driver.implicitly_wait(10) # seconds
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```

## Page Objects
A page object represents an area in the web application user interface that your test is interacting.

Benefits of using page object pattern:

- Creating reusable code that can be shared across multiple test cases
- Reducing the amount of duplicated code
- If the user interface changes, the fix needs changes in only one place