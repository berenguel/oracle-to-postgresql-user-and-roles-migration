How to migrate users, roles and grants from Oracle to Azure Database for PostgreSQL using ora2pg 
---

**Introduction**

This document highlights how to migrate oracle users and roles using ora2pg

* step 1: Create an ora2pg migration project

* step 2: Set up the correct ora2pg config file parameters

* step 3: Run ora2pg to export users and grants to a file

* step 4: execute output file ("grants.sql") against Azure Database for PostgreSQL

  

**Estimated Time**
5 minutes



**Pre-Requisites**

* *ora2pg is already installed in either Linux or Windows computer/machine*

* *Azure Database for PostgreSQL is provisioned*

* *An Oracle user with DBA role is necessary to perform this task*
 


###  Step 1: Create an ora2pg migration project ###

This is not a mandatory step. This is a best practice for organising the output script when running ora2pg projects


1. Let's create our project base for the migration:


     - On “**cmd**”,  navigate to your ora2pg folder
        
     ~~~
     cd c:\ora2pg
     ~~~
	 
	- Run the command to create a migration template
     ~~~
     ora2pg --project_base c:\demo -c c:\demo\ora2pg.conf --init_project demo_mig
	 ~~~

**Summary:** In this step, you learnt how to create a project base for the migration project. Note that the path c:\demo has to be created manually.



### Step 2: Set up the correct ora2pg config file parameters ###

Let's setup the ora2pg config file in order to enable users, roles and grants export from the Oracle database

  
   - first, set the connection parameters in the ora2pg file to connect to the desired Oracle database.
   
     ~~~
		ORACLE_HOME	C:\Users\corp\oracle\db_home
		ORACLE_DSN	dbi:Oracle:host=localhost;sid=myoradb;port=1521
		ORACLE_USER	system
		ORACLE_PWD	manager
	 ~~~		

   - After that, set the necessary grant extraction parameters. 
   
	 ~~~
		USER_GRANTS     0
	 ~~~
	
	 ~~~
		TYPE           GRANT
	 ~~~		
		
		OR
	 ~~~
		TYPE	TABLE PACKAGE VIEW GRANT SEQUENCE TRIGGER FUNCTION PROCEDURE TYPE PARTITION FDW MVIEW
	 ~~~	
	 
	 
	Make sure on the TYPE parameter, you include at least GRANT but it works with a list of objects as well.		
		
	


**Summary:** The config file is ready. 
     
### Step 3: Run ora2pg to export users and grants to a file ###



     ~~~
     ~~~
     ora2pg -p -t GRANT -o grant.sql -b c:\demo\demo_mig\schema\grants -c C:\demo\demo_mig\config\demo_conf.conf	
     ~~~
	 ~~~


**Summary:** Navigate to c:\demo\demo_mig\schema\grants and look for grant.sql file.

### Step 4: Run grants.sql against Azure Database for PostgreSQL ###

~~~
psql -h lab8381412.postgres.database.azure.com -p 5432 -U dummydb@lab8381412 -d lab303
~~~
Password is YYYYYYY
~~~
\o 'C:/demo/demo_mig/schema/grants/grants.log'
~~~
~~~
\i 'C:/demo/demo_mig/schema/grants/grants.sql'
~~~
~~~
\q
~~~
**Summary:** The users, roles and permissions are now successfully created in Azure Database for PostgreSQL