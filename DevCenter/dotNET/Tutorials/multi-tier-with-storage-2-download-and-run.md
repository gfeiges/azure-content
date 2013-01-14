﻿<properties linkid="develop-net-tutorials-multi-tier-web-site-2-download-and-run" urlDisplayName="Step 2: Download and Run" pageTitle="Multi-tier web site tutorial - Step 2: Download and run" metaKeywords="Windows Azure tutorial, deploying email service app, publishing email service" metaDescription="The second tutorial in a series that teaches how to configure your computer for Windows Azure development and deploy the Email Service app." metaCanonical="" disqusComments="1" umbracoNaviHide="1" writer="riande" editor="mollybos" manager="wpickett" />

<div>
<div class="left-nav">
<div class="static-nav">
<ul>
<li class="menu-nodejs-compute"><a href="/en-us/develop/net/compute/">Compute</a></li>
<li class="menu-nodejs-data"><a href="/en-us/develop/net/data/">Data Services</a></li>
<li class="menu-nodejs-appservices"><a href="/en-us/develop/net/app-services/">App Services</a></li>
</ul>
<ul class="links">
<li class="forum"><a href="/en-us/support/forums/">Forums</a></li>
</ul>
<ul>
<li>IN THIS SERIES</li>
<li><a href="../1-overview/">1. Overview</a></li>
<li><strong>2. DOWNLOAD AND RUN </strong></li>
<li><a href="../3-web-role/">3. Web Role</a></li>
<li><a href="../4-worker-role-a/">4. Worker Role A</a></li>
<li><a href="../5-worker-role-b/">5. Worker Role B</a></li>
</ul>
</div>
<div class="floating-nav jump-to">
<ul>
<li>On the page (jump to):</li>
</ul>
</div>
</div>
</div>

# Configuring and Deploying the Windows Azure Email Service application - 2 of 5

This is the second tutorial in a series of five that show how to build and deploy the Windows Azure Email Service sample application.  For information about the application and the tutorial series, see the [first tutorial in the series][firsttutorial].

This tutorial shows how to configure your computer for Azure development and how to deploy the Windows Azure Email Service application to a Windows Azure Cloud Service by using  any of the following products:

* Visual Studio 2012
* Visual Studio 2012 Express for Web
* Visual Studio 2010
* Visual Web Developer Express 2010.

You can open a Windows Azure account for free, and if you don't already have Visual Studio 2012, the SDK automatically installs Visual Studio 2012 for Web Express. So you can start developing for Windows Azure entirely for free.

In this tutorial you'll learn:

* How to set up your computer for Windows Azure development by installing the Windows Azure SDK.
* How to configure and test the Windows Azure Email Service application on your local machine.
* How to publish the application to Windows Azure.
* How to view and edit Windows Azure tables, queues, and blobs by using Visual Studio or Azure Storage Explorer.
* How to configure tracing and view trace data.
* How to scale the application by increasing the number of worker role instances.

<div class="dev-callout"><strong>Note</strong>
<p>To complete this tutorial, you need a Windows Azure account that has the Windows Azure Web Sites feature enabled. You can create a free trial account and enable preview features in just a couple of minutes. For details, see <a href="/en-us/develop/net/tutorials/create-a-windows-azure-account/" target="_blank">Create a Windows Azure account and enable preview features</a>.</p>
</div>
<br />

### Tutorial segments

- [Set up the development environment][]
- [Set up a free Windows Azure account][]
- [Create a Windows Azure Storage account][]
- [Install Azure Storage Explorer][]
- [Create a Cloud Service][]
- [Download and run the completed solution][]
- [View developer storage in Visual Studio][]
- [Configure the application for Windows Azure Storage][]
- [Deploy the application to Windows Azure][]
- [Promote the application from staging to production][]
- [Configure the application to use SendGrid][]
- [Configure and view trace data][]
- [Add another worker role instance to handle increased load][]





<h2><a name="setupdevenv"></a><span class="short-header">Set up environment</span>Set up the development environment</h2>

To start, set up your development environment by installing the Windows Azure SDK for the .NET Framework. 

1. To install the Windows Azure SDK for .NET, click the link that corresponds to the version of Visual Studio you are using. If you don't have Visual Studio installed yet, use the Visual Studio 2012 link.<br/>
[Windows Azure SDK for Visual Studio 2010][]<br/>
[Windows Azure SDK for Visual Studio 2012][]<br/>
If you don't have Visual Studio installed yet, it will be installed by the link.

   **Warning:** Depending on how many of the SDK dependencies you already have on your machine, installing the SDK could take a long time, from several minutes to a half hour or more.

2. When you are prompted to run or save vwdorvs11azurepack.exe, click **Run**.

3. In the Web Platform Installer window, click **Install** and proceed with the installation.

   ![Web Platform Installer - Windows Azure SDK for .NET][WebPIAzureSDKNETVS12Oct2012]<br/>

When the installation is complete, you have everything necessary to start developing.





<h2><a name="setupwindowsazure"></a><span class="short-header">Create Windows Azure account</span>Set up a free Windows Azure account</h2>

The next step is to create a Windows Azure account.

