# SpecFlow + Selenium Test

## Usage

1. Ensure you download and setup the path for ChromeDriver.
1. Open the solution in Visual Studio 2022
2. Execute available tests (Test->Run All Tests)

# VAT calculator

## Background info
Calkoo introduced a simple online VAT calculator for calculating tax on their website:

[http://www.calkoo.com/?lang=3&page=8](http://www.calkoo.com/?lang=3&page=8)

## User story
As a traveller, I want to calculate the various purchase/tax amounts to verify that my shop 
receipt is correct, and the merchant correctly charged the VAT for my recently bought items.


## Acceptance Criteria
- User must select a country which applies VAT scheme
- User must be able to choose a valid VAT rate for the selected country
- One of the following amounts are supported as an input:
	- Net
	- VAT
	- Gross
- Amounts can be entered with maximum 2 decimal digit precision
- Given all mandatory fields (country, vat rate, one of the amounts) are provided, 
the website will calculate and show the other 2 amounts which were not 
provided originally as an input value
- The API provides an error with meaningful error message, in case of:
	- negative input
	- amount >999.999.999

## Technical Requirements
- Please document test cases (manual) that come to your mind.
- In case you encounter bugs - which one and how would you report them (provide example)
- In case there are open questions regarding requirements/acceptance criteria:
	- Document the questions you would like to ask/clarify with End-users/business/product owner based on the given information.
- Please identify at least two test cases for automation:
	- explain why you would choose specifically those
	- automate them using Cypress, Robot or Selenium (if having experience)

---

## Test Scenarios

### 1 - Basic

1. Navigate to calculator
2. Select Country: UK
3. Select Rate: 20%
4. Input value into `Price without VAT` (Net) input: `10.00`
5. Check value of `Value-Added Tax` (VAT): `2.00`
6. Check value of `Price incl. VAT` (Gross): `12.00`

### 2 - Rounding

1. Navigate to calculator
2. Select Country: UK
3. Select Rate: 20%
4. Input value into `Price without VAT` (Net) input: `9.99`
5. Check value of `Value-Added Tax` (VAT): `2.00`
6. Check value of `Price incl. VAT` (Gross): `11.99`

Note: Rounding is right `9.99 x 0.200 = 1.998 ~ 2.00`

### 3 - Precision

1. Navigate to calculator
2. Select Country: UK
3. Select Rate: 20%
4. Input value into `Price without VAT` (Net) input: `9.999`
5. Check value of `Value-Added Tax` (VAT): `2.00`
6. Check value of `Price incl. VAT` (Gross): `12.00`

[Bug] the Net price should be `11.99`: `9.999 + 2.00 = 11.99`

### 4 - Negative Value (check for error)

1. Navigate to calculator
2. Select Country: UK
3. Select Rate: 20%
4. Input value into `Price without VAT` (Net) input: `-10.00`
5. Check value of `Value-Added Tax` (VAT): `-2.00`
6. Check value of `Price incl. VAT` (Gross): `-12.00`

Note: No warning is shown when a negative input is used (all fields)


### 5 - Text value (check for error)

1. Navigate to calculator
2. Select Country: UK
3. Select Rate: 20%
4. Input value into `Price without VAT` (Net) input: `test`
5. Check value of `Value-Added Tax` (VAT): `NaN`
6. Check value of `Price incl. VAT` (Gross): `NaN`

Note: No warning is shown when a non-numeric input is used (all fields)


## Notes

- Pressing `Return` on the price input will reset the calculator.
- Pie chart does not work for negative numbers: `Negative values are invalid for a pie chart.`
- Inserting `999999999` (maximum value) in the Net input leads to truncated output on VAT and Gross inputs.
	- `199 999 999,8` (VAT)
	- `1 199 999 998,8` (Gross)
- When testing the input of VAT values, we can not get the prices ending in `.99`. E.g. `9.99` price w/o VAT, equates to `2.00` in VAT sum, but when inserting `2.00` VAT, we will never get `9.99` in price and instead we get `10.00`.

---

# Reference / Useful links

- [NUnit Documentation](https://docs.nunit.org/)
- [SpecFlow Documentation](https://docs.specflow.org/)
- [Cucumber Gherkin Documentation](https://cucumber.io/docs/gherkin/reference/)
- [Selenium Documentation](https://www.selenium.dev/documentation/webdriver/)
- [Chrome WebDriver Download](https://sites.google.com/chromium.org/driver/downloads)

## Advanced topics

- [Selenium Finders](https://www.selenium.dev/documentation/webdriver/elements/finders/)
- [Step Definitions](https://docs.specflow.org/projects/specflow/en/latest/Bindings/Step-Definitions.html)
- [SpecFlow Examples](https://docs.specflow.org/en/latest/Examples.html)
- [WebDriver Factory](https://thefriendlytester.co.uk/2014/02/webdriver-factory.html)

## Example projects

- [SpecFlowOSS/CalculatorSelenium](https://github.com/SpecFlowOSS/SpecFlow-Examples/tree/master/CalculatorSelenium)
	- Explained in [Selenium with Page Object Pattern](https://docs.specflow.org/projects/specflow/en/latest/ui-automation/Selenium-with-Page-Object-Pattern.html)
- [spoquality/SpecFlowSeleniumDemo](https://github.com/spoquality/SpecFlowSeleniumDemo)
	- **Beginner tutorial for functional automated web testing**
		[Part 1](https://medium.com/@amaya30/specflow-selenium-beginner-tutorial-for-functional-automated-web-testing-part-1-bf5c8fe53c3f)
		[Part 2](https://medium.com/@amaya30/specflow-selenium-beginner-tutorial-for-functional-automated-web-testing-part-2-d3a2ba3d7c2)


# TODO

- Add test scenario to validate reset button.
- Add support to multiple browsers.
- At runtime, compute the expected output values, based on the inputs for each scenario. This way we can have more generic tests and cover all the possibilities.