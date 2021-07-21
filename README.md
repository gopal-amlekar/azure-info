## Azure Cloud services summary


Azure cloud has a wide range of services available to help you get your code and data on the cloud. I decided to pen down few thoughts about which service to use in different use case scenarios. Beware, this is not a formal or official guide,ymmv!

#### Deploy web / API application on cloud
Your web application in this case could be a user facing web UI having HTML/CSS for front-end or an API application serving only JSON like data without any HTML. Azure provides various options for this purpose.

##### Azure Virtual Machine
Though this is an overkill for simple web apps, it is worth to know. You get a full blown up computer in the cloud with an Operating system of your choice such as Linux or Windows Server. You choose the disk size, type, RAM and various other factors to create the VM of your choice. You can log in to this machine via Remote desktop or SSH. 

Once you get into the VM remotely, you can then run your web application on this machine similar to how you do on your local machine. However, it is not as straightforward as you do on local machine. You will need to have a web server such as IIS on Windows or Apache on Linux for example. Besides, you need to set up proper networking, firewall to make your web app available to the rest of the world. Moreover, you are responsible to keep your virtual machine updated with latest operating system updates, patches, take care of the disk space and so on. This choice is quite expensive as well compared to other options but you get full control. 

Think of this as if you are constructing your own house, you can control the architecture of your house, the material used for construction and so on. But then you are also responsible for the robustness, aesthetics, utility of the house you are building.

##### Azure App Services
Azure app service takes the concept of VM to a little further and makes the deployment of your code simpler. You deploy your code directly without worrying about choosing the disk space, RAM or the OS patches and so on. Azure then takes your code and runs it to make it available on the web. You don't need to worry about the web server, networking, firewall or operating system updates etc. You still get a choice to choose the operating system or environment to run your code but the choice is simpler here, you either choose Windows or Linux. The choice depends on what platform your code is capable to run. With Node JS for example, you can run it on both Linux and Windows. With the legacy ASP.Net Full framework, you can run only on Windows. (ASP.Net core and ASP.Net 5 onwards run on Linux). In general, Linux is the preferred choice as it costs lesser and is lightweight and faster than Windows. The web app runs 24 x 7 and therefore is always available to the users. It costs significantly lesser than a full blown VM.

When you deploy your app using App services, you get a public facing URL like yourapp.Azurewebsites.net. You can optionally add your own domain name with the required SSL certificates to make it available via your domain name. As you can see, you don't own the server here so you don't get full control of everything underlying but then your deployment job becomes easier.

Think of this as a readymade apartment wherein you can choose the interior and furniture but you don't have direct control over the architecture and construction material used. 