1. Browse to [Windows Azure](http://www.windowsazure.com "Windows Azure").

2. Click  the **Free trial** link and follow the instructions. 




<h2><a name="createWASA"></a><span class="short-header">Create Storage account</span>Create a Windows Azure Storage account</h2>

When you run the sample application in Visual Studio, you can access tables, queues, and blobs in Windows Azure development storage or in a Windows Azure Storage account in the cloud. Development storage uses a SQL Server Express LocalDB database to emulate the way Windows Azure Storage works in the cloud.  In this tutorial you'll start by using development storage, and then you'll learn how to configure the application to use a cloud storage account when it runs in Visual Studio. In this section of the tutorial you create the Windows Azure Storage account that you'll configure Visual Studio to use later in the tutorial.    

1. In your browser, open the [Windows Azure Management Portal][NewPortal].

2. In the [Windows Azure Management Portal][NewPortal], click **Storage**, then click **New**.

   ![New Storage][mtas-portal-new-storage]

3. Click **Quick Create**.
   
   ![Quick Create][mtas-storage-quick]

4. In the URL input box, enter a URL prefix. 

   This prefix plus the text you see under the box will be the unique URL to your storage account. If the prefix you enter has already been used by someone else, you'll see "The storage name is already in use" above the text box and you'll have to choose a different prefix.

5. Set the region to the area where you want to deploy the application.

6. Uncheck the **Enable Geo-Replication** check box. 

   When geo-replication is enabled for a storage account, the stored content is replicated to a secondary location to enable failover to that location in case of a major disaster in the primary location. Geo-replication can incur additional costs. You'll see a warning when you disable geo-replication because you pay a data transfer charge if you start with it disabled and then decide to enable it later. You don’t want to disable replication, upload a huge amount of data, and then enable replication. For test and development accounts, you generally don't want to pay for geo-replication. For more information, see [How To Manage Storage Accounts][managestorage].

5. Click **Create Storage Account**. 

   In the image below, a storage account is created with the URL `aestest.core.windows.net`.

   ![create storage with URL prefix][mtas-create-storage-url-test]

   This step can take several minutes to complete. While you are waiting, you can repeat these steps and create a production storage account. It's often convenient to have a test storage account to use for local development, another test storage account for testing in Windows Azure, and a production storage account.

5. Click the test account that you created in the previous step, then click the **Manage Keys** icon.

   ![Manage Keys][mtas-manage-keys]<br/>

   ![Keys GUID][mtas-guid-keys]<br/>

   You'll need the **Primary Access Key** or **Secondary Access Key** access key throughout this tutorial. You can use either one of these keys in a storage connection string. 

   There are two keys so that you can periodically change the key that you use without causing an interruption in service to a live application. You regenerate the key that you're not using, then you can change the connection string in your application to use the regenerated key. If there were only one key, the application would lose connectivity to the storage account when you regenerated the key. The keys that are shown in the image are no longer valid because they were regenerated after the image was captured.

6. Copy one of these keys into your clipboard for use in the next section.





<h2><a name="installASE"></a><span class="short-header">Install ASE</span>Install Azure Storage Explorer</h2>

**Azure Storage Explorer** is a tool that you can use to query and update Windows Azure storage tables, queues, and blobs. You will use it throughout these tutorials to verify that data is updated correctly and to create test data.

1. Install  [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/ ).

2. Launch **Azure Storage Explorer** and click **Add Account**.

   ![Add ASE Account][mtas-ase-add]

3. Enter the name of the test storage account and paste the key that you copied previously.

4. Click **Add Storage Account**.

   ![Add ASE Account][mtas-ase-add2]




<h2><a name="createcloudsvc"></a><span class="short-header">Create Cloud Service</span>Create a Cloud Service</h2>

1. In your browser, open the [Windows Azure Management Portal][NewPortal].

2. Click **Cloud Services** then click the **New** icon.

   ![Quick Cloud][mtas-new-cloud]<br/>

3. Click **Quick Create**.

4. In the URL input box, enter a URL prefix. 

   Like the storage URL, this URL has to be unique, and you will get an error message if the prefix you choose is already in use by someone else.

5. Set the region to the area where you want to deploy the application.

   You should create the cloud service in the same region that you created the storage account. When the cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside the data center. Bandwidth within a data center is free.

	Azure affinity groups provide a mechanism to minimize the distance between resources in a data center, which can reduce latency. This tutorial does not use affinity groups. For more information, see [How to Create an Affinity Group in Windows Azure](http://msdn.microsoft.com/en-us/library/windowsazure/hh531560.aspx).

6. Click **Create Cloud Service**. 

   In the following image, a cloud service is created with the URL aescloud.cloudapp.net.

   ![create storage with URL prefix][mtas-create-cloud]

   You can move on to the next step without waiting for this step to complete.





<h2><a name="downloadcnfg"></a><span class="short-header">Download and run</span>Download and run the completed solution</h2>

1. Download and unzip the [completed solution](http://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

2. Start Visual Studio with elevated permissions.

   The compute emulator that enables Visual Studio to run a Windows Azure project locally requires elevated permissions.

3. To keep the download size small, the completed solution is provided without the assemblies or other content for the installed NuGet packages. When you open and build the solution, NuGet automatically gets all of the package content. In order for this to work, you have to enable the NuGet package restore option in Visual Studio. If you haven't already enabled NuGet package restore, do the following steps. 

      1. From the **Tools** menu, click **Library Package Manager**, and then click **Manage NuGet Packages for Solution**. 

      2. In the lower left corner of the **Manage NuGet Packages** dialog, click **Settings**.

      3. In the left pane of the **Options** dialog box, select **General** under **Package Manager**.

      4. Select **Allow NuGet to download missing packages during build**.

      ![Enabling NuGet package restore](../Media/NuGetPkgRestore.png)

3. From the **File** menu choose **Open Project**, navigate to where you downloaded the solution, and then open the solution file.

3. In **Solution Explorer**, make sure that **AzureEmailService** is selected as the startup project.

1. Press CTRL+F5 to run the application.

   The application home page appears in your browser.

   ![Run the App.][mtas-mailinglist1]

2. Click  **Create New**.

2. Enter some test data, and then click **Create**.

   ![Run the App.][mtas-create1]

3. Create a couple more mailing list entries.

   ![Mailing List Index Page][mtas-mailing-list-index-page]

4. Click **Subscribers**, and then add some subscribers. Set **Verified** to `true`.

   ![Subscriber Index Page][mtas-subscribers-index-page]

4. Prepare to add messages by creating a *.txt* file that contains the body of an email that you want to send. Then create an *.htm* file that contains the same text but with some HTML (for example, make one of the words in the message bold or italicized). You'll use these files in the next step.

4. Click **Messages**, and then add some messages. Select the files that you created in the previous step. Don't change the scheduled date which defaults to one week in the future. The application can't send messages until you configure SendGrid.

   ![Message Create Page][mtas-message-create-page]
	<br/><br/>
   ![Message Index Page][mtas-message-index-page]

The data that you have been entering and viewing is being stored in Windows Azure development storage. Development storage uses a SQL Server Express LocalDB database to emulate the way Windows Azure Storage works in the cloud.  The application is using development storage because that is what the project was configured to use when you downloaded it. This setting is stored in *.cscfg* files in the **AzureEmailService** project.  The *ServiceConfiguration.Local.cscfg* file determines what is used when you run the application locally in Visual Studio, and the *ServiceConfiguration.Cloud.cscfg* file determines what is used when you deploy the application to the cloud. Later you'll see how to configure the application to use the Windows Azure Storage account that you created earlier.





<h2><a name="StorageExpVS"></a><span class="short-header">Developer storage</span>Viewing developer storage in Visual Studio</h2>

The **Windows Azure Storage** browser in **Server Explorer** (**Database Explorer** in Express editions of Visual Studio) provides a convenient read-only view of Windows Azure Storage resources.

1. From the **View** menu in Visual Studio, choose **Server Explorer** (or **Database Explorer**).

2. Expand the **(Development)** node underneath the **Windows Azure Storage** node.

3. Expand **Tables** to see the tables that you created in the previous steps.

   ![Server Explorer][mtas-serverExplorer]

3. Double click the **MailingList** table.

   ![VS storage explorer][mtas-wasVSdata]

   Notice how the window shows the different schemas in the table. `MailingList` entities have `Description` and `FromEmailAddress` property, and `Subscriber` entities have the `Verified` property (plus `SubscriberGUID` which isn't shown because the image isn't wide enough). The table has columns for all of the properties, and if a given table row is for an entity that doesn't have a given property, that cell is blank.

You can't use the storage browser in Visual Studio to update or delete Windows Azure Storage resources. You can use [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/ ) to update or delete development storage resources. (To configure Azure Storage Explorer to use development storage, click the **Developer Storage** check box in the **Add Storage Account** dialog box.)



<h2><a name="conf4azureStorage"></a><span class="short-header">Use your storage account</span>Configure the application to use your Windows Azure Storage account</h2>

Next, you'll see how to configure the application so that it uses your Windows Azure Storage account when it runs in Visual Studio, instead of development storage. There is a newer way to do this in Visual Studio that was introduced in version 1.8 of the SDK, and an older way that involves copying and pasting settings from the Windows Azure management portal. The following steps show the newer way to configure storage account settings.

4. In **Solution Explorer**, right-click **MvcWebRole** under **Roles** in the **AzureEmailService** project, and click **Properties**.

   ![Right Click Properties][mtas-rt-prop]<br/>

5. Click the **Settings** tab. In the **Service Configuration** drop down box, select **Local**.

6. Select the **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at the right end of the line. Click the ellipsis button to open the **Storage Account Connection String** dialog box.

   ![Right Click Properties][mtas-elip]<br/>

7. In the **Create Storage Connection String** dialog, click **Your subscription**, and then click **Download Publish Settings**. 

   ![Right Click Properties][mtas-enter]<br/>

   Visual Studio launches a new instance of your default browser with the URL for the Windows Azure portal download publish settings page. If you are not logged into the portal, you will be prompted to log in. Once you are logged in, your browser will prompt you to save the publish settings. Make a note of where you save the settings.

   ![publish settings][mtas-3]

1. In the **Create Storage Connection String** dialog, click  **Import**, and then navigate to the publish settings file that you saved in the previous step.

1. Select the subscription and storage account that you wish to use, and then click **OK**.

  ![select storage account][mtas-5]

1. Follow the same procedure that you used for the `StorageConnectionString` connection string to set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.

   You don't have to download the publish settings file again. When you click the ellipsis for the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string, you'll find that the **Create Storage Connection String** dialog box remembers your subscription information. When you click the **Your subscription** radio button, all you have to do is select the same **Subscription** and **Account Name** that you selected earlier, and then click **OK**. 

2. Follow the same procedure that you used for the two connection strings for the MvcWebRole role to set the connection strings for the WorkerRoleA role and the workerRoleB role.

### The manual method for configuring storage account credentials

The following procedure shows what the manual way to configure storage account settings. If you used the automatic method that was shown in the previous procedure, you can skip this procedure, or you can read through it to see what the automatic method did for you behind the scenes.

1. In your browser, open the [Windows Azure Management Portal][NewPortal].

2. Click the **Storage** Tab, then click the test account that you created in the previous step, and then click the **Manage Keys** icon.

   ![Manage Keys][mtas-manage-keys]<br/>

   ![Keys GUID][mtas-guid-keys]<br/>

3. Copy the primary or secondary access key.


4. In **Solution Explorer**, right-click **MvcWebRole** under **Roles** in the **AzureEmailService** project, and click **Properties**.

   ![Right Click Properties][mtas-rt-prop]<br/>

5. Click the **Settings** tab. In the **Service Configuration** drop down box, select **Local**.

6. Select the **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at the right end of the line. Click the ellipsis button to open the **Storage Account Connection String** dialog box.

   ![Right Click Properties][mtas-elip]<br/>

7. In the **Create Storage Connection String** dialog, select the **Manually entered credentials** radio button. Enter the name of your storage account and the primary or secondary access key you copied from the portal. 

7. Click **OK**.

You can use the same procedure to configure settings for the worker roles, or you can propagate the web role settings to the worker roles by editing the configuration file. The following steps explain how to edit the configuration file. (This is still part of the manual method for setting storage credentials, which you don't have to do if you already propagated the settings to the worker roles by using the automatic method.)

8. Open the **ServiceConfiguration.Local.cscfg** file that is located in the **AzureEmailService** project.

   In the `Role` element for `MvcWebRole` you'll see a `ConfigurationSettings` element that has the settings that you updated by using the Visual Studio UI.

		  <Role name="MvcWebRole">
		    <Instances count="1" />
		    <ConfigurationSettings>
		      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[Key]" />
		      <Setting name="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=aestest;AccountKey=[Key]" />
		    </ConfigurationSettings>
		  </Role>
		
   In the `Role` elements for the two worker roles you'll see the same two connection strings.

9. Delete the `Setting` elements for these two connection strings from the `WorkerRoleA` and `WorkerRoleB` elements, and then copy and paste in their place the `Setting` elements from the `MvcWebRole` element.

For more information on the configuration files, see [Configuring a Windows Azure Project ](http://msdn.microsoft.com/en-us/library/windowsazure/ee405486.aspx)

### Test the application configured to use your storage account

9. Press CTRL+F5 to run the application. Enter some data by clicking the **Mailing Lists**, **Subscribers**, and **Messages** links as you did previously in this tutorial.

You can now use either **Azure Storage Explorer** or **Server Explorer** to view the data that the application entered in the Windows Azure tables.

### Use Azure Storage Explorer to view data entered into your storage account

10. Open **Azure Storage Explorer**.

11. Select the storage account that you entered credentials for earlier.

12. Under **Storage Type**, select **Tables**.

12. Select the `MailingList` table, and then click **Query** to see the data that you entered on the **Mailing List** and **Subscriber** pages of the application.

   ![ASE][mtas-ase1]<br/>

### Use Server Explorer to view data entered into your storage account

11. In **Server Explorer** (or **Database Explorer**), right-click **Windows Azure Storage** and click **Add New Storage Account**.

12. Follow the same procedure you used earlier to set up your storage account credentials.

13. Expand the new node under **Windows Azure Storage** to view data stored in your Windows Azure storage account.

   ![ASE][mtas-se3]<br/>

### Optional steps to disable Azure Storage Emulator automatic startup 

If you are not using the storage emulator, you can decrease project start-up time and use less local resources by disabling automatic startup for the Windows Azure storage emulator.

11. In **Solution Explorer**, right click the **AzureEmailService** cloud project and select **Properties**.

   ![Selecting cloud project properties][mtas-aesp]<br/>

11. Select the **Development** tab.

12. Set **Start Windows Azure storage emulator** to **False**.

   ![Disabling the storage emulator automatic startup][mtas-1]<br/>

   **Note**: You should only set this to false if you are not using the storage emulator. 

   This window also provides a way to change the **Service Configuration** file that is used when you run the application locally from **Local** to **Cloud** (from *ServiceConfiguration.Local.cscfg* to *ServiceConfiguration.Cloud.cscfg*).

13. In the Windows system tray, right click on the compute emulator icon and click **Shutdown Storage Emulator**.

   ![ASE][mtas-se4]<br/>





<h2><a name="sendGrid"></a><span class="short-header">SendGrid</span>Configure the application to use SendGrid</h2>

The sample application uses SendGrid to send emails.  In order to send emails by using SendGrid, you have to set up a SendGrid account, and then you have to update a configuration file with your SendGrid credentials.

<div class="note"><p><strong>Note:</strong> If you don't want to use SendGrid, or can't use SendGrid, you can easily substitute your own email service. The code that uses SendGrid is isolated in two methods in worker role B.  [Tutorial 5][tut5] explains what you have to change in order to implement a different method of sending emails. If you want to do that, you can skip this procedure and continue with this tutorial; everything else in the application will work (web pages, email scheduling, etc.) except  for the actual sending of emails.</p></div>

### Create a SendGrid account
 
1. Follow the instructions in [How to Send Email Using SendGrid with Windows Azure](http://www.windowsazure.com/en-us/develop/net/how-to-guides/sendgrid-email-service/ "SendGrid") to sign up for a free account.

### Update SendGrid credentials in worker role properties
 
Earlier in the tutorial when you set the storage account credentials for the web role and the two worker roles, you may have noticed that worker role B had three settings that were not in the web role or worker role A. You can use that same UI now to configure those three settings (select **Cloud** in the **Service Configuration** drop-down list).

The following steps show an alternative method for setting the properties, by editing the configuration file.

2. Edit the *ServiceConfiguration.Cloud.cscfg* file in the `AzureEmailService` project and enter the SendGrid user name and password values that you obtained in the previous step into the `WorkerRoleB` element that has these settings. The following code shows the WorkerRoleB element.

   ![SendGridSettings][mtas-sg]<br/>

3. There is also an AzureMailServiceURL setting. Set this value to the URL that you selected when you created your Windows Azure Cloud Service, for example:  "http://aescloud.cloudapp.net".

By updating the cloud configuration file, you are configuring settings that will be used when the application runs in the cloud. If you wanted the application to send emails while it runs locally, you would also have to update the *ServiceConfiguration.Local.cscfg* file.




<h2><a name="deployAz"></a><span class="short-header">Deploy to Windows Azure</span>Deploy the Application to Windows Azure</h2>

To deploy the application you can create a package in Visual Studio and upload it by using the Windows Azure Management Portal, or you can publish directly from Visual Studio. In this tutorial you'll use the publish method.

You'll publish the application to the staging environment first, and  later you'll promote the staging deployment to production.

### Implement IP restrictions

When you deploy to staging, the application will be publicly accessible to anyone who knows the URL. Therefore, your first step is to implement IP restrictions to ensure that no unauthorized persons can use it. In a production application you would implement an authentication and authorization mechanism like the ASP.NET membership system, but these functions have been omitted from the sample application to keep it simple to set up, deploy, and test.

1. Open the *Web.Release.config* file that is located in the root folder of the `MvcWebRole` project, and replace the **ipAddress** attribute value 127.0.0.1 with your IP address. (To see the **Web.Release.config** file in **Solution Explorer** you have to expand the *Web.config* file.) 

   You can find your IP address by searching for "Find my IP" with  [Bing](http://www.bing.com/search?q=find+my+IP&qs=n&form=QBLH&pq=find+my+ip&sc=8-10&sp=-1&sk= "find my IP") or another search engine. 

   When the application is published, the transformations specified in the *Web.release.config* file are applied, and the IP restriction elements are updated in the *web.config* file that is deployed to the cloud. You can view the transformed *web.config* file in the *AzureEmailService\MvcWebRole\obj\Release\TransformWebConfig\transformed* folder after the package is created.

### Configure the application to use your storage account when it runs in the cloud

Earlier in the tutorial when you set the storage account credentials for the web role and the two worker roles, you set the credentials to use when you run the application locally. Now you need to set the storage account credentials to use when you run the application in the cloud.

For this test run you'll use the same credentials for the cloud that you have been using for running locally. If you were deploying a production application, you would typically use a different account for production than you use for testing. Also a best practice for production would be to use a different account for the diagnostics connectionString than the storage connection string, but for this test run you'll use the same account.

You can use the same UI to configure the connection strings (just make sure that you select **Cloud** in the **Service Configuration** drop-down list). As an alternative, you can edit the configuration file, as explained in the following steps.

1. Open the *ServiceConfiguration.Local.cscfg* file in the **AzureEmailService** project, and copy the `Setting` elements for `StorageConnectionString` and `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`.  

2. Open the *ServiceConfiguration.Cloud.cscfg* file in the **AzureEmailService** project, and paste the copied elements into the `Configuration Settings` element for `MvcWebRole`, `WorkerRoleA`, and `WorkerRoleB`, replacing the `Setting` elements that are already there.

2. Verify that the web role and two worker role elements all define the same connection strings.

### Publish the application

2. If it is not already open, launch Visual Studio as administrator and open the **AzureEmailService** solution.

3.	Right-click the **AzureEmailService** cloud project and select **Publish**.

   ![Package][mtas-6]

   The **Publish Windows Azure Application** dialog appears. 

   ![Cloud Package][mtas-16]

4. If you used the automatic method for importing storage account credentials earlier, your Windows Azure subscription is in the drop-down list and you can select it and then click **Next**. Otherwise, click **Sign in to download credentials** and follow the instructions in [Configure the application for Windows Azure Storage][] to download and import your publish settings.

1. In the **Common Settings** tab, verify the setting in the **Cloud Service** drop-down list.

2. In the **Environment** drop-down list, change **Production** to **Staging**.

  ![Dashboard][mtas-7]

3. Keep the default **Release** setting for **Build configuration** and **Cloud** for **Service configuration**.

	The default settings in the **Advanced** tab are fine for this tutorial. On the **Advanced** tab are a couple of settings that are useful for development and testing. For more information on the advanced tab, see [Publish Windows Azure Application Wizard](http://msdn.microsoft.com/library/windowsazure/hh535756.aspx "pub wiz").
   
1. Click **Next**.

1. In the **Summary** step of the wizard, click the **save** icon (the diskette icon shown to the right of the Target profile drop-down list) to save the publish settings. 

   The next time you publish the application, the saved settings will be used and you won't need to go through the publish wizard again. 

1. Review the settings, then click **Publish**.

   ![pub][mtas-8]

   The **Windows Azure Activity Log** window is opened in Visual Studio. 

2. Click the right arrow icon to expand the deployment details.

   ![pub][mtas-11]
	<br/><br/>
   ![pub][mtas-9]

	The deployment can take about 5 minutes or more to complete.

1. When the deployment status is complete, click the **WebSite URL** to launch the application.

   ![Dashboard][mtas-c55]

9. Enter some data in the **Mailing List**, **Subscriber**, and **Message** web pages to test the application.

	**Note**: Delete the application after you have finished testing it to avoid paying for resources that you aren't using. If you are using a [Windows Azure 90 day free trial account](http://www.windowsazure.com/en-us/pricing/free-trial/ "free-trial account"), the three deployed roles will use up your monthly limit in a couple of weeks. 	To delete a deployment by using the Windows Azure management portal, select the cloud service and click **DELETE** at the bottom of the page, and then select the production or staging deployment.
<br/>
<br/>
 ![pub][mtas-19]

1. In the Windows Azure Activity Log in Visual studio, select **Open in Server Explorer**.

   Under **Windows Azure Compute** in **Server Explorer** you can monitor the deployment. If you selected **Enable Remote Desktop for all roles** in the **Publish Windows Azure Application** wizard, you can right click on a role instance and select **Connect using Remote Desktop**. 

   ![pub][mtas-12]


<h2><a name="swap"></a><span class="short-header">Production</span>Promote the Application from Staging to Production</h2>

1. In the [Windows Azure Management Portal][NewPortal], click the **Cloud Services** icon in the left pane, and then select your cloud service.

2. Click  **Swap**.

2. Click **Yes** to complete the VIP (virtual IP) swap. This step can take several minutes to complete.

   ![Dashboard][mtas-c6]

3. Click the **Cloud Services** icon in the left pane, and then select  your cloud service.

4. Scroll down the **Dashboard** tab for the **Production** deployment to the **quick glance** section on the lower right part of the page. Notice that the **Site URL** has changed from a GUID prefix to the name of your cloud service. 

   ![Dashboard][mtas-c7]

5. Click link under **Site URL** or copy and paste it to a browser to test the application in production. 

   If you haven't changed the storage account settings, the data you entered while testing the staged version of the application is shown when you run the application in the cloud.




<h2><a name="trace"></a><span class="short-header">Tracing</span>Configure and View Tracing Data</h2>

Tracing is an invaluable tool for debugging a cloud application. In this section of the tutorial you'll see how to view tracing data.

1. Verify that the diagnostics connection string is configured to use your Windows Azure Storage account and not development storage. 

   If you followed the instructions earlier in the tutorial, they will be the same. You can verify that they are the same either using the Visual Studio UI (the **Settings** tab in **Properties** for the roles), or by looking at the *ServiceConfiguration.\*.cscfg* files.
 
   **Note:** A best practice is to use a different storage account for tracing data than the storage account used for production data, but for simplicity in this tutorial you have been configuring the same account for tracing.

1. In Visual Studio, open *WorkerRoleA.cs* in the **WorkerRoleA** project, search for `ConfigureDiagnostics`, and examine the `ConfigureDiagnostics` method. 

        private void ConfigureDiagnostics()
        {
            DiagnosticMonitorConfiguration config = DiagnosticMonitor.GetDefaultInitialConfiguration();
            config.ConfigurationChangePollInterval = TimeSpan.FromMinutes(1d);
            config.Logs.BufferQuotaInMB = 500;
            config.Logs.ScheduledTransferLogLevelFilter = LogLevel.Verbose;
            config.Logs.ScheduledTransferPeriod = TimeSpan.FromMinutes(1d);

            DiagnosticMonitor.Start(
                "Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString",
                config);
        }

   In this code, the `DiagnosticMonitor` is configured to store up to 500 MB of trace information (after 500 MB, the oldest data is overwritten) and to store all trace messages (LogLevel.Verbose). The `ScheduledTransferPeriod` transfers the trace data to storage every minute. You must set the `ScheduledTransferPeriod` to save trace data. 

   The `ConfigureDiagnostics` method in each of the worker and web roles configures the trace listener to record data when you call the Trace API. For more information, see [Using Trace in Windows Azure Cloud Applications](http://blogs.msdn.com/b/windowsazure/archive/2012/10/24/using-trace-in-windows-azure-cloud-applications-1.aspx "Using Trace in Windows Azure")

1. In **Server Explorer**, select **WADLogsTable** for the storage account that you added previously. You can enter a [WCF Data Services filter](http://msdn.microsoft.com/en-us/library/windowsazure/ff683669.aspx "WCF filter") to limit the entities displayed. In the following image, only warning and error messages are displayed.

  ![Dashboard][mtas-trc]





<h2><a name="addRole"></a><span class="short-header">Add a Role Instance</span>Add another worker role instance to handle increased load</h2>

There are two approaches to scaling compute resources in Azure roles, by specifying the [virtual machine size](http://msdn.microsoft.com/en-us/library/windowsazure/ee814754.aspx "VM sizes") and/or by specifying the instance count of running virtual machines. 

The virtual machine (VM) size is specified in the `vmsize` attribute of the `WebRole` or `WorkerRole` element in the *ServiceDefinition.csdef* file. The default setting is `Small` which provides you with one core and 1.75 GB of RAM. For applications that are multi-threaded and use lots of memory, disk, and bandwidth, you can increase the VM size for increased performance. For example, an `ExtraLarge` VM has 8 CPU cores and 14 GB of RAM. Increasing memory, cpu cores, disk, and bandwidth on a single machine is known as *scale up*. Good candidates for scale up include ASP.NET web applications that use [asynchronous methods](http://www.asp.net/mvc/tutorials/mvc-4/using-asynchronous-methods-in-aspnet-mvc-4 "Async MVC"). See [Virtual Machine Sizes](http://msdn.microsoft.com/en-us/library/windowsazure/ee814754.aspx "VM sizes") for a description of the resources provided by each VM size.

Worker role B in this application is the limiting component under high load because it does the work of sending emails. (Worker role A just creates queue messages, which is not resource-intensive.) Because worker role B is not multi-threaded and does not have a large memory footprint, it's not a good candidate for scale up. Worker role B can scale linearly (that is, nearly double performance when you double the instances) by increasing the instance count. Increasing the number of compute instances is known as *scale out*. There is a cost for each instance, so you should only scale out when your application requires it. 

You can scale out a web or worker role by updating the setting in the Visual Studio UI or by editing the *ServiceConfiguration.\*.cscfg* files directly. The instance count is specified in the **Configuration** tab of the role **Properties** window and in the `Instances` element in the *.cscfg* files. When you update the setting, you have to deploy the updated configuration file to make the change take effect. Alternatively, for transient increases in load, you can change the number of role instances in the Windows Azure Management Portal. You can also configure the number of instances using the Windows Azure Management API. Finally, you can use the [Autoscaling Application Block][autoscalingappblock] to automatically scale out to meet increased load. For more information on autoscaling, see the links at the end of [the last tutorial in this series][tut5].

In this section of the tutorial you'll scale out worker role B by using the management portal, but first you'll see how it's done in Visual Studio.

To do it in Visual Studio, you would right-click the role under **Roles** in the cloud project and select **Properties**.

   ![Right Click Properties][mtas-rt-prop]
 
You would then select the **Configuration** tab on the left, and select **Cloud** in the **Service Configuration** drop down. 

  ![Instance Count][mtas-instanceCnt]

Notice that you can also configure the VM size in this tab.

The following steps explain how to scale out by using the Windows Azure Management Portal.

1. In the Windows Azure Management Portal, select your cloud service, then click **Scale**.

1. Increase the number of instances for worker role B, and then click **Save**.

   ![increase instances][mtas-in3]

   It can take a few minutes for the new VMs to be provisioned.

1. Select the **Instances** tab to see your each role instance in your application.

 ![view instances][mtas-in2]




<h2><a name="nextsteps"></a><span class="short-header">Next steps</span>Next steps</h2>

You have now seen how to configure, deploy, and scale the completed application. The following tutorials show how to build the application from scratch. In the [next tutorial][tut3] you'll build the web role.

For links to additional resources for working with Windows Azure Storage tables, queues, and blobs, see the end of [the last tutorial in this series][tut5].

<div><a href="../3-web-role/" class="site-arrowboxcta download-cta">Tutorial 3</a></div>

[Set up the development environment]: #setupdevenv
[Set up a free Windows Azure account]: #setupwindowsazure
[Create a Windows Azure Storage account]: #createWASA
[Install Azure Storage Explorer]: #installASE
[Create a Cloud Service]: #createcloudsvc
[Download and run the completed solution]: #downloadcnfg
[View developer storage in Visual Studio]: #StorageExpVS
[Configure the application for Windows Azure Storage]: #conf4azureStorage
[Deploy the application to Windows Azure]: #deployAz
[Promote the application from staging to production]: #swap
[Configure the application to use SendGrid]: #sendGrid
[Configure and view trace data]: #trace
[Add another worker role instance to handle increased load]: #addRole

[firsttutorial]: /en-us/develop/net/tutorials/multi-tier-web-site/1-overview/
[tut2]: /en-us/develop/net/tutorials/multi-tier-web-site/2-download-and-run/
[tut3]: /en-us/develop/net/tutorials/multi-tier-web-site/3-web-role/
[tut5]: /en-us/develop/net/tutorials/multi-tier-web-site/5-worker-role-b/
[Windows Azure SDK for Visual Studio 2010]: http://go.microsoft.com/fwlink/?LinkID=254269
[Windows Azure SDK for Visual Studio 2012]:  http://go.microsoft.com/fwlink/?LinkId=254364
[NewPortal]: http://manage.windowsazure.com
[managestorage]: /en-us/manage/services/storage/how-to-manage-a-storage-account/
[autoscalingappblock]: /en-us/develop/net/how-to-guides/autoscaling/

[WebPIAzureSDKNETVS12Oct2012]: ../Media/WebPIAzureSDKNETVS12Oct2012.png
[mtas-portal-new-storage]: ../Media/mtas-portal-new-storage.png
[mtas-storage-quick]: ../Media/mtas-storage-quick.png
[mtas-create-storage-url-test]: ../Media/mtas-create-storage-url-test.png
[mtas-manage-keys]: ../Media/mtas-manage-keys.png
[mtas-guid-keys]: ../Media/mtas-guid-keys.PNG
[mtas-new-cloud]: ../Media/mtas-new-cloud.png
[mtas-create-cloud]: ../Media/mtas-create-cloud.png
[mtas-ase-add]: ../Media/mtas-ase-add.png
[mtas-ase-add2]: ../Media/mtas-ase-add2.png
[mtas-ase-add3]: ../Media/mtas-ase-add3.png
[mtas-rt-prop]: ../Media/mtas-rt-prop.png
[mtas-mailinglist1]: ../Media/mtas-mailinglist1.png
[mtas-create1]: ../Media/mtas-create1.png
[mtas-mailing-list-index-page]: ../Media/mtas-mailing-list-index-page.png
[mtas-subscribers-index-page]: ../Media/mtas-subscribers-index-page.png
[mtas-message-create-page]: ../Media/mtas-message-create-page.png
[mtas-message-index-page]: ../Media/mtas-message-index-page.png
[mtas-serverExplorer]: ../Media/mtas-serverExplorer.png
[mtas-wasVSdata]: ../Media/mtas-wasVSdata.png
[mtas-elip]: ../Media/mtas-elip.png
[mtas-enter]: ../Media/mtas-enter.png
[mtas-ase1]: ../Media/mtas-ase1.png
[mtas-se1]: ../Media/mtas-se1.png
[mtas-se2]: ../Media/mtas-se2.png
[mtas-se3]: ../Media/mtas-se3.png
[mtas-aesp]: ../Media/mtas-aesp.png
[mtas-1]: ../Media/mtas-1.png
[mtas-se4]: ../Media/mtas-se4.png
[mtas-2]: ../Media/mtas-2.png
[mtas-pack]: ../Media/mtas-pack.png

[mtas-fe]: ../Media/mtas-fe.png
[mtas-c1]: ../Media/mtas-c1.png
[mtas-c2]: ../Media/mtas-c2.png
[mtas-c3]: ../Media/mtas-c3.png
[mtas-c4]: ../Media/mtas-c4.png
[mtas-c5]: ../Media/mtas-c5.png
[mtas-c6]: ../Media/mtas-c6.png
[mtas-c7]: ../Media/mtas-c7.png
[mtas-er1]: ../Media/mtas-er1.png
[mtas-sg]: ../Media/mtas-sg.png
[mtas-trc]: ../Media/mtas-trc.png
[mtas-instanceCnt]: ../Media/mtas-instanceCnt.png
[mtas-sleep2]: ../Media/mtas-sleep2.png
[mtas-in3]: ../Media/mtas-in3.png
[mtas-in2]: ../Media/mtas-in2.png
[mtas-3]: ../Media/mtas-3.png
[mtas-5]: ../Media/mtas-5.png
[mtas-6]: ../Media/mtas-6.png
[mtas-16]: ../Media/mtas-16.png
[mtas-7]: ../Media/mtas-7.png
[mtas-8]: ../Media/mtas-8.png
[mtas-9]: ../Media/mtas-9.png
[mtas-11]: ../Media/mtas-11.png
[mtas-12]: ../Media/mtas-12.png
[mtas-c55]: ../Media/mtas-c55.png
[mtas-np]: ../Media/mtas-np.png
[mtas-19]: ../Media/mtas-19.png

[tut4]: /en-us/develop/net/tutorials/multi-tier-web-site/4-worker-role-a/
[blob6]: ../Media/blob6.png
[blob7]: ../Media/blob7.png
[blob8]: ../Media/blob8.png
[blob9]: ../Media/blob9.png

[mtas-storage-quick-SM]: ../Media/mtas-storage-quick-SM.png
[0]: ../../Shared/media/antares-iaas-preview-01.png
[1]: ../../Shared/media/antares-iaas-preview-05.png
[2]: ../../Shared/media/antares-iaas-preview-06.png
[Image001]: ../Media/Dev-net-getting-started-001.png
