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
- Step 1.4, the line of the failed check and the line Tests:
- Step 1.5, target chosen for Submit, and why the recorded one was fragile:
- Step 1.6, commands changed, added or deleted after recording:
- Step 1.6, the pizza names of run 1 and run 2, from the lines info: echo:
- Step 1.6, the line Tests: of both runs:
- Step 1.7, one line of QP01customerTest.java that needs rewriting, and why:

## Part 4: Cypress

- node --version, npm --version and npx cypress version:
- Step 4.3, failure message for the wrong title:
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
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
