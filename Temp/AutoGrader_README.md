## Introduction

This AutoGrader supports:-
1. Jupyter Notebook (*.ipynb*) with Python and SageMath kernels.
2. databricks Notebook (*.dbc*) with Python, Scala and Spark kernels. This one supports Python and Scala languages in a single notebook.

## Prerequisites
One needs to install:-
1. Python (Python3 is recommended.)
2. Docker and Docker Compose
3. Ubuntu is recommended, and some basic knowledge in Linux is required.

## Master Notebook Preparation
### Your Course Directory
1. Clone `test_course` repository. Put it beside the repositories of `AssignmentNotebook`, `Autograder`, `StudiumInterface`, etc.
2. Rename it to your course name.

### Master Notebook Creation
To create a master notebook, please see examples of master notebooks and read the description in the section below:- 

- See `Example_databricks_1.dbc` and `Example_databricks_2.dbc` in `master/db` directory for examples of databricks master notebooks

- See `Example_Jupyter_1.ipynb` and `Example_Jupyter_2.ipynb` in `master/jp` directory for examples of Jupyter master notebooks

> **NOTE !**
> The construction of a master notebook for both databricks and Jupyter is the same. The difference is just the file format.

> One may put problems in an assignment in different master notebooks.

> For databricks notebook, one may use magic line (`%python` or `%scala`) as desired to switch between languages in a single notebook. **However, for Jupyter notebook, do not put `%python` in any cells of Jupyter notebook; otherwise, the master notebook cannot be extracted.** Actually, as Jupyter notebook only supports Python language (with SageMath kernel), there is no need to put `%python` in any cells anyway. 

> All master notebooks have to be extracted into lecture and assignment notebooks using our system; otherwise, the student submission notebooks cannot be graded in our system !

As seen in the examples of the master notebooks, there are different cell types with different cell tags one needs to strictly specify. Generally, it consists of:-

1. **Lecture Part**
This part includes lecture notes, pictures, VDO, websites, etc. in markdown cells. It can also includes code cells for students to play along with the lecture. Hence, there can be more than one lecture cells and, of course, they can be put in any places in the notebook. These lecture cells do not need any tags, and will be extracted into a lecture notebook.

2. **Assignment Part**
The assignment part is where an instructor creates problems for students as homework. In one assignment, it can include more than one problem, and for each problem, it needs to have 4 cell types along with cell tags as follows:-

	- **Problem Cell**
	In one problem, it can have more than one problem cell. For example, one can specify a problem statement in one markdown cell, and put codes with some parts left blank in a code cell for students to fill in. It can also be a code cell where students have to put in a correct choice, number or string into a variable. These cells will be extracted to an assignment notebook and have to be tagged with the following comment in the first line.
	
		For scala,
		
	    `// ASSIGNMENT 1, Problem 1, Points 2`
    
		For Python,
		
	    `# ASSIGNMENT 1, Problem 1, Points 2`
    > See an example of Problem cell in the example notebooks
    
	- **Test Cell**
	The Test cell is a cell that an instructor has to put in a code for students to run to check whether their codes or answers in a corresponding problem cell is in a correct format or not, e.g. String, Integer, List of Integer, RDD and so on. In one problem, it can have only one Test cell. The Test cell will be extracted along with problem cells to an assignment notebook and has to be tagged with the following comment in the first line.
	
		For scala,
		
	    `// ASSIGNMENT 1, Test 1, Points 2`
	    
		For Python,
		
	    `# ASSIGNMENT 1, Test 1, Points 2`
	    
	    > See an example of Test cell in the example notebooks
    	
	- **TEST Cell**
		The TEST cell is a cell where an instructor has to put test cases, or correct answers for variables. The instructior has to write a code to check whether students' codes satisfy all test cases or students' answers are correct and adjust scores accordingly. In one problem, it can have only one TEST cell. The TEST cell will be appended to students' submission notebooks when grading. The TEST cell has to be tagged with the following comment in the first line.
	
		For scala,

		  // ASSIGNMENT 1, TEST 1, Points 2
		  var local_points = 0
    
		For Python,

		  # ASSIGNMENT 1, TEST 1, Points 2
    	  local_points = 0
    > **NOTE !**
    In every TEST cell, it is necessary to initialize a `local_points` variable to 0, and if the student answer is correct, then add  up a point to this variable, e.g. `local_points = local_points + 2` (in case it is worth 2 points).
    
    > See an example of TEST cell in the example notebooks
    
    
	- **SOLUTION Cell**
		The SOLUTION cell is a cell that an instructor puts correct codes or answers just for a reference. In one problem, it can have only one SOLUTION cell. The SOLUTION cell has to be tagged with the following comment in the first line.
	
		For scala,
  
	    `// ASSIGNMENT 1, SOLUTION 1, Points 2`

		For Python,
		
	    `# ASSIGNMENT 1, SOLUTION 1, Points 2`
    > See an example of SOLUTION cell in the example notebooks

	> **WARNING !!!**
	> Don't be confused between **Test** and **TEST** cells !!!

