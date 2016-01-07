Easy web reports helps you create Reports  on the fly for your webapps (Java Webapps to start with) . Once you integrate this to your project creating any report becomes as simple as writing an SQL Query.

SEE THE **[LIVE DEMO HERE](http://webreports.tez.cloudbees.net/reports/)**


#Helps you to create reports for your webapps on the fly.

# Introduction #

Easy Web Reports framework is a combination of Client side(js) and Server side libraries(java classes) which takes care of the User Interface,async requests,database access,report rendering etc .So the developer just need to specify the queries and the magic happens .The Grid representation is powered by http://www.dhtmlx.com DHTMLX Library.

# Features #

  * Easy to integrate
  * Fully Asynchronous
  * Works with all databases.
  * Built with AngularJS.

# Get Started #


  * Download the [demo-reports.war](http://code.google.com/p/easy-webreports/downloads/list) file and deploy it in your application server
  * Import the sql(MySql) file [grid.sql](http://code.google.com/p/easy-webreports/downloads/list) to your database.You can find this file in the root folder of the application or in Downloads.
  * Configure your database properties **"WEB-INF/classes/jdbc.properties"**.
  * Restart your application server and go to http://localhost:8080/demo-reports to see the Demo .

# Reports Customization #
> Before starting our reports customization,let's know about 3 important files in the application.
  * index.html ==> The view page for reports.
  * JsonService.jsp ==> Service page which returns the data as Json.
  * reportCtrl.js  ==> Is the controller page,which is responsible for handling requests,making async calls,handling responses,handling errors etc.

We've a 2 level navigation menu defined in a json called **navigationMenu** in the file **reportCtrl.js**.First level is the report Section and the second  is List of reports in that section.Change the labels as per your needs.Each report Item  should have an unique Id.This Id will be used as a reference to the Sql Query in **JsonService.jsp**.
So When any report link is clicked the Id  will be passed to **JsonService.jsp** and corresponding sqlQuery will be executed and data will be returned as Json.


So change the labels of the items or add items of your own or add sections of your own in the json and add sql Queries for the respective reportIds.That's all folks.


# Adding HTML elements in reports #

Just add it to your sql query in JsonService.jsp

```
  * select foo,bar,concat("<a href='#'>Edit<a>") edit  from table
  * select foo,bar,concat("<button>Click Me<button>") clickme from table
```

_Don't forget to add alias to the column names if you use any sql functions like IF,CONCAT(),DATE\_FORMAT() etc otherwise the report will not be rendered propery_


# Adding Filters,Column Sortings to reports #

You do that in JsonService.jsp .
```
case 101:
     sqlQuery="select foo,bar,test from table";
     filters="#text_filter,#select_filter";
     colSorts="str,str";
break;

This create text_filter for first column,select filter for the second and String sorting for the two columns.

If you dnt wanna create filters or sorting for any column just leave it empty as shown below.

case 101:
     sqlQuery="select foo,bar,test from table";
     filters="#select_filter,,#select_filter";
     colSorts="str,,str";
break;
This create filters for 1st and 3rd columns.


```
  1. Available filters : #text\_filter,#select\_filter.
  1. Available Sortings: int,str,date .(Integer,String,date).

Refer http://docs.dhtmlx.com/doku.php?id=dhtmlxgrid:toc for more information

# How to Integrate to your Project #

  * copy the directories codebase,reports to your project root directory.
  * Add jars report-utils.jar,xml2excel.jar,jxl.jar,json-simple-1.1.1.jar to /WEB-INF/lib
  * Add the following servlet to web.xml

```
        <servlet>
	<servlet-name>ExcelGenerator</servlet-name>
	<servlet-class>com.dhtmlx.xml2excel.ExcelGenerator</servlet-class>
        </servlet>
	<servlet-mapping>
	<servlet-name>ExcelGenerator</servlet-name>
	<url-pattern>/generate.do</url-pattern>
	</servlet-mapping>
```
  * Create an Sql connection object with your own implementation in JsonService.jsp.
  * Now it's ready to use.