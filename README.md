# Data Analyst Job Market Analysis with SQL

# Introduction

This project explores the Data Analyst job market using SQL to examine job postings, salary information, and employer skill requirements. My goal was to understand which opportunities offer high salaries, which skills employers request most often, and how skill demand compares with average advertised pay.

I organized the analysis into five queries, moving from individual job opportunities to broader skill trends. The project demonstrates how relational data can support practical questions about career development and job searching.

# Background

Choosing which skills to learn is easier when the decision is supported by job-market data. I built this project to investigate the relationship between Data Analyst roles, requested skills, and advertised annual salaries.

The analysis addresses five questions:

1. What are the highest-paying remote Data Analyst jobs?
2. What skills are associated with those top-paying roles?
3. Which skills are most in demand across Data Analyst postings?
4. Which skills are associated with the highest average annual salaries?
5. How do skill demand and average salary compare when considered together?

The project uses four related tables:

| Table | Purpose |
| --- | --- |
| `job_postings_fact` | Stores job titles, locations, salary information, posting dates, and company identifiers. |
| `company_dim` | Provides company names and other company details. |
| `skills_dim` | Defines the skills referenced in job postings. |
| `skills_job_dim` | Connects job postings to their associated skills. |

The bridge table, `skills_job_dim`, makes it possible to analyze multiple skills for a single job and the same skill across many jobs.

## Analysis Scope

Every analysis filters for `job_title_short = 'Data Analyst'`. The first two queries also restrict the location to `Anywhere`. The remaining three queries have no location restriction.

Salary-based analyses exclude postings with missing `salary_year_avg` values. The overall skill-demand query includes postings regardless of whether they disclose a salary. No query applies a date filter, so the analysis reflects the records available in the loaded dataset rather than a specified reporting period.

# Tools I Used

- **SQL:** Filtered job postings, connected related tables, calculated summary statistics, and ranked results.
- **PostgreSQL:** The project's database scripts define tables, primary keys, foreign keys, and indexes using PostgreSQL syntax.
- **Visual Studio Code:** Used to organize and edit the SQL project files.
- **Git and GitHub:** Used to maintain and share the project repository.
- **CSV files:** Provide the source data for the relational tables.

Key SQL techniques include `LEFT JOIN`, `INNER JOIN`, common table expressions (CTEs), `WHERE`, `GROUP BY`, `COUNT`, `AVG`, `ROUND`, `ORDER BY`, and `LIMIT`.

# Analysis

## 1. Highest-Paying Remote Data Analyst Jobs

**File:** [1_top-paying_jobs.sql](project_sql/1_top-paying_jobs.sql)

The first query focuses on Data Analyst postings with a location of `Anywhere` and a recorded average annual salary. It connects job postings with company information, sorts salaries from highest to lowest, and limits the output to ten jobs.

The analysis brings together job identifiers, titles, location information, advertised annual salaries, posting dates, and company names. This provides a starting point for investigating the highest-paying opportunities within the selected remote-job subset.

**SQL techniques:** `LEFT JOIN`, multiple filtering conditions, descending sorting, and `LIMIT 10`.

## 2. Skills Associated with Top-Paying Roles

**File:** [2_top_paying_jobs_skills.sql](project_sql/2_top_paying_jobs_skills.sql)

The second analysis is designed to connect the top-paying remote roles with their listed skills. A CTE named `top_paying_jobs` first selects ten salary-ranked Data Analyst postings. The following query references the job-to-skill bridge table and the skills table to bring skill information into the analysis.

This question shifts the focus from identifying attractive jobs to understanding their technical requirements. It is intended to help connect career opportunities with skills to investigate further. The final query includes a five-row output limit.

**SQL techniques:** A CTE, table joins, salary sorting, and an output limit.

## 3. Most In-Demand Data Analyst Skills

**File:** [3_top_demanded_skills.sql](project_sql/3_top_demanded_skills.sql)

This query joins job postings to their associated skills, filters for Data Analyst roles, and counts job-to-skill records for each skill. It returns the five skills with the highest counts.