### Directories for Storing Master Notebooks
One has to export master notebooks and store them in a proper place. For Jupyter notebooks, just export them as *.ipynb*, and for databricks notebooks, export them as *.dbc*. Then, for the locations to store them, go into the course directory. For Jupyter notebooks, save them in `master/jp`, and for databricks notebooks, save them in `master/dbc`.

## Master Notebook Extraction
### Pre-configured file
For the extraction of lecture and assignment notebooks out of a master notebook, one has to pre-configure `config.json` stored inside `AssignmentNotebook` directory (There is also a symbolic link of `AssignmentNotebook` in `GenerateMaterial` directory in your course repository). See an example of the configuration below.

    {
	    "master_notebooks":["A1_MASTER, A2_MASTER, A3_MASTER"],
	    "notebook_file_extension":"db",
	    "notebook_folder":"master/db",
	    "target_notebook_folder":"lectures",
	    "target_notebook_book_folder":"Notebooks",
	    "assignments":[1,2],
	    "CourseID":"58713",
	    "CourseName":"Scalable Data Science and Distributed Machine Learning",
	    "CourseInstance":"2022"
    }

> One must make sure all directories specified in `config.json` exist !

The description of each key in `config.json` is in the table below:-

| Key | Description |
|--|--|
| master_notebooks | A list of names of master notebooks **WITHOUT** any file extension|
| notebook_file_extension | A file extension of master notebooks. This key accepts only two values, namely `db` for databricks notebooks, and `jp` for Jupyter notebooks. |
| notebook_folder | A path to a directory that stores master notebooks. For databricks notebooks, they are stored in `master/db`, and for Jupyter notebooks, they are stored in `master/jp`. Different directory names or paths are also possible, if necessary. |
| target_notebook_folder | A directory where extracted notebooks (both lecture and assignment notebooks) are stored |
| target_notebook_book_folder | A directory where extracted notebooks in a form of e-book are stored (Support only Jupyter notebook)|
| assignments | A list of assignment numbers, which will be extracted into assignment notebooks from the master notebooks |
| CourseID | A course ID on Studium |
| CourseName | A course name on Studium |
| CourseInstance | This is usually a year in which the course is conducted.  |

### Make Lecture Notebook
Go into your course directory, and then into `GenerateMaterial` directory, and run the following command:-

    python3 generate.py

In this case, the extracted lecture notebooks are stored in `lectures` directory as specified in `config.json`.

### Make Assignment Notebook

Go into the course directory, and then into `GenerateMaterial` directory. One needs to open and edit  `generate_assignment.py` and specify in the function `makeAssignmentNotebook` what assignment number and cell types one needs the system to extract from master notebooks. See an example below:-

    course = IDSCourse()
    
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='problem')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='problem_TEST')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='problem_solution')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='problem_solution_TEST')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='solution')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='solution_TEST')
    course.makeAssignmentNotebook(assignment_number = 3,notebook_type='TEST')

