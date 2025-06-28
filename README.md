# HR Analytics: Job Change of Data Scientists
## 📌 Project Overview
This project aims to help a data-driven company identify which candidates are likely to seek a new job after completing the company's training program for data science. By predicting candidate behavior, the company can optimize training resources, reduce cost, and improve course planning and targeting.

## 📊 Data Source
Information related to demographics, education, experience are in hands from candidates signup and enrollment.
Total 6 data tables collected from various sources including an Excel file, a CSV file, a Google Sheet, a web page, and an SQL database.

### Table 1: Enrollies' data
As enrollies are submitting their request to join the course via Google Forms, we have the Google Sheet that stores data about enrolled students, containing the following columns:

`enrollee_id`: unique ID of an enrollee

`full_name`: full name of an enrollee

`city`: the name of an enrollie's city

`gender`: gender of an enrollee

The source [here](https://docs.google.com/spreadsheets/d/1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI/edit?usp=sharing).

### Table 2: Enrollies' education
After enrollment everyone should fill the form about their education level. This form is being digitalized manually. 
Educational department stores it in the Excel format [here](https://assets.swisscoding.edu.vn/company_course/enrollies_education.xlsx).

This table contains the following columns:

`enrollee_id`: A unique identifier for each enrollee. This integer value uniquely distinguishes each participant in the dataset.

`enrolled_university`: Indicates the enrollee's university enrollment status. Possible values include no_enrollment, Part time course, and Full time course.

`education_level`: Represents the highest level of education attained by the enrollee. Examples include Graduate, Masters, etc.

`major_discipline`: Specifies the primary field of study for the enrollee. Examples include STEM, Business Degree, etc.

### Table 3: Enrollies' working experience
Another survey that is being collected manually by educational department is about working experience.

Educational department stores it in the CSV format [here](https://assets.swisscoding.edu.vn/company_course/work_experience.csv).

This table contains the following columns:

`enrollee_id`: A unique identifier for each enrollee. This integer value uniquely distinguishes each participant in the dataset.

`relevent_experience`: Indicates whether the enrollee has relevant work experience related to the field they are currently studying or working in. Possible values include Has relevent experience and No relevent experience.

`experience`: Represents the number of years of work experience the enrollee has. This can be a specific number or a range (e.g., >20, <1).

`company_size`: Specifies the size of the company where the enrollee has worked, based on the number of employees. Examples include 50−99, 100−500, etc.

`company_type`: Indicates the type of company where the enrollee has worked. Examples include Pvt Ltd, Funded Startup, etc.

`last_new_job`: Represents the number of years since the enrollee's last job change. Examples include never, >4, 1, etc.

### Table 4: Training hours
From LMS system's database you can retrieve a number of training hours for each student that they have completed.

**Database credentials:**

+ Database type: `MySQL`
+ Host: `112.213.86.31`
+ Port: `3360`
+ Login: `etl_practice`
+ Password: `550814`
+ Database name: `company_course`
+ Table name: `training_hours`
  
### Table 5. City development index
Another source that can be usefull is the table of City development index.

The City Development Index (CDI) is a measure designed to capture the level of development in cities. It may be significant for the resulting prediction of student's employment motivation.

It is stored [here](https://sca-programming-school.github.io/city_development_index/index.html).

### Table 6. Employment
From LMS database you can also retrieve the fact of employment. If student is marked as employed, it means that this student started to work in our company after finishing the course.

**Database credentials:**

+ Database type: `MySQL`
+ Host: `112.213.86.31`
+ Port: `3360`
+ Login: `etl_practice`
+ Password: `550814`
+ Database name: `company_course`
+ Table name: `employment`

## 🔧 Project Workflow (ETL process)
### Extract Data
Each source was accessed using the appropriate method:
+ `Excel` and `CSV`: downloaded using requests and saved locally
+ `Google Sheets` and `Web` data: loaded with pandas
+ `SQL`: queried via sqlite3 or SQLAlchemy

### Transform Data

+ 🧼 **Data Type Conversion**: Categorical fields were converted to the 'category' data type to optimize memory and processing efficiency; string fields like `full_name` were explicitly cast to 'string' for clarity.
+ ⚙️ **Handling Missing Values**: Missing values in categorical columns were filled using the mode (most frequent value), as these fields are nominal and imputation with mode maintains consistency.
+ 🛠️ **Refactoring**: Combine similar steps (fillna + astype) into a loop and function to make the code more concise and easier to maintain.

### Load Data 
+ 🗃️ A SQLite database (`data_warehouse.db`) was created using SQLAlchemy.

+ DataFrames were separated into dimension tables and a fact table, then loaded into the database:

  + 🧩 1 Fact Table: `fact_enrollies_education`

  + 🧱 5 Dimension Tables: `dim_enrollies_data`, `dim_work_experience`, `dim_training_hours`, `dim_cities`, `dim_employment`

👉 This reflects a basic star schema, enabling more efficient querying and reporting.

## ⚙️ How to Run This Project (Google Colab)
To run this project on Google Colab:
1. Download full notebook [here](https://github.com/Truc034/HR_Analytics/blob/main/HR_analytics.ipynb) and open with Google Colab.
2. Follow the cells step-by-step:
   + Collect data from all sources
   + Clean and unify the datasets
   + Store them into a local SQLite database
3. Download the database file (`data_warehouse.db`) to explore or use in other tools

## ⏱️ Instruction: How to Schedule the Script (on Windows Task Scheduler)
Automate this script using Windows Task Scheduler by following these steps:
**1. Export Notebook to Python Script**
+ In Colab: `File > Download > Download .py`, and then save it as: `HR Analytics.py`
+ Or download [.py file here](https://github.com/Truc034/HR_Analytics/blob/main/HR_analytics.py).

**2. Create a `.bat` file to run the script**
+ Open Notepad and paste the following:
<cd path\to\your\script

python HR_analytics.py>
+ Save it as `run_hr_script.bat`




