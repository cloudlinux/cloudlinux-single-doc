
# FAQ

[[toc]]

### What is the difference between CloudLinux Solo and CloudLinux OS Shared?

CloudLinux OS Solo is not designed for shared hosting usage and it does not include any [LVE-related](https://docs.cloudlinux.com/lve_manager/) features and CageFS. 

CloudLinux OS Solo provides a set of tools for website monitoring and performance tracking out-of-box. [Read more](/manager/).

### Can I convert CloudLinux Solo to CloudLinux Shared?

The conversion mechanism will be implemented in the next releases.

### Is the CloudLinux OS Solo paid/unpaid?

The final pricing will be available with the upcoming Beta release, but you already can [sign up](https://lp.cloudlinux.com/cloudlinux-os-solo) for free Beta testing and test it without any costs using the trial license which can be extended.

### Where can I get the latest ISO of CloudLinux OS Solo?

You can download the latest ISO and use it to install CloudLinux OS Solo on your server using [this link](https://repo.cloudlinux.com/cloudlinux/8.3_solo_beta-netinstall/iso/x86_64/).

### How can I get the CloudLinux OS Solo trial license?

Please refer to [this page](https://lp.cloudlinux.com/cloudlinux-os-solo) to find information on how to get the trial license and the activation key.

### How many servers can I use with the trial license?

The number of servers is unlimited.

### What control panel can be used with CloudLinux OS Solo?

For the current implementation, we recommend using cPanel Solo and DirectAdmin. Plesk is not supported yet.

### How can I get support for my CloudLinux OS Solo servers?

Please feel free to ask your questions [here](https://cloudlinux.zendesk.com/hc/en-us/).

### Can I convert to CloudLinux OS Solo from another OS?

You can convert CentOS/AlmaLinux OS to CloudLinux OS Solo. Please refer to [this instruction](/installation/#converting-existing-servers).

### What is the Website Monitoring tool intended for?

The Website Monitoring tool is intended for monitoring the domain main page generation time and errors occurring when requesting the main page (the main tab).

* The administrator can get the list of the slowest requests during 24 hours (the PHP Slow Site analyzer tab).
* The administrator can receive the daily reports with the list of the slow requests.
* The administrator receives instant emails if the domain main page is unavailable.

You can read more about the Website Monitoring tool [here](/manager/#website-monitoring-tool).

### What is the X-Ray tool intended for?

The X-ray tool is intended for deep analysis of slow request cause. The X-ray tool can provide information about software modules and plugins execution time, database queries, system functions and external requests execution time.

It can be used by the administrator and by the user.

You can read more about the X-Ray tool [here](/manager/#x-ray).

### Will the Website Monitoring Tool (WMT) work with the LiteSpeed and Nginx web servers?

Yes, they will.

### Why I don't receive emails from the Website Monitoring tool?

Please verify your cPanel firewall settings.

### Can I use other CloudLinux products (Imunify360, KernelCare) with CloudLinux OS Solo?

Yes, it is possible.

### Can I register my CloudLinux OS Shared 6/7/8 with CloudLinux OS Solo trial key?

No, you cannot.

### Can I convert CentOS 6,7 to CloudLinux OS Solo 8 using the cldeploy script?

No, you cannot. Only CentOS 8/AlmaLinux 8 can be converted.

### Does CloudLinux OS Solo work with LiteSpeed?

No, it does not. We are still implementing this feature, so it will be available in the next CloudLinux OS Solo releases.

### What is the process of upgrading if the customer adds more users to the server and will have to upgrade? (i.e., upgrading from CloudLinux OS Solo to CloudLinux OS Shared)

Create a new server by [converting an existing server](/installation/#converting-existing-servers) or [from ISO](/installation/#iso-file-source)) and migrate the settings of that one user.

:::tip
We are collecting feedback about CloudLinux OS Solo to CloudLinux OS Shared transitions and may consider implementing that automatically in the future.
:::

### Where can I use the WP Optimization Suite?

In the current beta release only on CloudLinux OS Solo and cPanel. In the next releases it will be available for CloudLinux OS Shared PRO.

### How will it help my customers?

In the current beta version, the WP Optimization Suite automatically configures the Object Cache module per site. In the next releases we will add modules to help automatically increase performance for WordPress sites.

### How to set up PHP-fpm handler for the domain?

Since the PHP-fpm handler is required to use WP Optimization Suite, you may need to configure it manually.

These are configuration steps:

1. Ensure the PHP-fpm package for the current PHP version is installed or install it.

    * **Manual installation**
    
    Run the following command:
    ```
    yum -y install ea-phpXY-php-fpm
    ```
    Where `XY` - is required PHP version, for instance `ea-php74-php-fpm`

    * **Via MultiPHP Manager**

    ![](/images/InstallationViaMultiPHPManager.png)


2. Enable PHP-fpm handler for domain via MultiPHP Manager

![](/images/EnablePHPFpm.png)

