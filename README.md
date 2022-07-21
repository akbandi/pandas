# Dependencies and Setup
import pandas as pd
In [ ]:
# File to Load (Remember to Change These)
school_data_to_load = "../Resources/schools_complete.csv"
student_data_to_load = "../Resources/students_complete.csv"
In [ ]:
# Read School Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
school_data
In [ ]:
# Read Student Data File and store into Pandas DataFrames
student_data = pd.read_csv(student_data_to_load)
student_data.head()
In [ ]:
# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on="school_name")
school_data_complete.head()
District Summary
In [ ]:
#Calculate the total number of schools
Total_schools = len(school_data.index)
Total_schools
In [ ]:
#Calculate the total number of students
Total_students = len(student_data.index)
Total_students
In [ ]:
#Calculate the total budget
Total_budget = school_data["budget"].sum()
Total_budget
In [ ]:
#Calculate the average math score
Average_Math_score = student_data["math_score"].mean()
Average_Math_score
In [ ]:
#Calculate the average reading score
Average_Reading_score = student_data["reading_score"].mean()
Average_Reading_score
In [ ]:
# students with >= 70 marks in maths
passing_math = student_data.loc[(student_data["math_score"] >= 70),:]
passing_math
In [ ]:
#Total number of students passed in maths
passing_math_students = len(passing_math.index) 
passing_math_students
In [ ]:
#Calculating the percentage of students passed in maths
passing_math_perecent = passing_math_students/Total_students * 100
passing_math_perecent
In [ ]:
# students with >= 70 marks in reading
passing_reading = student_data.loc[(student_data["reading_score"] >= 70),:]
passing_reading
In [ ]:
#Total number of students passed in raeding
passing_reading_students = len(passing_reading.index) 
passing_reading_students
In [ ]:
#Calculating the percentage of students passed in reading
passing_reading_perecent = passing_reading_students/Total_students * 100
passing_reading_perecent
In [ ]:
# students with >= 70 marks in maths and reading
passing_overall = student_data.loc[(student_data["reading_score"] >= 70) & (student_data["math_score"] >= 70),:]
passing_overall
In [ ]:
#Total number of students passed Overall
passing_overall_students = len(passing_overall.index) 
passing_overall_students
In [ ]:
#Calculating the percentage of students passed overall
passing_overall_perecent = passing_overall_students/Total_students * 100
passing_overall_perecent
In [ ]:
#District Summary Data Frame - Dictionary of List method
District_Summary = pd.DataFrame({ "Total Schools":[Total_schools],"Total Students":[Total_students],
                    "Total Budget":[Total_budget],"Average Math Score":[Average_Math_score],
                    "Average Reading Score":[Average_Reading_score],"% Passing Math":[passing_math_perecent],
                    "% Passing Reading":[passing_reading_perecent],"% Overall Passing":[passing_overall_perecent]})
District_Summary
In [ ]:
# formating the cells & final Output for District Summary
District_Summary["Total Students"] = District_Summary["Total Students"].map("{:,}".format)
District_Summary["Total Budget"] = District_Summary["Total Budget"].map("${:,.2f}".format)
In [ ]:
District_Summary
School Summary
In [ ]:
#school names
school_name = school_data_complete.set_index('school_name').groupby(['school_name'])
In [ ]:
#school types
school_type = school_data.set_index('school_name')['type']
school_type
In [ ]:
#Student Count per school
student_count = school_name['Student ID'].count()
student_count
In [ ]:
#Total budget foreach school
Total_budget = school_data.set_index('school_name')['budget']
Total_budget
In [ ]:
#per student budget
budget_per_student = school_data.set_index('school_name')['budget']/school_data.set_index('school_name')['size']
budget_per_student
In [ ]:
#Average Scores 
Average_Math =  school_name['math_score'].mean()
Average_Reading = school_name['reading_score'].mean()
In [ ]:
# passing %
passing_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('school_name')['Student ID'].count()/student_count * 100
passing_read = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('school_name')['Student ID'].count()/student_count * 100
overall_passing = school_data_complete[(school_data_complete['reading_score'] >= 70) &
         (school_data_complete['math_score'] >= 70)].groupby('school_name')['Student ID'].count()/student_count*100                 
