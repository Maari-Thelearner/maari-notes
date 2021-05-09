# Deploy Asp.net core MVC project in cloud using heroku and docker

**Steps are given below**

**************

1. ***Create Asp.net core MVC Project*** \
	
*using visual studio 2019 or using command line*

```bash
		dotnet new mvc –name MVCWebApplication
```

2. ***Install Heroku CLI***

	Go to [HerokuCLI](https://devcenter.heroku.com/articles/heroku-cli#verifying-your-installation)

	after downloaded the heroku cli then type verify heroku is installed or not

	To check run this code

```bash
	heroku --version
```

3. ***Create new app in heroku***

	Go to [Heroku](https://www.heroku.com/)
	Complete your login credentials its completely free
	after that create a new app
	**Note: This app name will be change in to url name of your website future so give primary name of your project**


4. ***Install Docker Desktop***

	If you have already docker means skip to next step else click [docker](https://docs.docker.com/get-docker/) to download the docker desktop in your pc


5. ***Create Dockerfile inside your project***

	After install the docker desktop in your pc then create a Dockerfile inside your project

	if (your are using visual studio 2019 means)
	{
		open your project in visual studio --> right click your root directory --> click add --> click docker support
	}
	it will automatically create a docker file inside your project

	else {
		right click root directory --> add new file --> name it as "Dockerfile"
	}

6. ***Sample Dockerfile code***


```bash
	FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
	WORKDIR /app
	COPY . .

	CMD ASPNETCORE_URLS=http://*:$PORT dotnet AWSdeploy.dll
```

7. ***Add one line inside your Program.cs file***

	Inside your project --> click --> Program.cs

 existing code is 

```c#
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
```

Replace by this

```c#
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.UseUrls("http://*:" + Environment.GetEnvironmentVariable("PORT"));
                });
 ```

 8. ***Release your project in single contained mode***

 	Inside your project run this command
 	***Note: Inside your project means which have .sln file*** so change the directory to that project directory

 ```bash
 		dotnet publish {solutionfilePATH} -c Release
 ```
 after creating the publish folder then cut the Dockerfile which we created already and paste it in publish folder which we created now

 9. ***Create a Dockerimage for Deploy**

 	Run this command
```bash
	heroku login

	heroku container:login

	docker build -t {Appname} {PATH_OF_PUBLISH_FOLDER}
```
	***Note: Appname is Heroku created Appname***

10. ***Create container tag for image***
	
	Run this command

```bash
	docker tag {Appname} registry.heroku.com/{Appname}/web

	docker push registry.heroku.com/{Appname}/web

	heroku container:release web --app {Appname}
```

	Click Open app in your heroku console page to check it is deploy...


***Thats it Deploy Process is done...***
