# Data Dictionary

| Column              | Description                                        |
| ------------------- | -------------------------------------------------- |
| job_id              | Unique identifier for each job posting             |
| job_title           | Title of the advertised position                   |
| company_name        | Name of the hiring organization                    |
| company_rating      | Company rating                                     |
| location            | Original job location                              |
| scraped_city        | City extracted from the source                     |
| primary_city        | Standardized city used for analysis                |
| role_category       | Standardized job role category                     |
| experience_raw      | Original experience requirement                    |
| experience_min_yrs  | Minimum required experience                        |
| experience_max_yrs  | Maximum required experience                        |
| experience_tier     | Experience-level classification                    |
| salary_raw          | Original salary information                        |
| salary_min_lpa      | Minimum salary in LPA                              |
| salary_max_lpa      | Maximum salary in LPA                              |
| salary_midpoint_lpa | Midpoint of salary range                           |
| salary_disclosed    | Indicates whether salary was disclosed             |
| salary_tier         | Salary classification                              |
| skills_required     | Skills listed in the job posting                   |
| skills_count        | Number of skills associated with the posting       |
| skill_domain        | Skill-domain classification                        |
| work_mode           | Remote, Hybrid, or On-site                         |
| company_size_bucket | Company size classification                        |
| posted_date_raw     | Original posting date information                  |
| days_since_posted   | Number of days since posting                       |
| is_fresher_friendly | Indicates whether the job is suitable for freshers |
| is_senior           | Indicates senior-level position                    |
| salary_negotiable   | Indicates whether salary is negotiable             |
| data_source         | Source from which the job posting was collected    |
| job_url             | URL associated with the job posting                |
| scraped_at          | Timestamp/date when the data was collected         |
