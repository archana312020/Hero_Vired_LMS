# Hero_Vired_LMS
SQL Assignment

Use lms;

-- 1. List All Courses with Their Category Names
Select c.course_name as Course_Name, ct.category_name as Category from lms.courses c
join lms.categories ct on ct.category_id = c.category_id;

-- 2. Count the Number of Courses in Each Category

Select ct.category_name as Category, count(c.course_id) from lms.courses c
join lms.categories ct on ct.category_id = c.category_id
group by ct.category_name;

-- 3. List All Students’ Full Names and Email Addresses

Select concat(first_name," ",last_name) as Full_Name, email from lms.user
where role = 'student';

-- 4. Retrieve All Modules for a Specifi c Course Sorted by Module Order
Select c.course_name, m.module_name, c.course_id, m.module_order  from lms.modules m
join lms.courses c on c.course_id = m.course_id
order by c.course_id, m.module_id;

-- 5. List All Content Items for a Specifi c Module

Select  c.module_id ,m.module_name, c.title from lms.content c
join lms.modules m on m.module_id = c.module_id;

-- 6. Find the Average Score for a Specifi c Assessment

Select a.assessment_id, a.assessment_name, avg(sa.score) from lms.assessments a
join lms.assessment_submission sa on sa.assessment_id = a.assessment_id
group by a.assessment_id, a.assessment_name;
Select * from lms.assessment_submission;

-- 7. List All Enrollments with Student Names and Course Names

Select concat(u.first_name," ",u.last_name) as Full_Name, c.course_name as Course_Name,e.enrolled_at as Enrollment_Date from lms.enrollments e
join lms.user u on u.user_id = e.user_id
join lms.courses c on c.course_id;

-- 8. Retrieve All Instructors’ Full Names

Select concat(first_name," ",last_name) as Full_Name, email from lms.user
where role = 'instructor';

-- 9. Count the Number of Assessment Submissions per Assessment

Select a.assessment_id,a.assessment_name, count(sa.submission_id) as Submission_Count from lms.assessments a
join lms.assessment_submission sa on sa.assessment_id = a.assessment_id
group by a.assessment_id,a.assessment_name;

-- 10. List the Top Scoring Submission for Each Assessment

Select a.assessment_id ,a.assessment_name, max(sa.score) as Max_Score from lms.assessments a
join lms.assessment_submission sa on sa.assessment_id = a.assessment_id
group by a.assessment_id, a.assessment_name;

-- 11. Retrieve Courses Created After a Specifi c Date

Select c.course_name, c.created_at as Created_Date from lms.courses c
where c.created_at > '2023-04-01';

-- 12. Find Students Who Have Not Submitted Any Assessments

Select u.user_id, u.first_name, u.last_name, count(sa.user_id) as score from lms.user u
left join lms.assessment_submission sa on sa.user_id = u.user_id
group by u.user_id, u.first_name, u.last_name
having count(sa.user_id) = 0;

-- 13. List the Content for Courses in the 'Programming' Category

select cs.course_name as Course_Name, c.title as Content, ct.category_name from lms.content c
join lms.modules m on m.module_id = c.module_id
join lms.courses cs on cs.course_id = m.course_id
join lms.categories ct on ct.category_id = cs.category_id
where ct.category_name = 'Programming';

-- 14. Retrieve Modules That Have No Associated Content

Select m.module_id, m.module_name, count(cn.content_id) as Content_Count from lms.modules m
left join lms.content cn on cn.module_id = m.module_id
group by  m.module_id, m.module_name
having count(cn.content_id) = 0;

-- 15. List Courses with the Total Number of Enrollments

Select c.course_name, count(e.enrollment_id) as Enrollments from lms.courses c
join lms.enrollments e on e.course_id = c.course_id
group by c.course_name;

-- 16. Find the Average Assessment Submission Score for Each Course

Select c.course_name, avg(sa.score) from lms.courses c
join lms.modules m on m.course_id = c.course_id
join lms.assessments a on a.module_id = m.module_id
join lms.assessment_submission sa on sa.assessment_id = a.assessment_id
group by c.course_name;

-- 17. List Users with Their Number of Enrollments

Select u.user_id, u.first_name, u.last_name, count(distinct e.course_id) as Course_Count from lms.user u
join lms.enrollments e on e.user_id = u.user_id
group by u.user_id, u.first_name, u.last_name;

-- 18. Find the Assessment with the Highest Average Score

Select	assessment_id ,Avg(score) as Avg_Score from lms.assessment_submission
group by assessment_id
order by Avg_Score desc
limit 1;

-- 19. List Courses Along with Their Modules and Content in Hierarchical Order

Select c.course_name ,m.module_name,ct.title from lms.content ct
join lms.modules m on m.module_id = ct.module_id
join lms.courses c on c.course_id = m.course_id;

-- 20. Find the Total Number of Assessments Per Course

Select c.course_id, c.course_name, count(distinct a.assessment_id) as Assessmens_Count from lms.courses c
left join lms.modules m on m.course_id = c.course_id
left join lms.assessments a on a.module_id = m.module_id
group by c.course_id,c.course_name;

-- 21. List All Enrollments from May 2023

Select e.enrollment_id, c.course_name, Concat(u.first_name," ",u.last_name) as FullName, e.enrolled_at from lms.enrollments e
join lms.courses c on c.course_id = e.course_id
join lms.user u on u.user_id = e.user_id
where e.enrolled_at between '2023-05-01' and '2023-05-31';

-- 22. Retrieve Assessment Submission Details Along with Course and Student Information

Select c.course_name,Concat(u.first_name," ",u.last_name) as Student_Name, a.assessment_name as Assessment_Name, sa.submission_data, sa.submitted_at, sa.score from lms.assessment_submission sa
join lms.assessments a on a.assessment_id = sa.assessment_id
join lms.user u on u.user_id = sa.user_id
join lms.modules m on m.module_id = a.module_id
join lms.courses c on c.course_id = m.course_id;

-- 23. List All Users with Their Roles

Select Concat(u.first_name," ",u.last_name) as Full_Name, u.role from lms.user u;

-- 24. Find the Percentage of Passing Submissions for Each Assessment

Select a.assessment_name, sa.score, a.max_score, (sa.score/a.max_score)*100 as Percentage from lms.assessment_submission sa
join lms.assessments a on a.assessment_id = sa.assessment_id
where (sa.score/a.max_score)*100 >= 60;

-- 25. Find Courses That Do Not Have Any Enrollments

Select c.course_id,c.course_name, Count(e.enrollment_id) as Enrollments from lms.courses c
left join lms.enrollments e on e.course_id = c.course_id
group by c.course_id,c.course_name
having Count(e.enrollment_id) = 0;

