# ui-lab

Automated UI testing lab for ITCS386 Software Verification and Validation.
The tests run against Selenium's web form and QuickPizza (https://quickpizza.grafana.com).

Tool parts that I chose: **Part 1 (Selenium IDE)** and **Part 4 (Cypress)**.
Results of every step and the reflection are in [RESULTS.md](RESULTS.md).

## selenium-ide

`ui-lab.side` is the Selenium IDE project with two tests: `web form` and `QP-01 customer` (QP-01 steps 1 to 5).
`QP01customerTest.java` is the Java JUnit export of `QP-01 customer`. It needs ChromeDriver, with the variable `CHROMEDRIVER` set as in Step 0.3.

Run the tests from the folder `selenium-ide` (Windows PowerShell):

```powershell
$env:Path = (Split-Path $env:CHROMEDRIVER) + ";" + $env:Path
npx --yes selenium-side-runner@4.0.13 ui-lab.side
```

## cypress

A Cypress 16.2.1 project with two specs: `web-form.cy.js` and `pizza-scenario.cy.js` (the whole of QP-01, steps 1 to 7).
`cypress/support/commands.js` holds the custom command `adminLogin`. `cypress/videos` and `cypress/screenshots` hold the evidence of a full run.

Run the tests from the folder `cypress`, after `npm install` once:

```powershell
npx cypress run --browser chrome
```
