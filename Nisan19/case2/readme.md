# Kodluyoruz Trendyol System_Infra Bootcamp Case 2 

In this case  worked on deploying a Wordpress - MariaDb - Nginx Container via Ansible to destination server.


# File Structure 

File structure of this case can be seen below 


> │   ansible.cfg
│   hosts
│   installdocker.yml
│   requirements.yml
│   wordpress-docker-nginx.yml
│
├───defaults
│       main.yml
│
└───roles
    └───wordpress-docker
        ├───tasks
        │       main.yml
        │
        └───templates
                case.j2
                docker-compose.j2
                wordpress-nginx.j2


## Approach 

I mostly combined existing projects from Ansible-Galaxy and Github .
Both Ansible Server and Deployment Server are working on Centos 8 . Both Server has  *root * 
privilege's and deployed on *Virtualbox*. 

Ansible Server Ipv4 Address was :

> 192.168.135.4

and  Deployment Server's Ipv4 Address  was :

> 192.168.135.3



 

##  Pre-Instructions

First of all make sure you pointed out your Deployment Server's Ip address and Domain name in Ansible Server's /etc/hosts file and /etc/ansible/hosts file.

    nano /etc/hosts  then add your config to the last line 
    192.168.135.3 ansible.case
After that configure your *SSH* configuration  for passwordless connection.

       ssh-keygen
       ssh-copy-id root@ansible.case 
       ssh root@ansible.case

Create a directory for your Ansible project and then Create *hosts* file  to point our deployment server Ipv4 address. 

    [root@localhost case2]# cat hosts 

    192.168.135.3  ansible_user=root

Create a *ansible.cfg* file for further configuration .

    [root@localhost case2]# cat ansible.cfg 
    [defaults]
    retry_files_enabled = False
    host_key_checking = False[

Now we are ready to use Ansible and can use ad-hoc commands to confirm our setup.
  


## Instructions 

In this section i will explain how to use this project. 

To install our Dependency *Docker and Docker Compose * Ansible-Galaxy Roles execute this command with `requirements.yml`.

       ansible-galaxy install -r requirements.yml 
   This will install `geerlingguy.docker and geerlingguy.pip ` to our Ansible server. 
 After that we can execute our playbook which will  install `Nginx, Wordpress , Mariadb ` by   creating a `docker compose` file via Ansible.
 

     ansible-playbook wordpress-docker-nginx.yml 
 When our playbook plays we can access our WordPress template  from browser.

## Variables and Various Files 

We can set some variables  in our 

    ├───defaults / main.yml
  This variables include : 

    ---
    system_user: root
    compose_project_dir: /home/{{ system_user }}/compose-wordpress
    
    
    domain: 192.168.135.3
    wp_version: 5.0.2
    wp_db_name: wordpress
    wp_db_tb_pre: wp_
    wp_db_host: mysql
    wp_db_psw: change-M3     
   

      //////////////////////////////////////////////////////////////////////////////////
        wp_version : Wordpress version of our docker file.  This project works with  ***- php7.0-fpm type docker files.
        wp_db_name = WORDPRESS_DB_NAME
        wp_db_tb_pre = WORDPRESS_TABLE_PREFIX
        wp_db_host = WORDPRESS_DB_HOST
        wp_db_psw =  WORDPRESS_DB_PASSWORD

### Playbooks and Configuration Files 
     * installdocker.yml
     Installs Docker and Docker Compose to our Destination Server. This playbook is included in our main (wordpress-docker-nginx.yml) playbook. 
     * wordpress-docker-nginx.yml
     This is our main playbook. It has some variable prompts. 
     * └───roles
		    └───wordpress-docker
		        ├───tasks
			        │       main.yml
		        │
		        └───templates
		                case.j2
		                docker-compose.j2
		                wordpress-nginx.j2
	  This is our Docker Compose Role.
		  * main.yml 
		  This playbook consists of 5 tasks 
		    ** setup compose dir structure
		    ** create docker-compose.yml file
		    ** deploy Docker Compose project for (Wordpress/MariaDB/Nginx containers)
		    ** deploy WordPress Nginx virtual host
		    ** deploy static page to Nginx
		    ** Run docker-compose up
		  * case.j2
		    ** Simple Php file for *bootcamp=devops* http header
		  * docker-compose.j2
		    ** Docker compose file works in Destination Server after builded by Ansible
		  * wordpress-nginx.j2
		    ** Nginx Configration File. Also Contains Configration for our static  case.php http file. 
	  
     
---------------------
# Static Page
If you send a special http request with `bootcamp=devops` http header  you will be granted in a specific static and simple text  web page called `case.php`

# Screenshots 

You can see various Screenshots in Screenshots folder about the project. 

# License 
M.I.T LICENSE



