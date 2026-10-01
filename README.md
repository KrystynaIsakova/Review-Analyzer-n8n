# Automated Course Reviews Analysis with n8n

This project demonstrates how to build an automated **course review
analysis workflow in n8n**.

The workflow downloads reviews from a CSV file, separates positive and
low-rated reviews, analyzes negative feedback with an LLM, and generates
a summary report.

## Workflow

``` text
Schedule Trigger
      ↓
Download & Read CSV
      ↓
Split by Rating
   ↙           ↘
Positive     Low-rated
   ↓             ↓
 Count       LLM Analysis
                 ↓
          Topic + Severity
             ↙       ↘
          Summarize Results
                 ↓
               Merge
                 ↓
          Generate Report
```

## AI Analysis

Only reviews with **1--3 stars** are sent to the LLM.

For each review, the model extracts:

-   `topic` --- main complaint category
-   `problem` --- short description
-   `severity` --- low, medium, or high

Positive reviews are simply counted, avoiding unnecessary LLM calls.

## Output

The final report contains:

-   total number of reviews
-   positive vs. low-rated reviews
-   most common complaint topics
-   severity distribution

Example:

``` text
Total reviews: 200
Positive reviews: 153
Low-rated reviews: 47

Main Issues
course_content: 22
difficulty: 8
assignments: 8

Severity
medium: 24
high: 12
low: 11
```

## Key Idea

**Structured data → workflow logic → LLM for text → structured output →
aggregation → report**

The LLM is used only where language understanding is actually needed.
