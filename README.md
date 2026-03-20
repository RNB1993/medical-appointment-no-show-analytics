# Medical Appointment No-Show Analytics

## Table of Contents
- [Overview](#overview)
- [Dataset Content](#dataset-content)
- [Business Requirements](#business-requirements)
- [Project Hypotheses and Validation](#project-hypotheses-and-validation)
- [Project Plan](#project-plan)
- [Data Collection, Cleaning and Preparation](#data-collection-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Machine Learning Modelling](#machine-learning-modelling)
- [The Rationale to Map the Business Requirements to the Data Visualisations](#the-rationale-to-map-the-business-requirements-to-the-data-visualisations)
- [Analysis Techniques Used](#analysis-techniques-used)
- [Ethical Considerations](#ethical-considerations)
- [Dashboard Design](#dashboard-design)
- [Key Findings](#key-findings)
- [Unfixed Bugs](#unfixed-bugs)
- [Development Roadmap](#development-roadmap)
- [Deployment](#deployment)
- [Main Data Analysis Libraries](#main-data-analysis-libraries)
- [Credits](#credits)
- [Acknowledgements](#acknowledgements)

---

## Overview

This project analyses medical appointment no-show behaviour using a structured data analytics workflow covering ETL, exploratory data analysis, machine learning, and dashboard design.

The main aim of the project is to identify the factors most associated with missed appointments and to build a predictive workflow that can help support earlier intervention, better appointment planning, and more informed operational decision-making.

The project is organised into separate notebooks for ETL, EDA, and machine learning, alongside dashboard files, processed data outputs, and supporting images. The work was completed using a business-focused and ethics-aware approach, with particular care taken around privacy, fairness, and the interpretation of potentially sensitive features.

---

## Dataset Content

The dataset used in this project is the **Medical Appointment No Shows** dataset. It contains appointment booking information, patient characteristics, health-related indicators, and a target field showing whether the patient attended or missed the appointment.

After the ETL and cleaning stage, the final working dataset contained **110,521 rows and 22 columns**. In addition to the original fields, new analytical features were created to better support exploratory analysis and modelling. These included:

- `wait_days`
- `scheduled_hour`
- `scheduled_day_of_week`
- `appointment_day_of_week`
- `no_show_flag`
- `has_handicap`
- `same_day_appointment`
- `row_id`

The cleaned dataset was then used as the input for both the EDA and machine learning stages of the project.

---

## Business Requirements

The business requirements for this project were:

- to understand the main patterns associated with appointment no-shows
- to test a set of business-focused hypotheses using exploratory analysis
- to identify which patient, scheduling, and appointment factors appear most relevant
- to build a machine learning model capable of predicting likely no-shows
- to communicate the findings clearly through a Power BI dashboard for both technical and non-technical audiences
- to ensure that privacy, fairness, and ethical use were considered throughout the project rather than only at the end

This project was designed as a decision-support exercise. The goal is to improve understanding and support planning, not to make automated or punitive decisions about individual patients.

---

## Project Hypotheses and Validation

The exploratory analysis was structured around six project hypotheses designed to test whether no-show behaviour was associated with different patient and scheduling characteristics.

### Hypothesis 1: Lead Time Impact
**Business question:**  
Are patients with longer wait times between booking and appointment more likely to miss their appointment?

### Hypothesis 2: Reminder Effect
**Business question:**  
Do SMS reminders appear to influence no-show behaviour?

### Hypothesis 3: Age Impact
**Business question:**  
Is age associated with appointment attendance behaviour?

### Hypothesis 4: Day-of-Week Effect
**Business question:**  
Does the appointment day of the week appear to influence no-show rates?

### Hypothesis 5: Same-Day Appointment Effect
**Business question:**  
Do same-day appointments behave differently from appointments booked further in advance?

### Hypothesis 6: Neighbourhood Impact
**Business question:**  
Are there location-based differences in no-show behaviour?

These hypotheses were validated through comparative visual analysis using appropriate chart types for numeric and categorical features. The focus of this stage was on identifying interpretable business patterns rather than relying only on formal statistical testing.

---

## Project Plan

The project followed a structured end-to-end workflow:

### 1. Data collection and understanding
The raw dataset was sourced and reviewed to understand the available fields, the target variable, and the data types involved.

### 2. ETL and cleaning
Column names were standardised, date fields were converted, invalid values were removed, and new time-based features were created to improve downstream analysis.

### 3. Exploratory data analysis
A hypothesis-driven EDA process was used to explore which factors appeared most associated with no-show behaviour.

### 4. Machine learning
Classification models were trained and compared to assess whether no-show risk could be predicted from the cleaned features.

### 5. Dashboard design
The main findings were translated into Power BI pages designed to communicate key trends, KPIs, and operational insights clearly.

### 6. Reflection and ethics
Privacy, fairness, and responsible use were considered throughout the project, particularly in relation to sensitive patient features and the interpretation of socio-economic patterns.

---

## Data Collection, Cleaning and Preparation

The ETL stage prepared the raw dataset for both exploratory analysis and predictive modelling.

In this stage, column names were standardised, date fields were converted to usable formats, and new features were engineered to support more meaningful analysis. Key engineered fields included `wait_days`, `no_show_flag`, `has_handicap`, and `same_day_appointment`.

Invalid records were then removed, including:

- rows with negative ages
- rows with impossible negative waiting times

Age values of 0 were retained because they may represent infants under 1 year old.

Privacy and governance were also considered during ETL. The `patient_id` and `appointment_id` columns were removed because they did not add explanatory value to the business questions and because retaining them would not support privacy-aware, governance-focused analysis. Instead, a non-identifying `row_id` field was created for possible later use in the visualisation stage.

The cleaned dataset was then saved for use in the EDA and modelling notebooks.

---

## Exploratory Data Analysis

The exploratory analysis stage was used to test the project hypotheses and identify the main factors associated with appointment no-shows.

The cleaned dataset used for this stage contained **110,521 rows and 22 columns**. Visualisations were created using `matplotlib` and `seaborn` to compare no-show behaviour across numeric and categorical variables, including:

- waiting time
- age
- day-based features
- reminder status
- neighbourhood

A key strength of this stage was that it did not treat all patterns as purely behavioural. Some findings may reflect broader structural inequalities rather than purely individual behaviour, so results were interpreted cautiously and at an aggregated level. This was especially important when reviewing features such as neighbourhood, scholarship status, age, and health conditions.

The EDA stage therefore supported both analytical insight and ethical interpretation by highlighting patterns while avoiding simplistic or overly individualised conclusions.

---

## Machine Learning Modelling

The machine learning stage focused on predicting whether a patient was likely to miss their appointment.

Two classification models were developed and compared:

- **Logistic Regression** as the baseline model
- **Random Forest** as the comparison model

Model evaluation used a range of classification metrics so that performance could be assessed more fully than by accuracy alone.

### Model comparison summary

| Model | Accuracy | Precision | Recall | F1 Score | ROC AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.6546 | 0.3107 | 0.5832 | 0.4055 | 0.6687 |
| Random Forest | 0.5678 | 0.2985 | 0.8449 | 0.4412 | 0.7193 |

Although Logistic Regression achieved higher accuracy, Random Forest performed better on recall, F1-score, and ROC AUC.

Because the project objective was to better identify likely no-shows, **recall** was especially important. For that reason, **Random Forest** was selected as the stronger final model.

Threshold tuning was also reviewed. A threshold of **0.5** gave the best overall balance in the tested range, with the strongest F1-score while still maintaining strong recall.

The final selected Random Forest pipeline was then saved for reuse.

---

## The Rationale to Map the Business Requirements to the Data Visualisations

The data visualisations were chosen to directly support the business requirements of the project.

| Business Requirement | Visualisation Approach | Rationale |
|---|---|---|
| Understand the overall scale of the no-show issue | KPI cards and summary visuals | Gives a quick operational overview of the problem |
| Identify whether waiting time is associated with no-shows | Boxplots, distributions, and grouped comparisons | Helps show how missed appointments vary by lead time |
| Explore demographic and behavioural patterns | Bar charts and count-based visuals | Makes differences between patient groups easier to interpret |
| Examine temporal and scheduling effects | Day-of-week and time-based visuals | Supports practical scheduling insight |
| Communicate findings clearly to business users | Dashboard pages with structured summaries and tooltips | Makes technical analysis more accessible |

This mapping helped ensure that the visuals were not added for presentation only, but were selected to answer specific project questions and support business interpretation.

---

## Analysis Techniques Used

A range of analysis techniques were used across the project.

### ETL and feature engineering
Data cleaning, column standardisation, date conversion, and new feature creation were used to prepare the dataset for downstream analysis.

### Exploratory analysis
Comparative visual analysis was used to test six business-focused hypotheses and identify practical patterns in no-show behaviour.

### Machine learning
Classification modelling was used to predict no-show risk. Logistic Regression was used as a baseline model and Random Forest was used as a more flexible comparison model. Performance was evaluated using:

- accuracy
- precision
- recall
- F1-score
- ROC AUC

Threshold tuning was also used to support model selection.

### Workflow structure
The project was structured in separate ETL, EDA, and ML notebooks so that each stage had a clear purpose and output. This helped keep the workflow easier to review, validate, and document.

### Use of generative AI
Generative AI tools were used to support ideation, project structuring, explanation refinement, and code troubleshooting. All final project decisions, code implementation, notebook interpretation, and written reflections were reviewed and validated by the project author.

---

## Ethical Considerations

Ethics was an important part of this project and was considered throughout ETL, EDA, modelling, and dashboard interpretation.

### Data privacy and governance
Direct identifier fields were removed from the final cleaned dataset because they were not needed for analysis and would not support privacy-aware, governance-focused work. A non-identifying `row_id` was used instead where needed.

### Bias and fairness
Some variables in the dataset may reflect structural disadvantage rather than simple patient choice. Findings relating to neighbourhood, scholarship, age, and health-related factors were interpreted cautiously and at an aggregated level because they may reflect broader structural inequalities rather than purely individual behaviour.

### Health-related features
Health-related variables may improve model performance but also raise ethical questions about how health information is used in decision-making. In this project, these features were used only for exploratory modelling and interpretation.

### Responsible model use
The model developed in this project should be treated as a **decision-support tool** rather than an automated decision-maker. It may help identify appointments that could benefit from reminders, support, or better scheduling, but it should not be used to deny access to care, deprioritise patients, or make unfair assumptions about individuals.

### Legal and societal considerations
Because the dataset relates to healthcare appointments and includes potentially sensitive characteristics, this project was approached with a strong focus on minimising re-identification risk, avoiding harmful interpretation, and keeping the analysis at a level suitable for insight generation rather than individual-level judgement.

---

## Dashboard Design

The dashboard stage was designed to communicate the key findings from the project in a clearer and more accessible way.

The dashboard was designed to support both technical and non-technical audiences by combining high-level summary views with more detailed exploratory visuals.

### Executive Overview

This page provides a high-level summary of the project through KPI cards, key no-show metrics, and headline insights. It was designed to give users a quick understanding of the scale of the issue and the main factors influencing missed appointments.

![Executive Overview Dashboard](images/04_dashboard_excutive_overview.png)

### Patient Behaviour

This page focuses more closely on behavioural and demographic patterns in the data, including variables such as wait time, age, reminder status, and related appointment features. It supports more detailed analysis of the trends identified during EDA.

![Patient Behaviour Dashboard](images/04_dashboard_patient_behavior.png)

### Custom Tooltip Page

This page was created to support the main visuals without overcrowding the main report pages. It allows additional detail to be shown on hover, helping users explore the data in more depth while keeping the main dashboard cleaner and easier to read.

![Custom Tooltip Dashboard](images/04_dashboard_tooltip_custom.png)

### Dashboard design approach

The design approach focused on making insights easy to understand while still keeping enough detail for analytical review. In practice, this meant using:

- KPI cards
- grouped charts
- filters
- tooltip pages
- short narrative summaries

This helped improve usability for different audiences while keeping the visuals aligned to the business requirements.

---

## Key Findings

The project found that appointment no-show behaviour is not random and appears to be associated with a combination of scheduling, timing, patient, and location-based factors.

The exploratory analysis identified several meaningful patterns across the six hypotheses, including an observed association between neighbourhood and no-show behaviour. The machine learning stage then showed that no-show risk can be predicted to a useful degree, with Random Forest performing better than Logistic Regression on recall, F1-score, and ROC AUC.

These findings suggest that healthcare providers may be able to reduce missed appointments through earlier identification of higher-risk bookings and more targeted reminder or support strategies. However, any such use should remain ethically cautious and should avoid treating model output as a substitute for human judgement.

---

## Unfixed Bugs

At the current stage of the project, there are no major known bugs that prevent the main workflow from running as intended. The notebooks are separated by stage and the core outputs are saved into structured folders for reuse.

One known limitation in the ML notebook is that some exports are conditional on whether specific dataframes were created earlier in the notebook. For example, feature importance export may be skipped if the dataframe has not been created. This does not break the final workflow, but it does show that the export section could be made more robust in a future version.

A second limitation is that dashboard polish and presentation can continue to be refined, particularly where narrative summaries, layout consistency, or tooltip design could be strengthened further.

---

## Development Roadmap

There are several areas that could be developed further in a future version of the project.

- expand model tuning and compare additional algorithms
- add more formal fairness checks across relevant subgroups
- improve explainability outputs for the final selected model
- continue refining dashboard interaction and presentation
- explore how the workflow could be packaged into a more reusable analytical product

One of the main learning points from the project was that strong technical results still need careful interpretation, especially when working with healthcare-related and socio-economic features. A future version would therefore place even more emphasis on fairness review and responsible deployment thinking.

---

## Deployment

This project is currently structured as a repository-based analytics project rather than a fully deployed web application.

The main project components are stored in the GitHub repository, including:

- Jupyter notebooks for ETL, EDA, and ML
- processed data outputs
- dashboard files
- project images and supporting assets

This structure supports reproducibility and project review, while keeping each stage of the workflow clearly separated.

---

## Main Data Analysis Libraries

The main libraries used in this project include:

- `pandas` for loading, cleaning, reshaping, and exporting data
- `numpy` for numerical operations
- `matplotlib` for charting and visual analysis
- `seaborn` for higher-level statistical visualisations in EDA
- `scikit-learn` for preprocessing, modelling, and evaluation
- `joblib` for saving the final trained model pipeline

---

## Credits

### Content
- Medical Appointment No Shows dataset
- Code Institute data analytics template repository
- Official documentation for Python libraries used in the project, including pandas, seaborn, matplotlib, scikit-learn, and joblib

### Media
- Dashboard screenshots and project images created as part of the project and stored in the repository

### Support and learning
- Code Institute learning materials and project guidance
- Generative AI support for ideation, structure, and debugging assistance, with all final implementation reviewed by the project author

---

## Acknowledgements

Thanks to the instructors, walkthrough materials, and feedback sources that supported the development of this project, as well as the broader learning content that helped shape the analytical and ethical approach used throughout.