In the example above, it creates an object of `IDSCourse()` and calls `makeAssignmentNotebook()` function from the object. It call this function 7 times to generate 7 separate notebooks. For example, the first one generates a notebook with only problem cells (along with their corresponding Test cells). The second one generates a notebook with problem cells (along with their corresponding Test cells) and TEST cells. The third one generates a notebook with problem cells (along with their corresponding Test cells) and solution cells.  

> **Note !**
> Problem cells are always extracted together with their corresponding Test cells.
> This means the first line where 'problem' is specified shall generate a notebook with both problem and Test cells.

> For the argument `notebook_type`, if more than one cell types are needed to be extracted into another notebook, use `underscore` (`_`) to concatenate cell types.

Then, run the following command:-

    python3 generate_assignment.py

In this case, the extracted notebooks are also stored in `lectures` directory as specified in `config.json`.

## Notebook AutoGrader
### Pre-configured files and relevant pre-setup
For the grading of student submission notebooks, one has to pre-configure 3 files as follows:-

1. `config.json`
This `config.json` is the different file from the one used for the notebook extraction. One can find this file in `Autograder` directory. This file is mainly used to specify information needed for the system to grade student submission notebooks.

    	{
	    	"course" : 54514,
			"API_URL" : "https://uppsala.instructure.com",
			"API_KEY" : "1111111~XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
			"API_KEY_OWNER": "Suparerk Angkawattanwit",
			"Assignments" : [
			{
				"name": "ASSIGNMENT_1",
				"studium_id" : 0,
				"master_filename" : "Master/Assignment_1_problem_TEST.dbc",
				"start_date" : "2022-05-01",
				"end_date" : "2022-05-31"
			},
			{
				"name": "ASSIGNMENT_2",
				"studium_id" : 0,
				"master_filename" : "Master/Assignment_2_problem_TEST.dbc",
				"start_date" : "2022-06-01",
				"end_date" : "2022-06-30"
			}
			],
			"dbc_cluster_id": "9999-999999-xxxxxxxxxxxx",
			"dbc_cluster_name": "raazRex-TeachingtestingCluster"
    	}
	
	The description of each key in `config.json` is in the table below:-
	
	| Key | Description |
	|--|--|
	| course | Course ID on Studium. One can find it in the link in your browser's address bar. |
	|  API_URL | Link to Studium (or a link to Canvas Instructure used by your university) |
	| API_KEY | An API key to access Studium. You can create one in the setting menu on Studium |
	| API_KEY_OWNER | (optional) A name of the owner of the API key |
	| Assignments | A list of assignment names and grading periods |
	| name | Assignment name. This must be exactly the same one on Studium. If the system cannot find the name specified here on Studium, nothing is graded. |
	| studium_id | This always has to be set as 0, according to the documentation. |
	| master_filename | A path to an extracted notebook with problem and TEST cells |
	| start_date | A start date to grade the assignment. It starts at 00.01. |
	| end_date | An end date to grade the assignment, It ends at 23.59. |
	| dbc_cluster_id | An ID of a cluster on databricks used to execute student submission notebooks. |
	| dbc_cluster_name | (optional) A name of the cluster on databricks. |
	
> **NOTE !**
> In case there are assignments with overlapped grading periods, the grading system will not start the next one until the current assignment's grading period ends. This means, practically, overlapped grading periods are not supported. However, if it is really necessary to have overlapped grading periods, one can work around by duplicating this set of codes (after extraction where extracted notebooks are present) and running the grading for two assignments separately.

