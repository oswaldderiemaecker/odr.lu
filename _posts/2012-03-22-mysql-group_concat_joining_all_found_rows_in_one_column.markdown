---
layout: post
title:  "MySQL GROUP_CONCAT: joining all found rows in one column"
date:   2012-03-18 21:47:09
categories: mysql
---

Use GROUP_CONTACT to group rows result in one column, use DISTINCT to prevent duplicates. e.g.

Table1:

| key_id |  id_person |  lastname |
| 1 |  1 |  John Doe |
| 2 |  2 |  Jane Doe |

Table1_projects:

|key_id | project_name |
|10 | Project 1|
|11 | Project 2|
|12 | Project 3|
|13 | Project 4|

Table1_has_projects:

|key_id | projects_id |
|1 |  10 |
|1 |  11 |
|1 |  12 |
|1 |  13 |
|2 |  11 |
|2 |  10 |
|2 |  11 |
|2 |  11 |

The below query return all projects of each record in the Table1 and list them in one column. Distinct prevent to have duplicates e.g. 11 in the Table1_has_projects.

{% highlight mysql%}
SELECT tb1.key_id, tb1.id_person, tb1.lastname, 
GROUP_CONCAT(DISTINCT tbproject.project_name SEPARATOR ';')
FROM Table1 tb1 
LEFT JOIN Table1_has_projects tb_has_project ON tb_has_project.key_id=tb1.key_id 
LEFT JOIN Table1_projects tbproject ON tbproject.key_id=tb_has_project.projects_id 
GROUP BY tb1.key_id ORDER BY tbproject.project_name DESC;
{% endhighlight %}

Output:

|key_id | id_person |  lastname |   GROUP_CONCAT(DISTINCT tbproject.project_name SEPARATOR ';') |
|2 |  2 |  Jane Doe |   Project 2;Project 1 |
|1 |  1 |  John Doe |   Project 1;Project 2;Project 3;Project 4 |
