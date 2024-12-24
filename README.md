# Three Tier Architecture using Docker

## Step: 1 [Install Docker & Start Docker Services]
Launch EC2 instance and install docker inside and start docker Service:

Docker install command:
    
    yum install docker -y 

Start Docker Service command:
   
    systemctl enable  docker --now 

Check Docker Status:

    systemctl status docker
      
![Screenshot 2023-08-30 181908](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/1bd38c3c-06dc-436f-9780-00f32455229c)

## Step: 2 [Create Custom Docker Network]
create own net name as "psnet"

Subnet range 10.0.0.1/16 in CIDR format.
        
![Screenshot 2023-08-30 185447](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/e265d278-0646-488d-ae0d-de9c46bcf87c)

Command for check docker network list:
    
    docker network ls

Docker network create command:

    docker network create --driver bridge --subnet 10.0.0.1/16  psnet
        
## Step: 3 [Launch MySQL Container using own Network]
Create database with own driver (database- container name)...also provide required enviromental variable


 ![Screenshot 2023-08-30 185342](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/98939e39-6331-4145-9fee-be84232e668e)


Launch MySQL container using custom networks & adding environmental variables & Attach Host folder:

    docker run -dit --name database --network psnet -v /mydata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=pratik55  -e MYSQL_DATABASE=mydatabase  -e MYSQL_USER=jack  -e MYSQL_PASSWORD=jack11 mysql

Inspect command for check:(here we see our Subnet range(10.0.0.1/16) to our given database container)

    docker inspect database   
                 
![Screenshot 2023-08-30 185033](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/a6c68e2e-cfea-4aa7-8f79-5d41ba5caa22)

![Screenshot 2023-08-30 185149](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/3f1270fe-8fb0-47e1-85d7-66445d06fec4)


Here, check provided Subnet range.

## Step: 4 [Launch Wordpress Container using own Newtwork]
Launch wordpress and uase PATTING to make outside world connection,As port number of container is 80.

Command for check list port numbers on system:
              
    netstat -tnlp
          
![Screenshot 2023-08-30 185418](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/b82e4635-d464-44b2-9729-0fc29d532f45)

Lanuch Wordpress container using custom network:

    docker run -dit --name mywordpress --network psnet -p 1234:80 wordpress

Can also check MYWORDPRESS has our subnet range by command on above screenshots.(step no.3)
    
    docker inspect mywordpress

## Step: 5 [Edit Inbound rule/ Firewall]
EC2 Intance has Firewall which cannot be connected by outside world, so we can modify Inbound rule(All traffic allowed)

![Screenshot 2023-08-30 182129](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/4c596a4b-39f4-49fa-911f-5dc0a4cc01a1)

Substeps:

1. EC2 Dashboard
2. Celect securtity option
3. Go inside Security groups
4. Select "edit inbound rule"
5. Delete defult rule and new rule
6. custom TCP ->> "ALL TRAFIC", ANYWHERE IPv4
7. Save rule

## Step: 6 [Access Wordpress from Browser]
Instance public + our port number that provide in wordpress contanier.

To access wordpresss from browser,need EC2 instance public IP Address+Port no. given to wordpresss container.

![Screenshot 2023-08-30 182811](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/90f63389-dbfc-40aa-84bb-82d28772da1b)

To get wordpress interface on google ,provide details that provided in the form of enviromental variable during "database" caintainer launch

http://65.2.146.158:1234    <<------- see interface(This site does not work as instance is terminated now)

![Screenshot 2023-08-30 182954](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/6ffc643c-d1bb-47fb-a6e4-4e376b263aa9)


![Screenshot 2023-08-30 183151](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/b8c3d0e1-9a34-4a66-970e-236fe770f9b4)

## Step: 7 [Fill Info]
Create username and password to create blog on wordpress:

![Screenshot 2023-08-30 183243](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/af521986-471a-4b56-a6f7-079cc889c2cf)

![Screenshot 2023-08-30 183447](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/e159eb84-9f1e-4158-8c41-75376ec2cda0)

## Step: 8 [Create Blog page]
On Wordpress Dashboard->create Post->Add Content->Publish->Copy link and paste in the browser.

![Screenshot 2023-08-30 183516](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/552edbcb-1dd5-4467-b99d-328a5bac4c53)

![Screenshot 2023-08-30 184349](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/9cf35c01-a9fb-40a5-9931-187d04b24787)

![Screenshot 2023-08-30 184434](https://github.com/Pratikshinde55/Three-Tier-Architecture/assets/145910708/c76e18ff-f6ea-4eba-b4dd-e18ce00331d7)


### This is Three Tier Architecture using Docker Overview:
1. WordPress-Blogging Site
2. MySQL-Database 
3. PSNet-Own Bridge driver