2. `.databrickscfg`
	This `.databrickscfg`file is used by `databricks CLI` in the AutoGrader system to specify a databricks workspace where student submission notebooks are executed and graded. First of all, one needs to install databricks CLI, using the following command:-

	    pip install databricks-cli

	> Use an appropriate version of pip, e.g. pip3
	
	Next, to configure the`.databrickscfg` file, one can find it in a user's home directory. Then, in the file, one has to set a link to a databricks workspace where student submission notebooks are executed, along with its access token. One can create an access token in User Settings on databricks workspace. In the example below, one can find there is a default section where default databricks workspace used to execute is specified. The middle one is the alternative workspace just in case the default one is not working. The last one is actually the default one. It is specified here again just to show the name of the databricks workspace.

		[DEFAULT]
		host = https://uppsalauni-scadamale-ds-projects.cloud.databricks.com/
	    token = dapi11111111111111111111111111111111111
	    jobs-api-version = 2.1
		
	    [dbua-us-west]
	    host = https://dbc-635ca498-e5f1.cloud.databricks.com/?o=445287446643905#
	    token = dapi222222222222222222222222222222222222
	    jobs-api-version = 2.1
       
	    [dbua-eu-west-1]
	    host = https://uppsalauni-scadamale-ds-projects.cloud.databricks.com/
	    token = dapi11111111111111111111111111111111111
	    jobs-api-version = 2.1
	    
3. `.netrc`
This `.netrc`is used when the system downloads graded student notebooks. The system uses `curl` command in a bash script to download the graded notebook from databricks where the access token is needed. Therefore, one also needs to specify the access token here in this file.

	    machine uppsalauni-scadamale-ds-projects.cloud.databricks.com
	    login token
	    password dapi11111111111111111111111111111111111
	    
	    machine dbc-635ca498-e5f1.cloud.databricks.com/?o=445287446643905#
	    login token
	    password dapi222222222222222222222222222222222222
	    
4.  The symbolic link named `Master`
Inside `Autograder` directory, there is a symbolic link named `Master`. This `Master` is linked to the directory where the extracted notebooks in the course are stored (e.g. `../1MS041/GenerateMaterial/lectures`).  One has remove this link and create a new one that is linked to the correct directory by using the following commands:-

	To remove the link:
		
		rm Master

	To create the link and link it to the correct directory:
	

		ln -s ../<...YourCourseDirectory...>/GenerateMaterial/<...YourExtractedNotebookDirectory...>/ ./Master
	
	For example,
	
		rm Master
		ln -s ../1MS041/GenerateMaterial/lectures/ ./Master
	
> For the symbolic link, there is no restriction to name it just only `Master`. A different name for the link can be used. However, one has to change the path in `master_filename`  for each assignment in `config.json` accordingly.

> In addition, one may decide to not use a symbolic link, but create an actual directory here instead, where TEST notebooks are copied and stored.

> The single dot (`.`) in the path means the current directory, while the double dots (`..`) means going up one level to its parent directory.

5. Build `sds-sagemath-autograde` image for the grading of Jupyter notebooks
Go into your course directory and run the following command:-

	    docker build -t sds-sagemath-autograde:latest -f Dockerfile-sds-autograde .
	    
	Then, check if the image is built successfully by using the following command:-

	    docker images

	You will see `sds-sagemath-autograde` in the list of repository.

### Run AutoGrader
In the `Autograder` directory, run the following command:-

    python3 Grader.py

The points, comments (captured from TEST cells) and graded notebooks for student submissions (with commands in all TEST cells removed and left only with their cell results) will be uploaded to Studium as a feedback.

> NOTE ! 
> The grading process starts at `start_date`. After completing grading student submissions for one round, it sleeps for 30 minutes and starts another round of grading again and again (in case students resubmit assignments or have just submitted them for the first time) until the `end_date`.

### Final Notes !
1. databricks does not support any function to skip an error (if any) in student submission notebooks. The AutoGrader system shall give 0 points for a student submission with an error and proceed to the next student submission notebooks. Therefore, students have to make sure there is no error in their submission. To do so, they have to run all the cells including local Test cells to check everything is working fine before submission.
2. For databricks, if the notebook default language is Scala, all the code cells without a magic line (%scala or %python) will be treated as Scala. This also applies to Python.

---
###### Written by Suparerk Angkawattanawit
