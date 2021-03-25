# Microsoft SQL Server in Mac

## Installing & Starting through Docker

```sh
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Maari@2021" \
   -p 1433:1433 --name sql1 -h sql1 \
   -d mcr.microsoft.com/mssql/server:2019-latest
```


## Check if Docker container running or To Remove Container

```sh
docker ps
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

## Run query inside SQL Server Docker Container

```sh
sudo docker exec -it sql1 "bash"
```

```sh
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Maari@2021"
```

**Reference :** [Docker Documentation of Microsoft SQL Server](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash)

## Connect from Code

```cs
// Student.cs
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace StudentManagement
{
    public class Student
    {
        public int StudentId { get; set; }
        public string StudentName { get; set; }
    }
}

// CollegeContext.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace StudentManagement
{
    public class CollegeContext : DbContext
    {
        public CollegeContext() : base("name=StudentConnectionString") { }
        public DbSet<Student> Students { get; set; }
    }
}

// Program.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace StudentManagement 
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Student stud = new Student();
            Console.WriteLine("Enter Student Id");
            stud.StudentId = int.Parse(Console.ReadLine());
            Console.WriteLine("Enter Student Name");
            stud.StudentName = Console.ReadLine();
            Program p = new Program();
            p.AddStudent(stud);
        }

        public void AddStudent(Student student)
        {
            CollegeContext c = new CollegeContext();
            c.Students.Add(student);
            //Add the student details to
            c.SaveChanges();

        }
    }
}
```
**app.config**

```xml
<configuration>
<connectionStrings>
    <add name="StudentConnectionString"
        connectionString="Data Source=localhost;Initial Catalog=TestDB;User Id=SA;Password=Maari@2021;"
        providerName="System.Data.SqlClient"
        />
  </connectionStrings>
</configuration>
```