Unlike the first two analyses, it covers all locations in the dataset. It also includes jobs without salary information, making its population broader than the salary-based analyses.

The `demand_count` metric measures how many linked Data Analyst postings list each skill. A single posting can contribute to the counts for several different skills.

**SQL techniques:** `INNER JOIN`, `COUNT`, `GROUP BY`, and descending ranking.

## 4. Highest-Paying Skills by Average Salary

**File:** [4_top_paying_skills.sql](project_sql/4_top_paying_skills.sql)

This query examines Data Analyst postings with recorded annual salaries. It groups postings by skill, calculates `AVG(salary_year_avg)`, rounds the result to a whole number, and returns the 25 skills with the highest averages.

This offers a salary-focused view of skills across all locations in the dataset. The metric describes the average advertised annual salary of jobs associated with a skill; it does not measure the additional pay caused by learning that skill.

Because the query does not impose a minimum posting count, a skill appearing in only a few highly paid jobs can rank above a more widely requested skill.

**SQL techniques:** Salary filtering, `AVG`, `ROUND`, grouping, and salary-based sorting.

## 5. Comparing Skill Demand and Salary

**File:** [5_optimal_skills.sql](project_sql/5_optimal_skills.sql)

The final query combines demand and salary information using two CTEs:

- `skills_demand` counts postings associated with each skill.
- `average_salary` calculates the rounded average annual salary associated with each skill.

Both CTEs use Data Analyst postings with non-null annual salaries. The final query joins them by `skill_id` and returns skill names, demand counts, and average salaries.

Results are ordered by demand count first and average salary second, with a limit of 25 rows. This prioritizes frequently requested skills within salary-disclosing postings, using salary to break equal demand counts. It does not calculate a combined demand-and-salary score.

**SQL techniques:** Multiple CTEs, aggregation, joins between summarized results, and sorting by more than one metric.

## Interpreting the Results

The five analyses provide different views of the job market. High salary, frequent demand, and availability in remote roles are separate measures, so the scope of each query matters when comparing its output with another.

The queries explore salary trends and skill demand across Data Analyst job postings.

The dataset also has practical limitations:

- Salary-based results represent only postings that disclose annual salary information.
- Average salaries can be influenced by unusually high values and small sample sizes.
- Geography, seniority, and employer differences can affect salary comparisons.
- Advertised salaries describe postings rather than confirmed employee earnings.
- Skill associations do not establish that a skill causes higher pay.

# What I Learned

Through this project, I practiced translating career-related questions into SQL queries and organizing an analysis around clear objectives.

- **Connecting relational tables:** I worked with job, company, and skill data linked through identifiers.
- **Using different join types:** I used a left join for company information and inner joins when working with linked skill records.
- **Breaking down complex questions:** CTEs helped separate the selection of top-paying jobs and the calculation of skill-level metrics into readable steps.
- **Building summary metrics:** I used counts to describe skill demand and averages to compare advertised annual salaries.
- **Managing missing salary data:** Filtering null salary values defined which postings could contribute to salary comparisons.
- **Understanding ranking logic:** Sorting by demand first produces a different priority order from sorting by salary first.
- **Interpreting scope:** Remote-only results, all-location results, and salary-disclosing subsets answer different questions.

The project strengthened my ability to explain what a query measures and how its filters and sorting rules shape the interpretation.

# Conclusions

This project uses five SQL analyses to explore Data Analyst opportunities through job-level salaries, employer skill requirements, overall skill demand, and skill-level salary averages.

The central lesson is that career research benefits from looking at more than one measure. A skill can be widely requested without having the highest associated salary, while a skill with a high average salary may appear in relatively few postings. Comparing both measures provides useful context for deciding what to investigate and learn next.

Future improvements could include adding posting counts beside every salary average, requiring a minimum sample size, comparing median salaries, and applying consistent location and date filters across analyses. Exporting reviewed query results would also make it possible to add evidence-based findings and charts to this README.
