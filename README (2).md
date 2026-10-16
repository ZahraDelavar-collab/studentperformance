# Students' Performance — Portugal Math Grades

Exploratory data analysis, simple linear-regression modelling, and an interactive Power BI dashboard built on the `student-mat.csv` dataset (students' math grades in Portugal).

## Repository Files

| File | Description |
|---|---|
| `student-mat.csv` | Dataset: 395 students, 34 columns |
| `portugal_math.ipynb` | Python notebook: data analysis, visualizations and linear regression |
| `studentsdashboard.pbix` | Power BI dashboard (6 pages) |

## Dataset

395 rows and 34 columns, including an `id` column for each student.

| Group | Columns |
|---|---|
| Student | `id`, `sex`, `age`, `health`, `romantic`, `higher` |
| School | `school` (GP / MS), `reason`, `schoolsup`, `paid`, `activities`, `nursery` |
| Family | `famsize`, `Pstatus`, `Medu`, `Fedu`, `Mjob`, `Fjob`, `guardian`, `famsup`, `famrel` |
| Home | `address`, `internet` |
| Time & lifestyle | `traveltime`, `studytime`, `freetime`, `goout`, `Dalc` (weekday alcohol), `Walc` (weekend alcohol) |
| Academic | `failures`, `absences`, `G1`, `G2`, `G3` (first, second and final grade) |

The target variable is `G3`, the final grade.

## Notebook — `portugal_math.ipynb`

**Libraries:** pandas, numpy, matplotlib, seaborn, scikit-learn

### Questions answered in the notebook

1. Mean, median and standard deviation of the final grade (`G3`)
2. How many students scored below 10 in `G3`?
3. Distribution of `G1`, `G2` and `G3`
4. Is there a considerable difference between girls' and boys' grades?
5. Rate of alcohol consumption among students and its effect on the final grade
6. How many students have internet access, and is there a relation between internet access and `G3`?
7. Study time and average grade
8. Which school has better performance?
9. What percent of students' parents have specific jobs?
10. Do extracurricular activities relate to the final grade?

### Results

| Question | Result |
|---|---|
| `G3` mean / median / std | 10.42 / 11.0 / 4.58 |
| Students with `G3` < 10 | About 33 % (the notebook treats them as failed); of these, about 29 % scored 0 |
| Average `G3` by sex | Female 9.97, Male 10.91 — only about 1 point difference, so no considerable gap |
| Alcohol consumption | About 30 % of students have an average of weekday and weekend alcohol use above 2; the scatter plot suggests that heavy consumption (about 3 and above) affects final grades |
| Internet access | 329 students have access; the bar and box plots show higher final grades for them |
| Study time | Higher study time can give a higher grade, but lower study-time levels show no striking difference |
| School | GP 10.49, MS 9.85 — GP performs better |
| Parents' jobs | 141 students have both a father and a mother with a specific job (neither `other`) |
| Activities vs `G3` | Correlation ≈ 0.016 (average `G3` of students with activities: 10.49) |

### Modelling

Two simple linear regressions were fitted to predict `G3` (80 % train / 20 % test split, `random_state` not set):

| Model | Feature | Equation | R² | MSE |
|---|---|---|---|---|
| Model 1 | `studytime` | 9.162 + 1 × studytime | ≈ -0.01 | 19.12 |
| Model 2 | `freetime` | 10.987 + 0 × freetime | ≈ -0.04 | 21.47 |

These are the values from the saved run of the notebook. A single feature has no real predictive power for `G3`, and the numbers change slightly on each run.

### Running the notebook

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook portugal_math.ipynb
```

The notebook loads the data with a local Windows path:

```python
portugal_math_grade = pd.read_csv("C:\\Users\\hp\\Documents\\student-mat.csv")
```

Change it to match the location of `student-mat.csv` on your machine, e.g. `pd.read_csv("student-mat.csv")` if both files are in the same folder.

## Dashboard — `studentsdashboard.pbix`

An interactive Power BI report with 6 pages. The data model has a fact table (`fact`) and dimension tables (`dimstudent`, `dimschool`, `dimfamily`, `dimhome`, `dimtime`), plus a `Measure Table` that holds the DAX measures.

| Page | Content |
|---|---|
| **Overview** | Cards: total students, average final grade, average age, percent of students passed, min and max final grade, students with the maximum (20) and minimum final grade, final grade with health, school with the highest and lowest final grade. Charts: students by school and by sex (donuts), distribution of `G3`, average final grade by sex and by failures. Slicers: family support, school support, health |
| **School Performance** | School slicer; average final grade by school; number of students with school support; average final grade by school support; average failures by school; final grades with and without school support; average age by school; average final grade and number of students by reason for choosing the school |
| **Family** | Number of students with family support; average final grade by father's job and by mother's job; students whose parents have specific jobs; final grades with and without family support; average final grade and average failures by family support |
| **Address and Internet** | Students by address (donut); percent of students with internet access; pass rate, average final grade and average failures by address; average final grade and average failures by internet access |
| **Time** | Average final grade by free time, travel time and study time; final grades with and without free time and travel time; average study time by sex and by final grade |
| **alcohol** | Average alcohol consumption overall and by sex, school, family support and school support; alcohol consumption by healthy students; alcohol consumption versus final grade |

Open the file with Power BI Desktop.

## Tools

Python · pandas · numpy · matplotlib · seaborn · scikit-learn · Power BI