In [ ]:
#school summary Dataframe
school_summary = pd.DataFrame({
    "School Type": school_type,
    "Total Students": student_count,
    "Total School Budget": Total_budget.map("{:,.2f}".format),
    "Per Student Budget": budget_per_student.map("${:,.2f}".format),
    "Average Math Score": Average_Math,
    "Average Reading Score": Average_Reading,
    '% Passing Math': passing_math,
    '% Passing Reading': passing_read,
    "% Overall Passing": overall_passing
})
In [ ]:
school_summary
Top Performing Schools (By % Overall Passing)
In [ ]:
#Top Performing Schools
top_performing_schools = school_summary.sort_values("% Overall Passing", ascending = False)
top_performing_schools.head()
Bottom Performing Schools (By % Overall Passing)
In [ ]:
#Bottom Performing Schools
bottom_performing_schools = school_summary.sort_values("% Overall Passing")
bottom_performing_schools.head()
Math Scores by Grade
In [ ]:
# Math grade level
nine_math = student_data.loc[student_data['grade'] == '9th'].groupby('school_name')["math_score"].mean()
ten_math = student_data.loc[student_data['grade'] == '10th'].groupby('school_name')["math_score"].mean()
eleven_math = student_data.loc[student_data['grade'] == '11th'].groupby('school_name')["math_score"].mean()
twelve_math = student_data.loc[student_data['grade'] == '12th'].groupby('school_name')["math_score"].mean()
In [ ]:
#Math Scores Dataframe
Math_Scores = pd.DataFrame({
        "9th": nine_math,
        "10th": ten_math,
        "11th": eleven_math,
        "12th": twelve_math
})
In [ ]:
Math_Scores
Reading Score by Grade
In [ ]:
# Reading grade level
nine_reading = student_data.loc[student_data['grade'] == '9th'].groupby('school_name')["reading_score"].mean()
ten_reading = student_data.loc[student_data['grade'] == '10th'].groupby('school_name')["reading_score"].mean()
eleven_reading = student_data.loc[student_data['grade'] == '11th'].groupby('school_name')["reading_score"].mean()
twelve_reading = student_data.loc[student_data['grade'] == '12th'].groupby('school_name')["reading_score"].mean()
In [ ]:
#Reading Scores Dataframe
Reading_Scores = pd.DataFrame({
        "9th": nine_reading,
        "10th": ten_reading,
        "11th": eleven_reading,
        "12th": twelve_reading
})
In [ ]:
Reading_Scores
Score by School Spending
In [ ]:
#creating bins
bins  = [0,585,630,645,680]
group_name = ["<$585", "$585-630", "$630-645", "$645-680"]
In [ ]:
school_summary["Per Student Budget"]=school_summary["Per Student Budget"].apply(lambda x: x.replace('$', '').replace(',', '')).astype('float')
In [ ]:
school_summary['Spending Ranges(per Student)']=pd.cut(school_summary["Per Student Budget"], bins, labels = group_name)
In [ ]:
#group by the ranges
School_spending = school_summary.groupby('Spending Ranges(per Student)')
In [ ]:
# mean values by spending
scores_by_school_spending = School_spending.mean()
In [ ]:
scores_by_school_spending["Average Math Score"] = scores_by_school_spending["Average Math Score"].map("{:.2f}".format)
scores_by_school_spending["Average Reading Score"] = scores_by_school_spending["Average Reading Score"].map("{:.2f}".format)
scores_by_school_spending["% Passing Math"] = scores_by_school_spending["% Passing Math"].map("{:.2f}".format)
scores_by_school_spending["% Passing Reading"] = scores_by_school_spending["% Passing Reading"].map("{:.2f}".format)
scores_by_school_spending["% Overall Passing"] = scores_by_school_spending["% Overall Passing"].map("{:.2f}".format)
In [ ]:
# Scores by School spending 
scores_by_school_spending[["Average Math Score",
                    "Average Reading Score",
                    "% Passing Math",
                    "% Passing Reading",
                    "% Overall Passing"]]
Score by School Size
In [ ]:
#creating bins
bins = [0, 1000, 2000,5000]
group_name = ["Small (<1000)", "Medium (1000-2000)" , "Large (2000-5000)"]
In [ ]:
school_summary['School Size'] = pd.cut(school_summary['Total Students'], bins, labels = group_name)
In [ ]:
#group by size
School_Size = school_summary.groupby('School Size')
In [ ]:
# mean values by size
scores_by_school_size = School_Size.mean()
In [ ]:
# Scores by school size
scores_by_school_size[["Average Math Score",
                "Average Reading Score",
                "% Passing Math",
                "% Passing Reading",
                "% Overall Passing"]]
Scores by School Type
In [ ]:
# group by school type
School_type = school_summary.groupby('School Type')
In [ ]:
# mean values by tpe
scores_by_school_type = School_type.mean()
In [ ]:
# Scores School type 
scores_by_school_type[["Average Math Score",
                "Average Reading Score",
                "% Passing Math",
                "% Passing Reading",
                "% Overall Passing"]]