#### Run tasks in the cloud
When you do not need an always available app like a website or an API app, but you want to carry out some tasks as needed, the choice is Azure functions a.k.a. Serverless. (Though the word serverless has a wider scope than just short-lived tasks. 

The concept of Azure function is similar to app services that you deploy your code without worrying about underlying server hardware, OS or other stuff, but the functions offer few different features.

Azure function service is designed for short lived tasks such as sending an email, SMS, updating some database or doing some quick data processing. Your code normally stays dormant in the cloud but when needed, you can wake it up, run it and then again make it go to sleep!

How long you can keep that code awake at a time? Well, that depends on how much you are willing to pay for it. The duration varies from 10 minutes to 60 minutes depending on your billing plan selected for the function. It is interesting to see various options in the billing plans and some of them even overlap the app services with an 'always-on' setting. 

There are a number of ways in which you can 'wake up' the function code. Most common is an http trigger which means you just navigate to the function app URL in a web browser or any other web client such as Postman or even the command-line utility CURL. You can even use this http trigger as something called as 'Webhook' and trigger the function via Twilio on an incoming message. 

You can alternatively, set up a time trigger which means the function waks up and executes at pre-defined time of day/week/month or even periodically at fixed intervals such as every hour.

Some advanced triggers include events such as when a database is updated (by some other application for example) or when some other web app publishes a message on an Azure service bus. 

Though not an exact analogy, but I like to think of functions as a hotel room. You go in and stay there, use the hotel services, pay for it and leave. You don't worry about and neither you have any control over the interior, furniture or any such thing in the room. The hotel management takes care to setup and clean up required resources.

#### Store data in the cloud
Azure provides a number of ways to store your data in the cloud. You can store text or binary data in databases. You can also store images, files in blobs. 

##### Relational Databases
When you ant the data to be stored in multiple tables and perhaps establish relations between the tables, relational databases is the choice. This looks like an excel workbook with data stored in multiple spreadsheets and some formulas or links to link the spreadsheets to each other. When using this type of database, you need to know the data structure in advance. Data structure simply means the columns / fields to store the data, their type (text / number etc.). You also need to define the relations between the tables in advance at design time. 

Azure offers SQL databases, MySQL databases, PostGreSQL databases as service. These are fully managed databases which means that you need not worry about the underlying SQL server, disk or memory requirements. Azure manages it for you and offers you database as a service. You can directly work with the databases using SQL (Strucuted Query Language) from a SQL client such as SQL Server Management Studio.

##### Non-Relational Databases
Often known as NoSQL databases, you can store data in a non-tabular way. These databases offer a more flexible way to store data than the relational tabular data form. These types of databases can be used to store documents such as blog posts or they can also store key-value paired database. Due to the non-relational nature, these databases do not offer a direct way to define relations between data. Also, since there are no tables as such, you can't have data joined from different tables. With these databases, the performance is usually faster than SQL databases.

Azure Cosmos database is a quite popular NoSQL database. You can store binary data, text data, documents or even files / images inside a Azure Cosmos database.

##### File storage
When you want to store files such as images or PDF files Azure offers storage accounts. You can create containers inside the storage accounts and store your files there. These are stored as BLOBs (Binary Large Objects). It is almost similar to having a cloud drive (Google Drive for example) to store your documents and share it with rest of the world. You can also control access to the documents and define who can upload documents to your storage account etc.

#### Store secrets in the cloud
Why would you need that? Well, when you run your apps in the cloud, eventually you will want to access some databse or some other 3rd party API services. Doing so usually needs to have some credentials like username/password or API keys or some other mechanism. It is not a safe / secure practice to store your credentials in plain text in the code. The code is available to developers in your team and perhaps to the entire world if you put it in a public GitHub repo. To avoid revealing your secrets via code, Azure offers a service called as Azure Key Vault. With this service, you store the credentials / keys inside the vault and access it securely from your app. 

Again as an analogy, think of this like a locker in a bank to store your important documents or jewellery. You can configure the key vault to have access only by your code / app. All the keys/data you store in the vault is stored on Azure in encrypted way.

#### Provide User accounts in the app
Eventually, when your application grows, you will want to provide personalised services to your users. You can have the users setup their account on your web app and let them save their preferences or favourites or anything else that your web app offers. 

It is not safe to store the user's credentials in your database. If your database gets compromised and some malicious hacker gains access to it, they can get full access to user's personal information including their passwords. If the users have reused their password from some other sites (including even banking) then this can turn into a nightmare for the users as well as you as the app owner.

Azure offers a service called as Active Directory B2C. With this, you can have the user's credentials stored in Azure in a much more secure and safer way. Users can even login to your app using their existing social accounts such as Google, Facebook etc. This saves you a lot of trouble by not worrying much about user's credentials and personal data other than what you store in your app. The B2C in this service stands for Business to Consumer. 


#### Cognitive / AI Services
Azure offers a number of cognitive services such as face recognition, translation, text analysis, speech recognition, text to speech conversion and so on. These services are offered as APIs and you generally do not need to write lot of code to access these APIs.



As mentioned earlier, this is not a comprehensive guide. Which Azure service to use depends a lot on specific use case but this post can serve as a starting point. 
