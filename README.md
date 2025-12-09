
# 🧩 AccessCheck – User’s Guide

**AccessCheck** is a simple web-based tool that analyzes any public webpage for basic accessibility issues.

It checks for:
- Missing image alt text  
- Missing form labels  
- Missing or weak link text  
- Missing or poorly structured headings  
- Line numbers showing exactly where issues occur  

This guide explains how to install, run, and use the tool.

---

## 📚 Table of Contents

1. [Requirements](#1-requirements)  
2. [Project Setup](#2-project-setup)  
3. [Installation Instructions](#3-installation-instructions)  
4. [How to Use AccessCheck](#4-how-to-use-accesscheck)  
5. [Screenshots](#5-screenshots)  
6. [Possible Errors and Fixes](#6-possible-errors-and-how-to-fix-them)  
7. [Limitations](#7-limitations)  
8. [Summary](#8-summary)

---

## 1. Requirements

- **Python:** 3.13.1  
- **Browser:** Chrome, Firefox, or Edge  
- **Internet connection:** Required for fetching webpages

---

## 2. Project Setup

Your project folder should look like this:

purva_project/
│
├── main.py
├── requirements.txt
├── README.md ← (this file)
├── templates/
│ └── index.html
├── docs/
│ └── (reports and specifications)
└── .venv/ (optional virtual environment)



---

## 3. Installation Instructions

Follow these steps after downloading or cloning the repository.

### 1️⃣ Create a virtual environment (recommended)
python -m venv .venv



### 2️⃣ Activate the environment
**Windows:**
.venv\Scripts\activate



**Mac/Linux:**
source .venv/bin/activate



### 3️⃣ Install required packages
pip install -r requirements.txt



### 4️⃣ Run the application
python main.py



Your terminal will show:
Running on http://127.0.0.1:5000



Open that link in your browser.

---

## 4. How to Use AccessCheck

### Step 1 — Enter a URL
Type or paste a website URL in the input box.  
You don't need to type `https://` — the app adds it automatically.

**Example:**
linkedin.com



### Step 2 — Press “Analyze” or hit Enter
The app will:
1. Fetch the HTML  
2. Insert line numbers  
3. Run four accessibility checks  
4. Display results below  

### Step 3 — Read the Results
Each section shows:
- ✅ **PASS** or ❌ **FAIL**  
- A short explanation  
- Line numbers showing exactly where issues occur  

---

## 5. Screenshots

✨ Add your real screenshots here. Example formatting:

### Homepage UI  
*(Insert screenshot here)*  

### Results Example  
*(Insert screenshot here)*  

> 💡 Tip: Drag images directly into the GitHub README editor to auto-upload.

---

## 6. Possible Errors and How to Fix Them

### ❌ 403 Forbidden
**Cause:** Some websites block bots.  
**Fix:** Try again — the app automatically switches from HTTPS → HTTP as fallback.

---

### ⚠️ Connection Error
**Possible reasons:**
- Wrong URL  
- Website temporarily down  
- No internet connection  

**Fix:**
- Check URL spelling  
- Try adding `https://` manually  
- Try HTTP version  

---

### 🕸️ Page loads but shows no headings/images/etc.
**Cause:** Some sites generate content with JavaScript.  
**Fix:** JavaScript-rendered content isn’t currently supported (static HTML only).

---

## 7. Limitations
- Websites that block crawlers may still fail.  
- JavaScript-generated content won't appear.  
- Line numbers may shift due to minified HTML.  
- No support for ARIA role analysis *(yet).*  
- No PDF report export *(only on CLI version).*  

These are normal and expected for a basic accessibility analyzer.

---

## 8. Summary
**AccessCheck** is a lightweight tool for checking common accessibility issues with helpful line-number tracing.  
It’s simple to install, easy to use, and beginner-friendly.  

For deeper architecture, development notes, and advanced details, see the **Developer Documentation** in the `/docs` folder.
