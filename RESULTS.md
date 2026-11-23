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
- Step 4.8, run 2, result, duration and pizza name: All specs passed, 2 of 2 tests (pizza-scenario.cy.js 00:08, web-form.cy.js 00:02, total 00:10), `Lab Pizza 20261123-160000` (tool=Scissors, newest in the admin list). The folders cypress/videos (2 videos) and cypress/screenshots (3 pictures) hold the evidence of this run
- Step 4.9 (optional), browser and result: not done

## Part B

1. In Step 0.4, `$$("#floating_filled").length` returned 3, because Max Calories per Slice, Min Number of Toppings and Max Number of Toppings all share the id `floating_filled`, so the id cannot tell them apart. The Selenium IDE recording even picked `id=floating_filled` for the calories box, which only worked because that box happens to come first. The tests therefore find the label text and step to its input, with `xpath=//label[normalize-space()='Max Calories per Slice']/../input` in Part 1 and `.siblings('input')` in Part 4. I would give each box its own unique id, such as `id="max-calories"`, `id="min-toppings"` and `id="max-toppings"`, or a `data-testid` attribute. A tester could then use a short locator like `#max-calories` that keeps working when the label text, the language or the layout changes, while the label-based XPath breaks as soon as someone rewords the label.

2. Step 7 is a consistency check: the oracle is the system itself, comparing the tool that the recommendation showed in step 4 with the tool that the admin list stored for the same pizza. In my Cypress run 2, step 4 gave Scissors and the admin line read `Lab Pizza 20261123-160000 (tool=Scissors, ingredients_number=6)`, so the two agreed. It would catch a fault where the server saves a different pizza from the one it sends to the customer, for example if it picks a new random tool when it stores the pizza, or if the admin list maps the wrong field into `tool=`. It would also catch a pizza that is never saved at all, because `cy.contains('li', pizzaName)` would not find the name and the test would fail.

3. A check could read the answer of `POST /api/pizza` with `cy.intercept`, which in Step 4.6 already holds a field `"vegetarian": true`, and assert `expect(response.body.vegetarian).to.equal(true)`. That checks the server's own flag, so a stronger check would also look at the ingredients themselves: in my run by hand they were Roma tomatoes, Mozzarella, Feta cheese, Buffalo mozzarella, Broccoli and Extra virgin olive oil. The test would need a reference list of which ingredients are vegetarian, from the specification, from the QuickPizza ingredients API, or from a list that the tester writes. The page itself only shows the ingredient names and no vegetarian mark, so a check on the page alone would need QuickPizza to show that flag, for example a line `Vegetarian: yes` in the answer.

4. Selenium IDE does not wait by itself, so every element that QuickPizza draws late needed a `wait for element visible` or `wait for element present` with 10000 ms, eight waits in a 29-command test. Recording QP-01 took only a few minutes, but the recording had 18 commands full of stray clicks, a double-click and mouseOver/mouseOut, and on the web form a recorded click on a label failed with `element click intercepted` until I deleted it, so cleaning up took longer than recording. Its report is the Jest output of selenium-side-runner: one tick per test and a clear message such as `Actual value 'Received!' did not match 'Received'`, but surrounded by deprecation warnings, with no screenshots or video. Cypress retried every `cy.get` and `should` for up to 10 seconds, so I wrote no wait except `cy.wait('@pizza')` for the request, and I could build QP-01 one block at a time with a run after each block. Its report was the most readable: a table with the duration of each spec (00:08 for QP-01, 00:10 to 00:11 for both specs), a Command Log with a snapshot for each command, three screenshots and an mp4 video, and a failure screenshot was saved automatically in Step 4.3.

5. I would use Cypress for the automated UI tests of Project 2. First, its automatic retrying handled every late element of QuickPizza, so QP-01 passed in both full runs without any fixed pause, which is exactly the rule that Project 2 sets for waits, while in Selenium IDE I had to add each wait by hand. Second, it produces the evidence that Project 2 asks for by itself: with `video: true` and `cy.screenshot`, both runs left a video and three screenshots without any screen recording. It also lets one test check the server's answer with `cy.intercept` and reuse a staff login through a custom command like `adminLogin`. Its limit is one browser per test, so a test with a staff user and a customer has to log out and log in again in the same browser.

6. Part 1: The recorder chose `css=.btn` for Submit. `btn` is a Bootstrap style class that says how a button looks, many buttons can share it, and `css=.btn` returns the first element with that class, so a new button above Submit would make the test click the wrong button without any error. I replaced it with `xpath=//button[@type='submit']`, which describes what the button does. That attribute does not change when the layout, colours or text of the button change, so the target keeps working, and the test passed with it (`Tests: 1 passed, 1 total`).

7. Part 4: Cypress clears the cookies, local storage and aliases before each test. QuickPizza runs on several servers, and a cookie keeps one browser on the same server, while the admin list only shows the pizzas of that server. If steps 6 and 7 were a second test, it would start with no cookie, could land on another server, and would not find the pizza from steps 1 to 5, so it could fail even though QuickPizza works. The second test would also lose `pizzaName` and the alias `@tool`, which only live inside the test that created them, so it would not know which pizza and which tool to look for. Keeping QP-01 as one test keeps the session, the pizza name and the tool together, and that is why my runs found `Lab Pizza 20261123-155522` and `Lab Pizza 20261123-160000` in the admin list.
