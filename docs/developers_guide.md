#️⃣ 1. Overview
AccessCheck analyzes any public webpage for four basic accessibility issues:
Missing image alt text


Missing form labels


Missing or empty link text


Missing or poorly structured headings


All results include HTML line numbers


The tool is built as a Flask web application, using:
requests for fetching HTML


BeautifulSoup (bs4) + lxml for parsing


Regular expressions (re) to inject line numbers


Bootstrap for frontend styling



#️⃣ 2. Project Structure (Final)
purva_project/
│
├── main.py                # Main Flask application (entry point)
├── requirements.txt           # Python dependencies
├── README.md                  # User Guide
│
├── templates/
│   └── index.html             # Front-end UI for the web app
│
├── docs/
│   ├── developers_guide.md    # Developer documentation  ← THIS FILE
│   └── AccessCheck_updated_specs.pdf  # Planning & spec documents
│
└── .venv/                     # Optional virtual environment


#️⃣ 3. Installation / Developer Setup
Developers should first read the User Guide (README.md) to run the basic app.
Additional Developer Setup
git clone <repo>
cd purva_project
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

No API keys are required.
 The app runs locally via:
python main.py


#️⃣ 4. Software Architecture
AccessCheck follows a simple 3-layer structure:
1. Presentation Layer
templates/index.html


Contains the UI:


URL input form


Bootstrap styling


Results section


2. Logic Layer
Flask route: index()


Orchestrates:


URL validation


Fallback from HTTPS → HTTP


Triggering the analysis


3. Analysis Layer (Core Engine)
Functions responsible for:
Function
Responsibility
add_line_numbers_to_html()
Injects line numbers into HTML as data-line attributes
get_line_number()
Extracts stored line numbers
check_img_alt()
Detects missing alt attributes
check_form_labels()
Detects inputs without labels
check_headings()
Checks presence of heading tags
check_links()
Finds links with missing text
run_accessibility_checks()
Pipeline: fetch → annotate → parse → run all checks


#️⃣ 5. Code Walkthrough (Developer Flow)
### 5.1 index() — Main Request Handler (Flask)
Flow when user presses “Analyze”:
Get user input URL


If no protocol → prepend https://


Try running an analysis on HTTPS


If HTTPS fails with:


403 Forbidden


HTTPSConnectionPool errors
 → automatically retry with HTTP


Render results in index.html


This handles the most common issue: sites blocking bots.

### 5.2 HTML Line Number Injection
HTML from the website is split into individual lines:
lines = html.splitlines()

For each line with a tag:
re.sub(r'<(\w+)([^>]*?)>', r'<\1\2 data-line="{i}">')

This allows:
Precise pinpointing of accessibility errors


Easy mapping to real HTML code


Limitations:
 Minified HTML may place many tags on one line → fewer line numbers.

### 5.3 Parsing & Checks (run_accessibility_checks())
The function:
Sends HTTP request with a browser-like user agent


Inserts line numbers


Parses with BeautifulSoup(lxml)


Runs all 4 checks sequentially


Returns list of dictionaries:


{
  "check": "Image alt text",
  "result": "Fail",
  "details": "Missing alt text on lines: 18, 34"
}


#️⃣ 6. Modules, Functions & Responsibilities
✔ add_line_numbers_to_html(html)
Adds data-line="{line_number}" to matching HTML tags.
✔ get_line_number(element)
Returns the stored HTML line number.
✔ check_img_alt(soup)
Fails if:
<img> has no alt


alt=""


✔ check_form_labels(soup)
Checks multiple labeling patterns:
<label for="id">


aria-label


aria-labelledby


Parent <label>


✔ check_headings(soup)
Fails only if zero headings exist.
✔ check_links(soup)
Fails if an anchor contains no visible text.
✔ run_accessibility_checks(url)
Full pipeline:
Requests webpage


Annotates HTML


Runs all checks


Returns results as list



#️⃣ 7. Error Handling & Known Issues
❗ HTTPS → HTTP fallback
Many sites block HTTPS requests from scripts.
 The code automatically switches.
❗ JavaScript-rendered pages
The app cannot read JS-loaded content because requests only fetches static HTML.
❗ Minified HTML
Sometimes many tags appear on a single line → line numbers may be less useful.
❗ Sites blocking bots
If both HTTPS & HTTP fail → error is displayed.

#️⃣ 8. Computational Considerations
Runtime: usually <1 second


Memory use is minimal (only static HTML storage)


Works best on documents < 2 MB


No concurrency (Flask dev mode is single-threaded)


Possible performance improvements:
Async HTML fetching


Caching repeated scans


Server-side rendering to handle large HTML files



#️⃣ 9. Future Work (Recommended Enhancements)
✔ Add WCAG 2.1 rules:
Color contrast rules


ARIA role validation


Keyboard navigation issues


✔ Add PDF/CSV report export
✔ Add real-time CLI mode
✔ Support JavaScript rendering
Using Selenium / Playwright.
✔ Add automated tests
Using pytest + Flask test client.
✔ Build as a pip-installable package
Restructure into:
accesscheck/
    __init__.py
    analyzer.py
    utils.py


#️⃣ 10. Ongoing Development Notes
If this project evolves:
Recommend:
Add unit tests


Keep run_accessibility_checks() independent from Flask


Convert checks into modular classes:


ImageAltCheck()


LabelCheck()


etc.


Code Extensibility Tip
Each test returns a dictionary.
 Future checks can easily follow the same structure.



