# CloudLinux Manager

[[toc]]

<span class="notranslate">CloudLinux Manager</span> is a plugin for most popular control panels including cPanel, Plesk, DirectAdmin and ISPmanager (InterWorx coming soon). It allows you to control and monitor all CloudLinux OS Solo features.

![](/images/solo_lvemanagermainmenu.png)

Log in as administrator to get access to the following functionality:

* <span class="notranslate">Website Monitoring</span> provides access to Website Monitoring and PHP Slow Site Analyzer.
* <span class="notranslate">X-Ray</span> tab - provides access to X-Ray;


## Activation

To start using CloudLinux Manager, it must be activated with your CloudLinux OS Solo activation key.

In order to activate CloudLinux Manager features, follow the next steps: 

* Enter your CLN activation key if you already have it 
    ![](/images/cloudlinuxmanageractivation.png)
  
    You can read more about CloudLinux OS Solo and how to get the license [here](https://lp.cloudlinux.com/cloudlinux-os-solo)

* Sign up and get 30-days trial license

    Fill out the next form

    ![](/images/cloudlinuxmanageractivationsignupform.png)

    CloudLinux Manager will be activated automatically after submitting the form.

    ![](/images/cloudlinuxmanageractivationsignup.png)

    :::tip Note
    After submitting the registration form you will get a trial license valid for next 24 hours.
    To prolong license for 30 days, please follow the activation instructions sent to email specified during the
    registration.
    :::

## Website Monitoring tool

**Website Monitoring tool (WMT)**  is a tool that collects the statistics of the domains' availability and responsiveness, as well as errors that occur when accessing these domains. An admin can get email reports with the statistics. The Website Monitoring tool uses the simple curl request like `curl http://domain.com` to get domains’ statistics.

You can configure the Website Monitoring tool and the [Slow Site analyzer](/manager/#php-slow-site-analyzer) and view the daily reports in the CloudLinux Manager -> Website monitoring tab.

There are Main, PHP Site analyzer, and Settings subtabs [here](/manager/#main).

:::warning Warning
For now, there is no any possibility to remove the `alt-php-ssa` and `cl-web-monitoring-tool` packages so that the _Website monitoring_ tab will be removed.
You can turn off the _Website monitoring_, _PHP Sites Analyzer_ in the _[Settings](/manager/#wmt-settings)_ tabs, so sites statistics will stop collecting and there will be no additional load on the server.
:::

#### Main

This subtab views the latest report (for the last 24 hours) of the Website monitoring tool.

![](/images/WebsiteMonitoringMain.png)

Report is regenerated each time the page is loaded, so all changes in configuration (the [_Settings_](/manager/#wmt-settings) tab) or in the list of domains will be applied immediately.

* **Total number of requests** - requests that were sent to all domains, existing on the servers
* **Successful requests** - the number of requests for all domains with ![](/images/Code200.png)
* **Requests with errors** - the number of requests for all domains which status code is not 200
* **Not started requests due to short check interval** - this metric is used to adjust configuration. If it is not equal 0, an admin should increase the value of Requests sending interval, because the tool does not fit into this interval to send requests to all domains.
* **Slowest websites in 24h and Websites with most errors in 24h** - in these sections you can find the number of domains that was exposed here.

### WMT Settings

To enable or disable **Website Monitoring**, use the following slider.

![](/images/WebsiteMonitoringSlider.png)


* **Email to send report** - all email notifications will be sent to this email address

:::warning Warning
If email is not specified, all [notifications](/manager/#wmt-email-notifications) will be sent to control panel administrator's email.
:::

![](/images/Webmonitoringtoolsettings.png)

* **Check every X minutes** - this is a period in minutes between requests to the same domain.
* **Time allowed for response** - if there is no answer from the website for this period of time, the Website Monitoring tool will regard this behaviour as the `HTTP 408` error.
* **Enable summary notifications** - turn on/off summary daily email report notifications
* **Enable alert notifications** - turn on/off immediate alert email notifications
* **Domains and URLs Ignore List** - domains and URLs that will not be requested and displayed in reports; supported formats: `example.com, http://example.com`

### WMT CLI

The `wmt-api-solo` utility allows managing Website Monitoring tool via CLI.

**Usage**

```
# /usr/share/web-monitoring-tool/wmtbin/wmt-api-solo [command] [--optional arguments]
```

**Optional arguments**:

| | |
|-|-|
|`-h`, `--help`|show help message and exit|

**Commands**:

| | |
|-|-|
|`config-change`|set the WMT configuration using the JSON string that follows the command|
|`config-get`|get the WMT configuration as JSON|
|`report-get`|generate a report for last 24 hours|
|`send-clickhouse`|send the summary report to the ClickHouse|
|`start`|start the WMT system|
|`status`|check the status of the WMT system|
|`stop`|stop the WMT system|

Example of the `/usr/share/web-monitoring-tool/wmtbin/wmt-api-solo` command usage:

```
# /usr/share/web-monitoring-tool/wmtbin/wmt-api-solo --config-change "{\"ping_timeout\":10,\"ping_interval\":5,\"report_email\":\"user@example.com\"}"
```

This way you can set all or only certain parameters.

### WMT email notifications

There are two types of emails sent by the Web Monitoring tool: **daily email report** and **alert**.

#### Daily email report

The summary report with all data for the previous day. It is created and sent every day at midnight.


**Example of the Web Monitoring tools daily report**.

![](/images/Webmonitoringtoolemail.png)

#### Alert

Domains that are responding with non-200 status code will be re-requested in 5 minutes or in the `ping_interval` setting (in case it is less than 5 minutes).

If domains are still responding with non-200 status code - Web Monitoring tool will inform about such domains with immediate alert email.

**Example of the Web monitoring tools immediate alert**.

![](/images/Webmonitoringtoolalert.png)

:::tip Note
The next alert with domains will not be sent in less than 6 hours. Also, if the alerted domain is still responding with non-200 status code even after 6 hours - it will not be re-alerted until it responds with 200 status code at least once and becomes unavailable again.
:::

## PHP Slow Site analyzer

**PHP Slow Site analyzer** (**SSA**) is a tool that generates daily reports for the server administrator with information about the top N slow PHP-based URLs for all domains. The Slow Site analyzer tracks all PHP-based requests and selects slow ones by specific rules.

![](/images/solo_SlowSiteAnalyzerReport.png)

This is an example of a report from the Slow Site analyzer. The report shows the number of slow requests per domain and its URLs and the average duration of each slow URL.


### SSA settings

To enable or disable the **Slow Site analyzer**, use the following slider:

![](/images/WebsiteMonitoringSlider1.png)

The following settings can be changed through the UI:

![](/images/solo_SlowSiteAnalyzerSettings.png)

* **Enable summary notifications** - turn on/off summary daily email report notifications;
* **Top slow URLs** - this number (N) will be used to select the top N URLs for each domain, marked as slow;
* **Slow request duration** - the duration of a request in seconds. If the request duration was less than specified in this setting then this request will not be marked as slow;
* **Domains and URLs Ignore List** - domains and URLs that will not be displayed in reports.



### SSA CLI

The `cloudlinux-ssa-manager` utility allows to manage Slow Site analyzer via CLI.

**Usage**

```
# /usr/sbin/cloudlinux-ssa-manager [command] [--optional arguments]
```

**Optional arguments**:

| | |
|-|-|
|`-h`, `--help`|show help message and exit|

**Commands**:

| | |
|-|-|
|`set-config`|set the SSA configuration|
|`get-config`|get the SSA configuration|
|`get-ssa-status`|get a current status of SSA|
|`enable-ssa`|enable SSA|
|`disable-ssa`|disable SSA|
|`get-report`|get the latest report|

You can use the `-h`, `--help` option with commands to get a full list of available optional arguments for each command.

Example of the `/usr/sbin/cloudlinux-ssa-manager get-report | json_reformat` command output:

```
# /usr/sbin/cloudlinux-ssa-manager get-report | json_reformat
{
    "result": "success",
    "date": "18.05.2021",
    "domains": [
        {
            "name": "user1.com",
            "slow_urls": 2,
            "slow_reqs": 23,
            "total_reqs": 24,
            "urls": [
                {
                    "name": "http://user1.com/",
                    "reqs_num": 13,
                    "average_duration": 3004509
                },
                {
                    "name": "http://user1.com/info.php",
                    "reqs_num": 10,
                    "average_duration": 3641018
                }
            ]
        },
        {
            "name": "user2.com",
            "slow_urls": 1,
            "slow_reqs": 12,
            "total_reqs": 12,
            "urls": [
                {
                    "name": "http://user2.com/",
                    "reqs_num": 12,
                    "average_duration": 3010571
                }
            ]
        }
    ]
}
```

Example of the `/usr/sbin/cloudlinux-ssa-manager set-config --requests-duration 2.5` command output:

```
# /usr/sbin/cloudlinux-ssa-manager set-config --requests-duration 2.5
{"result": "success"}
```


### SSA email notifications

SSA sends email reports daily if the "**Enable summary notifications**" setting is turned on.

**Example of the PHP Slow Site analyzer report**:

![](/images/solo_SlowSiteAnalyzerEmailNotifications.png)


## X-Ray

* [Description](/manager/#description)
* [How to manage X-Ray](/manager/#how-to-manage-x-ray)
* [Managing tracing task](/manager/#managing-tracing-task)
* [Managing continuous tasks](/manager/#managing-continuous-tasks)
* [End-user X-Ray plugin](/manager/#end-user-x-ray-plugin)
* [X-Ray client](/manager/#x-ray-client)
* [X-Ray service](/manager/#x-ray-agent)
* [FAQ](/manager/#faq)

### Description

X-Ray is a tool developed for website performance monitoring and performance issues detection.

X-Ray can gather and visualize information about top N slowest system functions, external requests, software modules and database queries of the website.

### How to manage X-Ray

X-Ray provides two options for monitoring domain requests speed: Tracing task and Continuous task.

* **Tracing task** is a task created manually for a specific URL to collect server requests. The task will end either after a specified number of requests to the URL or after a specified time (maximum after two days). It is not possible here to automatically email a report but it is possible to export the report in PDF and send to a user.

* **Continuous task** is a task that initiates a daily hourly tracing requests for a specified domain and email a monitoring report. Continuous task can't stop automatically, you need to stop it manually.

In fact, continuous task allows to automatically create a tracing task for each new day, with the ability to get a report for the past day.

#### Tracing tasks tab

The *Tracing tasks* tab contains a list of all tracing tasks created both manually and automatically via continuous tasks.

![](/images/XRayTracingTaskCreated.png)

The *Created* column shows how a task was created – automatically (by continuous task) or manually.

#### Continuous tracing tab
  
The *Continuous tracing* tab contains a list of continuous tasks for which tracing tasks will be created automatically for a new day for a specific domain.

![](/images/XRayContinuousTasksList.png)

### Managing tracing task

#### Creating a new tracing task

1. Go to the <span class="notranslate">_X-Ray_</span> tab
2. Click the <span class="notranslate">_Start tracing_</span> button to create a new task
3. In the opened popup specify a website URL to trace
4. Click the <span class="notranslate">_Run_</span> button
5. Tracing will run in the default mode. In the default mode <span class="notranslate">X-Ray</span> traces the first 20 requests for a specified URL

![](/images/XRayTracingTask.png)

* <span class="notranslate">**URL**</span> should be a valid URL of the domain which exists on the current hosting server. The URL field supports wildcard matching. To learn more about wildcard matching, click _How to use special characters_.
* <span class="notranslate">**Advanced settings**</span> allow you to set an IP address and tracing options: by time or by number of queries.

    ![](/images/XRayAdvanced.png)

**Advanced settings**

* <span class="notranslate">**Client’s IP**</span>: it is an IPv4 address of a machine to trace. For example, if you have a production website that processes requests from different IP addresses and you do not want to add these requests to the tracing task. So, you can set a specific IP address and <span class="notranslate">X-Ray</span> will analyze requests only from this specific IP address.
Record for
* <span class="notranslate">**Time period**</span>: how much time <span class="notranslate">X-Ray</span> collect the requests (2 days max)
* <span class="notranslate">**Requests**</span>: the amount of requests that <span class="notranslate">X-Ray</span> will collect

After creating, the task appears in the list of tracing tasks.

![](/images/XRayTrcingTaskList.png)

#### Viewing tracing tasks list

![](/images/XRayTrcingTaskList1.png)

Tasks created *Manually* are simply tracing tasks.

#### Tracing status

A tracing task can have the following statuses:

* <span class="notranslate">**Running**</span> – tracing is in progress
* <span class="notranslate">**Stopped**</span> – tracing was stopped by administrator
* <span class="notranslate">**On hold**</span> – the same URL already exists in the lists. Task processing will not start automatically. Administrator should start it manually.
* <span class="notranslate">**Completed**</span> – period of time is finished or number of requests is reached.

#### Collected requests for tracing task

:::warning Warning!
Collected requests are available in the UI for two weeks.
:::

Click ![](/images/XRayView.png) to open a list of collected requests.

#### Tracing tasks

![](/images/solo_XRayCollectedRequests.png)

The slowest request is highlighted.

![](/images/XRaySlowestRequest.png)

* <span class="notranslate">**Total**</span> displays how many requests were collected according to tasks requirements.
* <span class="notranslate">**Pending**</span> displays how many of collected requests are not visible in the table yet.
* <span class="notranslate">**Slow**</span> displays the number of requests lasting more than one second.

There are filters for the request types and the indicator of a filter used now.

![](/images/FilterIndicator.png)

If slow requests were not detected during the tracing task, the following is displayed. Here, you can also view all requests.

![](/images/solo_RecordedSession.png)


<span class="notranslate">X-Ray</span> collects the following data for each request:

* <span class="notranslate">**Top issues**</span> – the slowest items of a request
* <span class="notranslate">**Software modules/plugins**</span> by execution time (only for WordPress plugins)
* <span class="notranslate">**Database queries**</span> by execution time 
* <span class="notranslate">**External requests**</span> by execution time
* <span class="notranslate">**Other system functions**</span> by execution time 

#### Software modules/plugins

![](/images/XRaySoftwareModulesPlugins.png)

The <span class="notranslate">_Software modules/plugins_</span> section displays the following data:

* <span class="notranslate">**Software type**</span> – a type a module/plugin. For now, <span class="notranslate">X-Ray</span> can analyze only WordPress software
* <span class="notranslate">**Software module**</span> – a name of the WordPress plugin
* <span class="notranslate">**Duration**</span> – plugin execution time
* <span class="notranslate">**Duration (%)**</span> – plugin execution time as a percentage of the total duration of the request

#### Database queries

![](/images/XRayDatabaseQueries.png)

The <span class="notranslate">_Database queries_</span> section displays the following data:

* <span class="notranslate">**Query**</span> – the executed SQL-query
* <span class="notranslate">**File**</span> – the file and the line of the executed query and backtrace
* <span class="notranslate">**Software module**</span> – a WordPress plugin name from which the request was completed. If the request does not belong to any of the WordPress plugin, the name of the function that executed the given request is displayed
* <span class="notranslate">**Calls**</span> – the number of identical SQL queries
* <span class="notranslate">**Duration**</span> – execution time as a percentage of the total duration of a request and the function processing time (in brackets)
 
#### External requests

![](/images/XRayExternalRequests.png)

The <span class="notranslate">_External requests_</span> section displays the following data:

* <span class="notranslate">**URL**</span> – the URL of the executed request
* <span class="notranslate">**File**</span> – the file and the line of the executed request and backtrace
* <span class="notranslate">**Duration**</span> – execution time as a percentage of the total duration of a request and the function processing time (in brackets)
 
#### System functions

![](/images/XRaySystemFunctions.png)

The <span class="notranslate">_System functions_</span> section displays the following data:

* <span class="notranslate">**Function**</span> – the executed function
* <span class="notranslate">**File**</span> – the file and the line of the executed request
* <span class="notranslate">**Duration**</span> – execution time as a percentage of the total duration of a request and the function processing time (in brackets)

#### Stopping tracing task

Click ![](/images/XRayStop.png) to stop the tracing task.

![](/images/XRayStopped.png)

The tracing task status will be changed to <span class="notranslate">**Stopped**</span>. Data will not be collected anymore but you can see already collected information or continue tracing later by clicking ![](/images/XRayStart.png).

#### Deleting tracing task 

Click ![](/images/XRayDelete.png) to delete the tracing task.

:::warning Warning!
When you have deleted a tracing task, all collected data will be unavailable.
:::

### Managing continuous tasks

#### Creating a new continuous task

1. Click the *Create continuous tracing*  button 

![](/images/XRayCreateContinuousTaskBtn.png)

2. Specify URL in the *Domain* field and email in the *Email for reports* field and click the *Create* button.

![](/images/XRayCreateContinuousTaskForm.png)

3. You can see a new task in the *Continuous tracing* tab in the X-Ray UI.

![](/images/XRayContinuousTracingTab.png)

4. If you stop a continuous tracing task, a new task for the next 24 hours will not be created. The task for the current day will be finished at midnight and the report will be emailed.

5. If you delete a continuous tracing task, the task for the current day will be finished at midnight and the report will be emailed.

#### Viewing continuous tasks list

You can find a list of continuous tracing tasks in the _Continuous tracing_ tab.

![](/images/XRayContinuousTracingTasksList.png)

You can find automatically created tasks in the _Tracing tasks_ tab marked as _Automatically_ in the _Created_ column.

![](/images/XRayContinuousTracingTasksListCreated.png)

The [statuses for automatically created tasks](/manager/#tracing-status) are the same as for tracing task.

To view detailed info about an automatically created task, click ![](/images/XRayView1.png). You will get requests grouped by hour.

![](/images/XRayContinuousTracingTasksListGrouped.png)

Click to a group to open a list of the requests.

![](/images/XRayContinuousTracingTasksRequestsList.png)

The following data is collected for each request:

* Software modules/plugins by execution time (only for WordPress plugins)
* Database queries by execution time
* External requests by execution time
* Other system functions by execution time

#### Stopping automatic tracing task

Stopping automatic tracing task (a part of continuous tracing task) affects only the automatic tracing task for the current day. A new task for the next day will be created at the end of the day.

To stop the continuous tracing task completely, see [Creating a new continuous task, paragraph 4](/manager/#creating-a-new-continuous-task).


#### Deleting automatic tracing task

Deleting automatic tracing task (a part of continuous tracing task) affects only the automatic tracing task for the current day. A new task for the next day will be created at the end of the day.

To delete the continuous tracing task completely, see [Creating a new continuous task, paragraph 5](/manager/#creating-a-new-continuous-task).


#### Continuous task daily report

1. Users get daily reports on their emails. An example of a report is shown below:

    ![](/images/XRayContinuousTaskDaylyReportExample.png)

2. Click the link in the email to show the detailed report:

    ![](/images/XRayContinuousTaskDaylyReportCollectedRequests.png)

3. You can view requests grouped by hour:

    ![](/images/XRayContinuousTaskDaylyReportByHourRequests.png)

4. You can also view the detailed information about request:

    ![](/images/XRayContinuousTaskDaylyReportRequestDetails.png)


### End-user X-Ray plugin

The end-user X-Ray plugin is enabled by default on CloudLinux OS Solo.

![](/images/solo_XRayEndUserPluginUIIcon.png)

#### How to manage the end-user X-Ray plugin

The web interface of the end-user X-Ray plugin is almost the same as the X-Ray administrator interface.
 
![](/images/XRayEndUserUI.png)

But there are some differences and they are described further.

* End-users can create tasks only for their domains from the drop-down list:
    ![](/images/XRayEndUserUIStart.png)
* To specify URL or wildcard, end-users should use the input field next to the domain:
    ![](/images/XRayEndUserUiSpecifyURL.png)
 
You can read about all other basic interface elements and managing tracing tasks in the [Managing tracing task section](/manager/#managing-tracing-task).

:::warning Note
Tracing tasks created by an end-user will also be displayed in the administrator interface and administrators can manage the end-user's tasks the same way as they manage their own. At the same time, tasks created by the administrator or other end-users will not be displayed in the UI of the current user.
:::

#### End-user X-Ray plugin limitations

* The end-user X-Ray plugin does not support creating continuous tasks.
* The administrator and the end-user can’t run the tracing task for the same Domain/URL at the same time. Once the administrator started a specific tracing task, the end-user will not be able to duplicate it. And the same is true for the administrators – they will just see the running task for the specific domain and see the notification that they're trying to create a tracing task with a duplicated URL.
* If continuous tracing is enabled for the domain, the end-user will not be able to create a new task for this domain because the same rule works - it will be a duplicate of the existing tracing tasks. The next warning will appear:

    ![](/images/XRayEndUserUIWarning.png)

    To solve this, the existing running tasks for the same Domain/URL should be stopped or completed. You can find more details about this in the [FAQ](/manager/#what-should-i-do-if-i-see-the-warning-task-is-duplicated-by-url).


### X-Ray client

X-Ray client is a PHP extension named `xray.so`. It analyzes the processing time of the entire request and its parts and then sends the data to the X-Ray agent.

#### List of supported PHP versions

The list of currently supported PHP versions:

| | | | | |
|-|-|-|-|-|
|**ALT PHP**:|**EA PHP**:|**Plesk PHP**|**DirectAdmin PHP**|**Other panels PHP**|
| <ul><li>alt-php54</li><li>alt-php55</li><li>alt-php56</li><li>alt-php70</li><li>alt-php71</li><li>alt-php72</li><li>alt-php73</li><li>alt-php74</li></ul>|<ul><li>ea-php54</li><li>ea-php55</li><li>ea-php56</li><li>ea-php70</li><li>ea-php71</li><li>ea-php72</li><li>ea-php73</li><li>ea-php74</li></ul>|<ul><li>php54</li><li>php55</li><li>php56</li><li>php70</li><li>php71</li><li>php72</li><li>php73</li><li>php74</li></ul>|<ul><li>php54</li><li>php55</li><li>php56</li><li>php70</li><li>php71</li><li>php72</li><li>php73</li><li>php74</li></ul>|<ul><li>54</li><li>55</li><li>56</li><li>70</li><li>71</li><li>72</li><li>73</li><li>74</li></ul>|


#### Functions that X-Ray client can hook

#### Database queries

* Functions from the [MySQL](https://www.php.net/manual/ru/book.mysql.php) extension:
    * <span class="notranslate">`mysql_query`</span>
    * <span class="notranslate">`mysql_db_query`</span>
    * <span class="notranslate">`mysql_unbuffered_query`</span>
* Functions from the [MySQLi](https://www.php.net/manual/ru/book.mysqli.php) extension:
    * <span class="notranslate">`mysqli_query`</span>
    * <span class="notranslate">`mysqli::query`</span>
    * <span class="notranslate">`mysqli_multi_query`</span>
    * <span class="notranslate">`mysqli::multi_query`</span>
    * <span class="notranslate">`mysqli_real_query`</span>
    * <span class="notranslate">`mysqli::real_query`</span>
* Functions from the [PDO](https://www.php.net/manual/ru/book.pdo.php) extension:
    * <span class="notranslate">`PDO::exec`</span>
    * <span class="notranslate">`PDO::query`</span>
    * <span class="notranslate">`PDOStatement::execute`</span>

#### External requests

* Function [curl_exec](https://www.php.net/manual/ru/function.exec)

#### System PHP functions

It may be any PHP system function which can be related to a PHP engine or other PHP extension, for example <span class="notranslate">`fopen()`</span> or <span class="notranslate">`json_encode()`</span>. A list of these functions can be found [here](https://www.php.net/manual/en/indexes.functions.php).

#### Configuration Options

<div class="notranslate">

#### xray.enabled

</div>

**Syntax**: <span class="notranslate">`xray.enabled=On/Off`</span>

**Default**: <span class="notranslate">On</span>

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: Enable or disable <span class="notranslate">X-Ray</span> extension from php.ini

-----

<div class="notranslate">

#### xray.database_queries

</div>

**Syntax**: <span class="notranslate">`xray.database_queries=[number]`</span>

**Default**: 20

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: The number of the slowest SQL queries which will be sent to the <span class="notranslate">X-Ray</span> agent. The min value is 0 and the max value is 100. If the variable value is more, the default value will be used.

-----

<div class="notranslate">

#### xray.external_requests

</div>

**Syntax**: <span class="notranslate">`xray.external_requests=[number]`</span>

**Default**: 20

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: The number of the slowest external requests (the curl_exec function) which will be sent to the <span class="notranslate">X-Ray</span> agent. The min value is 0 and the max value is 100. If the variable value is more, the default value will be used.

-----

<div class="notranslate">

#### xray.system_functions

</div>

**Syntax**: <span class="notranslate">`xray.system_functions=[number]`</span>

**Default**: 20

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: The number of the slowest system functions which will be sent to the <span class="notranslate">X-Ray</span> agent. 
The min value is 0 and the max value is 100. If the variable value is more, the default value will be used.

-----

<div class="notranslate">

#### xray.backtrace_depth

</div>

**Syntax**: <span class="notranslate">`xray.backtrace_depth=[number]`</span>

**Default**: 10

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: The backtrace depth to the main() function which will be sent to the <span class="notranslate">X-Ray</span> agent. The min value is 0 and the max value is 20. If the variable value is more, the default value will be used.

-----

<div class="notranslate">

#### xray.processor

</div>


**Syntax**: <span class="notranslate">`xray.processor=[processor_name]`</span>

**Default**: <span class="notranslate">xray</span>

**Changeable**:  <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: Tells the <span class="notranslate">X-Ray</span> client which processor to use. The new processors may be added in the future. The default processor is xray which means to send data to the <span class="notranslate">X-Ray</span> agent.

-----

<div class="notranslate">

#### xray.tasks

</div>

**Syntax**: <span class="notranslate">`xray.tasks=host:uri:ip:id`</span>

**Default**: <span class="notranslate">no value</span>

**Changeable**:  <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: The current tracing tasks for the given PHP request. This directive is added automatically by the <span class="notranslate">X-Ray</span> manager when creating a task. It is not allowed to edit manually, as <span class="notranslate">X-Ray</span> may stop working.

-----

<div class="notranslate">

#### xray.to_file

</div>

**Syntax**: <span class="notranslate">`xray.to_file=On/Off`</span>

**Default**: <span class="notranslate">Off</span>

**Changeable**:  <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: Only for debug purposes. Writes to a file data which is sent to the processor.

-----

<div class="notranslate">

#### xray.debug

</div>

**Syntax**: <span class="notranslate">`xray.debug=On/Off`</span>

**Default**: <span class="notranslate">Off</span>

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: Only for debug purposes. Enables debug output during request processing. In the On mode can slow down the domain.

-----

<div class="notranslate">

#### xray.debug_file

</div>

**Syntax**: <span class="notranslate">`xray.debug_file=[path_to_file]`</span>

**Default**: <span class="notranslate">`/tmp/xray-debug.log`</span>

**Changeable**: <span class="notranslate">PHP_INI_SYSTEM</span>

**Description**: Only for debug purposes. Specifies a file for logging debug information.


### X-Ray agent 


This is a service that receives data from the <span class="notranslate">X-Ray</span> client and sends it to the remote storage.

#### Managing X-Ray service

The <span class="notranslate">X-Ray</span> agent is managed by the <span class="notranslate">`service`</span> utility.

* To start the <span class="notranslate">X-Ray</span> agent, run the following command:

    <div class="notranslate">

    ```
    # service xray-agent start
    ```
    </div>

* To stop the <span class="notranslate">X-Ray</span> agent, run the following command:

    <div class="notranslate">

    ```
    # service xray-agent stop
    ```
    </div>

* To restart the <span class="notranslate">X-Ray</span> agent, run the following command:

    <div class="notranslate">

    ```
    # service xray-agent restart
    ```
    </div>

### FAQ

#### Does X-Ray affect website performance?

<span class="notranslate">X-Ray</span> affects website performance. Our tests show 5-10 % overhead from a website loading time with <span class="notranslate">X-Ray</span> tracing enabled.
<span class="notranslate">X-Ray</span> allows you to find website performance issues and should not be enabled permanently. If your website is very slow, you can enable <span class="notranslate">X-Ray</span> to find the cause and then disable it.

#### What should I do if I see the warning "Task is duplicated by URL"?

This warning means that you already have a task to trace this URL in the list of your tracing tasks. If you see this warning, a new task can be created only with the <span class="notranslate">_On hold_</span> status and you will be able to run it only when the previous task with the same URL will be completed.

Note that the URL field supports wildcard matching and you can have a case when <span class="notranslate">X-Ray</span> is tracing the <span class="notranslate">`URL=domain.com/*`</span> and you are trying to create a new task with <span class="notranslate">`URL=domain.com/xray.php`</span>. In this case, you will see that warning because the `*` URLs array includes <span class="notranslate">`xray.php`</span>.

####  I started a tracing task and made requests to URL but did not see any results in the UI. What should I do?

1. Check that <span class="notranslate">**xray**</span> extension is enabled for the domain. To do so, go to the <span class="notranslate">`phpinfo()`</span> page and make a request. In the phpinfo output try to find the following section:
   
    ![](/images/XRayPHPInfo.png)

If you cannot see that section, try to restart PHP processes for that user (the simplest way is to restart Apache) and check that you can see the <span class="notranslate">**xray**</span> extension.


2. If you can see the <span class="notranslate">**xray**</span> extension in the phpinfo, check that <span class="notranslate">X-Ray</span> agent service is running with the service xray-agent status command. If it is not running, start it with the <span class="notranslate">`service xray-agent start`</span> command.
3. <span class="notranslate">X-Ray</span> may not send data if a site uses a caching plugin, as the caching plugin is outputting HTML, thus there are no PHP scripts to examine. We encountered such issues with sites that use <span class="notranslate">LSCache</span> and <span class="notranslate">WP Super Cache</span> plugins. Check that your site does not use caching plugins. If so, disable it while tracing a site to get information from <span class="notranslate">X-Ray</span>.
4. If you set a client’s IP when creating the tracing task, check that your requests come to the server with this IP via phpinfo (since there may be NAT between your local machine and the server).
   
    ![](/images/XRayPHPInfoRemoteAddr.png)

5. If, after checking the previous items, the issue persists, [contact our support team](https://cloudlinux.zendesk.com/hc/en-us/requests/new).

#### What to do if X-Ray is not found in the phpinfo() page?

If you managed to create a tracing task, this means that the <span class="notranslate">`xray.ini`</span> file was created in a system. Therefore, there may be two reasons why it did not appear in the phpinfo page of the domain.

1. PHP process wasn't reloaded after adding the xray.ini. To solve this, you should restart the Apache or fpm service for the domain on which the tracing was started. At the moment, this is done automatically by the <span class="notranslate">X-Ray</span> manager after creating the task.
2. Your domain uses a PHP version different from the one which was detected by the <span class="notranslate">X-Ray</span> manager. To solve this, check the scan dir for additional ini files for your domain.

    ![](/images/XRayScanDir.png)

    Then check the <span class="notranslate">`ini_location`</span> that was passed to the <span class="notranslate">X-Ray</span> manager by running the following command:

    <div class="notranslate">

    ```
    # cat /usr/share/alt-php-xray/manager.log | grep ini_location
    ```
    </div>

    Find your tracing task in the output and check that the <span class="notranslate">`xray.ini`</span> exists in this directory, also check that the `ini` path is the same in the phpinfo page output and in the <span class="notranslate">`ini_location`</span> directive for your tracing task. If they are the same, you should reload your PHP. If they are different that means that the <span class="notranslate">X-Ray</span> manager could not correctly determine the PHP version your domain uses. In this case, contact our support team at [https://cloudlinux.zendesk.com/hc/requests/new](https://cloudlinux.zendesk.com/hc/requests/new).


#### I use LiteSpeed, X-Ray is enabled and it is shown in the phpinfo() page but does not collect data when sending requests to a site. What to do?

Check for the <span class="notranslate">`CacheLookup on`</span> option in the `htaccess` file for your domain.
If the option is there, LiteSpeed processes requests bypassing the PHP X-Ray extension.
In this case, to get tracing information, you should remove the <span class="notranslate">`CacheLookup on`</span> option.

#### What is the proper format for the URL?

All of the examples below are correct:

* `http://domain.com`
* `http://domain.com/`
* `https://domain.com`
* `https://domain.com/`

You can use any of them with a prefix `www.` and it is also correct.

