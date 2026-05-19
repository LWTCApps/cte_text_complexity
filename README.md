# CTE Text Complexity Analysis Dashboard & Instructor Tools
## Maintenance & Configuration Manual

Welcome to the documentation for the **CTE Text Complexity Analysis Platform**. This application is built as a highly responsive single-page system for **Collier County Workforce Education** to track, map, and mitigate reading barrier thresholds across Career and Technical Education (CTE) programs.

The system links reading analytics (Lexile data, FLDOE standards, and text barriers) with clear, printable lesson planning templates for instructors.

---

## Table of Contents
1. [File Directory Structure](#1-file-directory-structure)
2. [Updating Program Data (`Program_Descriptions.js`)](#2-updating-program-data-program_descriptionsjs)
3. [Modifying Table Hover Descriptions (`Label_Descriptions.js`)](#3-modifying-table-hover-descriptions-label_descriptionsjs)
4. [Customizing Chart Colors & Metrics](#4-customizing-chart-colors--metrics)
5. [Managing Image Samples & Full-Scale Lightbox](#5-managing-image-samples--full-scale-lightbox)
6. [Critical Design Rules](#6-critical-design-rules)

---

## 1. File Directory Structure

The system runs entirely client-side. There are no databases or web servers required; it runs instantly by double-clicking `index.html`.

```text
├── index.html                           # Main structure, table logic, and layout styling
├── text/
│   ├── Program_Descriptions.js          # Core data file for all academic/vocational programs
│   └── Label_Descriptions.js            # Glossary dictionary for matrix hover tooltips
└── Images/
    └── Program_Text_Examples_images/    # Reading sample images named as Program_Name_X.png

2. Updating Program Data (Program_Descriptions.js)
All program cards, matrix grid values, searches, and instructor templates are generated dynamically from the PROGRAMS array array inside text/Program_Descriptions.js.

The Program Schema
When adding a new program or updating an existing entry, make sure it matches this format:

"accounting_operations": {
    "name": "Accounting Operations",
    "sector": "Business",
    "lexile": "1180L–1280L",
    "lexile_mid": 1230,
    "grade_band": "12.2",
    "cognitive_load": "Medium",
    "fast_ela": "Level 4 (proficient–strong)",
    "act": "20–22",
    "sat": "500–550",
    "barrier_pct": "30–40%",
    "workforce_rating": 5,
    "risk": "Medium",
    "fldoe_min": "Grade 9",
    "gap": "Moderate Gap",
    "main_challenge": "Interpreting procedural and regulatory texts that rely on precise terminology, sequential logic, and numerical accuracy.",
    "learning_profile": "Detail-oriented, analytical learners who prefer clear rules, definitions, and step-by-step explanations. Students who enjoy math-adjacent thinking and structured processes tend to excel.",
    "instructional_implications": "Students may focus on individual definitions but fail to connect how equity, retained earnings, and capital changes interact across financial statements. Model sequential accounting logic and connect text to numerical examples.",
    "strategies": ["Frayer Model Vocabulary Cards", "Flowcharting equity change steps", "Think-Aloud Modeling with financial statements", "Guided Annotation of cause-and-effect phrases", "Paired Explanation using real/simulated business examples"],
    "text_source": "Accredited Business Accountant Prep Course (Equity & Retained Earnings section)",
    "placement": "Text is dense with financial terminology and sequential logic. Learners must interpret multi-step accounting processes. Reading requires precision with abstract concepts.",
    "image": "images/accounting_operations.jpg",
    "sample_image": "images/Program_Text_Examples_images/Accounting_Operations_1.png, images/Program_Text_Examples_images/Accounting_Operations_2.png, images/Program_Text_Examples_images/Accounting_Operations_3.png"
  },

3. Modifying Table Hover Descriptions (Label_Descriptions.js)
Hovering over table column headers displays descriptive text derived directly from text/Label_Descriptions.js.

The file maps structural keys like this:

const labelData = {
  "Program_Name": {
    "name": "Program Name",
    "description": "Identifies the specific postsecondary CTE or Adult Education program being analyzed."
  },
  "Redabilicty_score/Lexile_range": {
    "name": "Readability Score/Lexile Range",
    "description": "Indicates the readability score or Lexile range of the text..."
  }
};

To change the text that displays when a user hovers over a table column, modify the text inside the .description property for that column's key.

4. Customizing Chart Colors & Metrics
The application reads colors directly from your custom CSS variables using JavaScript. This ensures your dashboard charts stay synced with your site theme automatically.

const COMPLEXITY_COLORS = {
  low: getComputedStyle(document.documentElement).getPropertyValue('--green-light').trim(),
  medium: getComputedStyle(document.documentElement).getPropertyValue('--yellow').trim(),
  mediumHigh: getComputedStyle(document.documentElement).getPropertyValue('--orange').trim(),
  high: getComputedStyle(document.documentElement).getPropertyValue('--red').trim(),
  severe: getComputedStyle(document.documentElement).getPropertyValue('--dark-red').trim()
};

Chart 1: Lexile Range (chartLexile)Evaluated top-to-bottom based on the midpoint of the text's Lexile score:
Midpoint >= 1350 -> Severe (--dark-red)
Midpoint >= 1280 -> High (--red)
Midpoint >= 1250 -> MediumHigh (--orange)
Midpoint >= 1160 -> Medium (--yellow)
Below 1160 -> Low (--green-light)

Chart 2: Program Reading Barrier (chartBarrier)
Evaluated based on the percentage of the course that acts as a reading barrier:
Percentage > 55% -> Severe (--dark-red)
Percentage 47.5% - 55% -> High (--red)
Percentage 40% - 47.4% -> MediumHigh (--orange)
Percentage 30% - 39.9% -> Medium (--yellow)
Below 30% -> Low (--green-light)5. 

5. Managing Image Samples & Full-Scale Lightbox

The system looks for up to 10 textbook text samples inside the target folder 
(Images/Program_Text_Examples_images/).

Naming Conventions

The application dynamically replaces spaces with underscores based on the Program Name to locate files:

Program Name: Automotive Service Technology
Image targets: Automotive_Service_Technology_1.png, Automotive_Service_Technology_2.png, etc.

Formats & Fallback Engine

If a program uses format styles other than .png, the browser executes an automatic fallback chain loop:

$$\text{.png} --> \text{.jpg} --> \text{.gif}$$

If the file doesn't exist under any of these extensions, the thumbnail will hide itself cleanly without creating layout issues.

Lightbox Function

Clicking a thumbnail switches the primary preview display. Clicking the main preview image opens the full-screen lightbox window (#lightbox-overlay). Clicking anywhere on the dark backdrop minimizes it instantly.


6. Critical Design Rules

When performing maintenance updates inside index.html, do not alter these features to keep layouts stable:

1. Sticky Matrix Table Headers: The sticky headers require the parent wrapper container to have overflow: visible !important; assigned in the CSS styling. Changing this to hidden or scroll will break the header locking behavior.

2. Chart Heights: All dashboard graphics have maintainAspectRatio: false enabled. To change their dimensions on screen layouts safely, alter the height constraints directly on their HTML structural parent wrappers (height: 400px; width: 100%;).

3. Instructor Templates: The lesson planning generator engine explicitly updates an element layout matching id="modal-body". Ensure no new sections or lightboxes duplicate or reuse this ID string to prevent system errors.

