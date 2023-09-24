# Three-Tier-Architecture
Three Tier Architecture using Docker
#Steps:

1.Launch EC2 instance and install docker inside and start docker Service:
   
Use following Commands:
      a. sudo su - root  ,             
      b. yum install docker -y   ,
      c. systemctl enable  docker --now   ,
      d. systemctl status docker

   ![Screenshot 2023-08-30 181908](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/1bd38c3c-06dc-436f-9780-00f32455229c)


2.create own net name as "psnet"
        subnet range 10.0.0.1/16 in CIDR format
        
  ![Screenshot 2023-08-30 185447](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/e265d278-0646-488d-ae0d-de9c46bcf87c)

      
#docker network ls
#docker network create --driver bridge --subnet 10.0.0.1/16  psnet
#docker network ls


3.Create database with own driver (database- container name)...also provide required enviromental variable

 ![Screenshot 2023-08-30 185342](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/98939e39-6331-4145-9fee-be84232e668e)


#docker run -dit --name database --network psnet -v /mydata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=pratik55  -e MYSQL_DATABASE=mydatabase  -e MYSQL_USER=jack  -e MYSQL_PASSWORD=jack11 mysql

#docker inspect database   <--- here we show our subnet range(10.0.0.1/16) to our given database container

![Screenshot 2023-08-30 185033](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/a6c68e2e-cfea-4aa7-8f79-5d41ba5caa22)

![Screenshot 2023-08-30 185149](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/3f1270fe-8fb0-47e1-85d7-66445d06fec4)

Here,check provided subnet range.

4.Launch wordpress and uase PATTING to make outside world connection 

 As port number of container is 80 also check by #netstat -tnlp
 
 ![Screenshot 2023-08-30 185418](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/b82e4635-d464-44b2-9729-0fc29d532f45)


  #docker run -dit --name mywordpress --network psnet -p 1234:80 wordpress

Can also check MYWORDPRESS has our subnet range by command on above screenshots.( step no.3)
#docker inspect mywordpress

5.EC2 intance has Firewall which cannot be connected by outside world, so we can modify inbound rules(All traffic allowed)

![Screenshot 2023-08-30 182129](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/4c596a4b-39f4-49fa-911f-5dc0a4cc01a1)

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

