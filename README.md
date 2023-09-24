# Three-Tier-Architecture
Three Tier Architecture using Docker
#Steps:

1.Launch EC2 instance and install docker inside and start docker Service:
   
Use following Commands:
      a. sudo su - root  ,             
      b. yum install docker -y   ,
      c. systemctl enable  docker --now   ,
      d. systemctl status docker

   ![Screenshot 2023-08-30 181908](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/c919913b-aa92-448a-97f3-7a61165a0451)

2.create own net name as "psnet"
        subnet range 10.0.0.1/16 in CIDR format
        
  ![Screenshot 2023-08-30 185447](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/bc2e8ddd-5f80-42f6-90a4-0f44bc5607be)
      

#docker network ls
#docker network create --driver bridge --subnet 10.0.0.1/16  psnet
#docker network ls


3.Create database with own driver (database- container name)...also provide required enviromental variable

 ![Screenshot 2023-08-30 185342](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/2e1f0892-4059-4b02-b876-f91907d4bd7b)

#docker run -dit --name database --network psnet -v /mydata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=pratik55  -e MYSQL_DATABASE=mydatabase  -e MYSQL_USER=jack  -e MYSQL_PASSWORD=jack11 mysql

#docker inspect database   <--- here we show our subnet range(10.0.0.1/16) to our given database container

![Screenshot 2023-08-30 185033](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/0188840f-d021-4d93-b3dc-acb4f5951ddd)

![Screenshot 2023-08-30 185149](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/491672f9-612e-4480-9fef-48cfd2c61920)

Here,check provided subnet range.

4.Launch wordpress and uase PATTING to make outside world connection 

 As port number of container is 80 also check by #netstat -tnlp
 
 ![Screenshot 2023-08-30 185418](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/67b0ad62-3774-4f05-baf9-d3c8ad45305a)

  
  #docker run -dit --name mywordpress --network psnet -p 1234:80 wordpress

Can also check MYWORDPRESS has our subnet range by command on above screenshots.( step no.3)
#docker inspect mywordpress

5.EC2 intance has Firewall which cannot be connected by outside world, so we can modify inbound rules(All traffic allowed)

![Screenshot 2023-08-30 182129](https://github.com/pratikshashinde55/agricultural_bot/assets/61465971/f5cb4732-1715-4a23-89da-92f825f1ec81)

Substeps
     1. : EC2 Dashboard 
     2. select securtity option 
     3. go inside Security groups
     4. select "edit inbound rule"
     5. delete defult rule and new rule
     5. custom TCP ->> "ALL TRAFIC" ,ANYWHERE IPv4
     6. save rule


6.instance public + our port number that provide in wordpress contanier.

To access wordpresss from browser,need EC2 instance public IP Address+Port no. given to wordpresss container.

![Screenshot 2023-08-30 182811](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/10362dc7-d157-4d81-801e-109dada0e487)

To get wordpress interface on google ,provide details that provided in the form of enviromental variable during "database" caintainer launch

http://65.2.146.158:1234 <<------- see interface(This site not works as instance is terminated now)

![Screenshot 2023-08-30 182954](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/373ccfce-737e-4d25-adb9-67e718b13eed)

![Screenshot 2023-08-30 183151](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/274b58bf-662d-471e-b30d-0377d9ac259d)

7.Create username and password to create blog on wordpress:

![Screenshot 2023-08-30 183243](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/252d4b4d-25b8-42c7-a578-533c799266e7)

![Screenshot 2023-08-30 183447](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/7d416ddc-dc57-4ea2-93f0-fa2f4271e632)


8. On Wordpress Dashboard->create Post->Add Content->Publish->Copy link and paste in the browser.

   ![Screenshot 2023-08-30 183516](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/1fcf7a55-6131-4b29-93a0-e4af41897ecf)

![Screenshot 2023-08-30 184349](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/81302180-ad3f-4a8a-83c5-12f70395e70e)

![Screenshot 2023-08-30 184434](https://github.com/pratikshashinde55/Three-Tier-Architecture/assets/61465971/f112c827-0bdd-4f42-970c-1aac3e2555ac)


This is Three Tier Architecture using Docker----
1>WordPress-Blogging Site  ,
2>MySQL-Database ,
3>PSNet-Own bridge driver.

