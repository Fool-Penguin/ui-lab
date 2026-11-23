# UI lab results

Tool parts that I chose: Part 1 (Selenium IDE) and Part 4 (Cypress)

## Part 0

- Step 0.1, Chrome version: 157.0.8103.79 (Official Build) (64-bit)
- Step 0.3, ChromeDriver version: 157.0.8103.79
- Step 0.4, locator of Pizza, Please!: `button[name='pizza-please']` (CSS, by name), finds 1
- Step 0.4, locator of Excluded tools: `select` (CSS, the only select on the page), finds 1
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS, by id), finds 1
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS, tag and id), finds 1. After Pizza, Please! the answer heading also has id pizza-name, so `#pizza-name` alone finds 2
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261123-0100, tool Scissors, 500 calories per slice (dough Thick, 6 ingredients)

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | Heading reads Looking to break out of your pizza routine? | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields show 600, Knife selected, vegetarian ticked, Lab Pizza 20261123-0100 (replaced the default 1000) | Pass |
| 4    | Answer shows Name: Lab Pizza 20261123-0100, Tool: Scissors (not Knife), Calories per slice: 500 (600 or fewer). The answer appeared a moment after the click | Pass |
| 5    | Page says Please log in first. | Pass |
| 6    | Signed in as admin/admin (boxes were already filled), page shows Latest pizza recommendations | Pass |
| 7    | Line "Lab Pizza 20261123-0100 (tool=Scissors, ingredients_number=6)", same tool as step 4. It was 3rd in the list, not newest, because other users' pizzas arrived after it | Pass |

## Part 1: Selenium IDE

- Firefox version and Node.js version: Firefox 160.0.1 (64-bit), Node.js v24.14.1 (npm 11.11.0), Selenium IDE 3.17.4
- Step 1.4, the line of the failed check and the line Tests: `Actual value 'Received!' did not match 'Received' during assertText:id=message:Received`, `Tests: 1 failed, 1 total`
- Step 1.5, target chosen for Submit, and why the recorded one was fragile: `xpath=//button[@type='submit']`. The recorded `css=.btn` uses a style class that any button can share, and it finds the first element with that class, so a new button above Submit would make the test click the wrong one. `type='submit'` says what the button does, so it keeps working when the layout, colours or text change. Run after the change: `Tests: 1 passed, 1 total`
- Step 1.6, commands changed, added or deleted after recording (the recording had 18 commands, the finished test has 29):
  - Changed: `open /` to the full address `https://quickpizza.grafana.com/`; the Advanced click `css=.float-right` to `xpath=//label[.//span[normalize-space()='Advanced']]`; `type id=floating_filled 600` to the label-based target `xpath=//label[normalize-space()='Max Calories per Slice']/../input` (three boxes share that id); `addSelection css=.ml-4` to `css=select`; `type id=pizza-name Lab Pizza test` to `css=input#pizza-name` with `${pizzaName}`.
  - Deleted: `setWindowSize 550x691`; the stray click `css=.w-9`; five clicks and one double-click on the label `css=.relative:nth-child(1) > .absolute`; `click id=pizza-name` before typing; `mouseOver` and `mouseOut` on Pizza, Please!.
  - Added: waits for the heading, the four advanced fields, the Knife option, the answer and the rate message (`wait for element visible` / `wait for element present`, 10000 ms); `execute script` + `echo` that build and print the pizza name with the date and time; the checks `assert text` (heading, Name:, rate message), `assert value` (600, pizza name), `assert selected label` (Knife), `assert checked` (vegetarian), `assert not text` (Tool: Knife), and `store text` + `execute script` + `assert` for calories of 600 or fewer.
  - Also added the test QP-01 customer to the Default Suite, so that selenium-side-runner plays it.
- Step 1.6, the pizza names of run 1 and run 2, from the lines info: echo: run 1 `Lab Pizza 20261123-013628`, run 2 `Lab Pizza 20261123-013711`
- Step 1.6, the line Tests: of both runs: run 1 `Tests: 2 passed, 2 total` (QP-01 customer 4990 ms), run 2 `Tests: 2 passed, 2 total` (QP-01 customer 4846 ms)
- Step 1.7, one line of QP01customerTest.java that needs rewriting, and why: line 66, `driver.findElement(By.xpath("//label[normalize-space()='Max Calories per Slice']/../input")).sendKeys("600");`. selenium-side-runner clears a box before it types, but WebDriver's `sendKeys` only adds keys, and the page fills this box with 1000, so the box would hold 1000600. It needs a `.clear()` before `sendKeys("600")`. Lines 91 and 98 are wrong too: the export put `vars.get("pizzaName").toString()` inside the quotes, so line 91 does not compile (unescaped quotes) and line 98 compares the page with the literal text `Name: vars.get("pizzaName").toString()` instead of the pizza name. Both should be `is("Name: " + vars.get("pizzaName").toString())` (and `is(vars.get("pizzaName").toString())` on line 91).

## Part 4: Cypress

- node --version, npm --version and npx cypress version: Node.js v24.14.1, npm 11.11.0, Cypress package 16.2.1 and binary 16.2.1 (Electron 41.7.0, bundled Node 24.15.0). Browsers found: Chrome 157.0.8103.79, Edge 158.0.4370.33, Firefox 160.0.1
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms`, `+ expected - actual`, `-'Web form'`, `+'Web Form'` (at web-form.cy.js:6:15, after 12 seconds, with the screenshot `Web form -- fills in the fields and sends the form (failed).png`)
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool Scissors, calories 325 (status 200; the request carried maxCaloriesPerSlice 600, and the page showed the same tool and calories)
- Step 4.7, the line of the admin list that shows your pizza: `Lab Pizza 20261123-155350 (tool=Scissors, ingredients_number=5) (newest)`
- Step 4.8, run 1, result, duration and pizza name: All specs passed, 2 of 2 tests (pizza-scenario.cy.js 00:08, web-form.cy.js 00:02, total 00:11), `Lab Pizza 20261123-155522` (tool=Pizza cutter, newest in the admin list)
- Step 4.8, run 2, result, duration and pizza name:
- Step 4.9 (optional), browser and result:

## Part B

1.

2.

3.

4.

5.

6.

7.
