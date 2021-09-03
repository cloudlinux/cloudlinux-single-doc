# Introduction

[[toc]]

## What is CloudLinux OS Solo


CloudLinux OS Solo is designed for installation on VPS or dedicated servers with a single hosting account for any number of websites.

Unlike [CloudLinux OS Shared](https://docs.cloudlinux.com/introduction/), this edition is not designed for Shared Hosting usage and it does not include any [LVE-related](https://docs.cloudlinux.com/lve_manager/) features.

We provide the CloudLinux OS Solo with pre-installed features, but for grafical interface, please install a control panel on your server.

:::warning
Currently supported OS version is CloudLinux OS Solo 8 only. Therefore, for now, there is no possibility to use CloudLinux OS Solo on servers with OS version 6 and 7.
Please refer to the [currently supported OS versions](/introduction/#cloudlinux-os-solo-lifecycle).
:::

#### Supported control panels

|**Panel**|**Supported**|
|:-:|:-:|
|cPanel Solo|Yes|
|cPanel regular with one user|Yes|
|Plesk|No (coming soon)|
|DirectAdmin|Yes|
|Other panels|No|
|No control panel|No|

#### CloudLinux OS editions comparison

|**Features**|**CloudLinux OS Solo**|**CloudLinux OS Shared**|**CloudLinux OS Shared Pro**|
|:-:|:-:|:-:|:-:|
|Maximum amount of hosting accounts|1|unlimited|unlimited|
|Limits (LVE Manager)|No|**Yes**|**Yes**|
|Cage FS|No|**Yes**|**Yes**|
|MySQL Governor|No|**Yes**|**Yes**|
|Selectors (PHP, Python, Node.js, Ruby)|No|**Yes**|**Yes**|
|HardenedPHP|No|**Yes**|**Yes**|
|Apache mod_lsapi PRO|No|**Yes**|**Yes**|
|SecureLinks|No|**Yes**|**Yes**|
|Website monitoring tool|**Yes**|**Yes**|**Yes**|
|Website monitoring alerts|**Yes**|No|No|
|Slow Site analyzer|**Yes**|**Yes**|**Yes**|
|PHP X-Ray|**Yes**|No|**Yes**|
|Centralized Monitoring|No|No|**Yes**|
|WordPress Optimization Suite|**Yes**|No|**Yes**|
|Support 24/7|**Yes**|**Yes**|**Yes**|
|Priority support|No|No|**Yes**|

### Benefits

We believe that this distribution will be useful for you if: 
1. as a site owner you have different performance issues with websites hosted. 
   CloudLinux OS Solo provides a set of tools for website monitoring and performance tracking out-of-box;
2. as a service provider you sell VPS servers and some of your customers require OS with extensive features 
   for better security, support, and performance.

In any case we provide you [24/7 professional support](https://cloudlinux.zendesk.com/)
to resolve any issues related to CloudLinux OS, its unique components and kernel.

### CloudLinux OS Solo features
 
We've pre-installed a couple of useful features in our distribution.

* **CloudLinux Manager**

  CloudLinux Manager is a plugin which allows you to control all the
  CloudLinux settings from an easy-to-use web UI integrated into your control panel. 
  
  The sub-modules you can find in CloudLinux Manager are Website Monitoring tool, PHP Slow Site analyzer, and X-Ray.

  [See more](/manager/).

* **Website Monitoring tool**

  Website Monitoring tool is a tool that allows you to track your websites uptime 
  and receive notifications about problems with access.

  Website Monitoring tool can gather and display information regarding the slowest websites on the server, 
  notify by emails about errors happened on websites and send you daily website performance reports.

  [See more](/manager/#website-monitoring-tool).

* **PHP Slow Site analyzer**

  PHP Slow Site analyzer (SSA) is a tool that generates daily reports for the server administrator 
  with information about the top N slow PHP-based URLs for all domains.

  The Slow Site analyzer tracks all PHP-based requests and selects slow ones by specific rules.

  [See more](/manager/#php-slow-site-analyzer).

* **X-Ray**

  X-Ray is a tool developed for website performance monitoring and performance issues detection.

  X-Ray can gather and visualize information about top slowest system functions, external requests, 
  software modules and database queries of the client’s PHP website.

  [See more](/manager/#x-ray).

### How can I try your distribution before paying for it?

We provide trial licenses for 30 days. You can get your trial key in two ways:

1. On the CloudLinux OS Solo main page, see [CloudLinux OS Solo](https://lp.cloudlinux.com/cloudlinux-os-solo).
2. In the CloudLinux Manager plugin, see the [Getting trial key](/manager/#activation) section.


The trial key must be entered during installation.

### CloudLinux OS Solo lifecycle

CloudLinux OS Solo has the same life cycle policy as RHEL. 
Using a supported operating system is critical to maintaining a stable server environment.

Currently, the following version are supported:

|Operating System | Release Date| End of Life and Support|
|-|--|-|
|CloudLinux OS Solo 8 | Jun 1, 2021 | May 31, 2029 |

## What's next

1. The [Installation](/installation) section contains information about how to install this OS.
2. The [landing page](https://lp.cloudlinux.com/cloudlinux-os-solo) contains the instructions about how to buy license or get trial.
