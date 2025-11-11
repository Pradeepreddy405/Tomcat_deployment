Project info :

    How to build a CI/CD pipeline that automatically takes your code from GitHub → builds it → and deploys it to a Tomcat server running on AWS EC2.

Tools used   :
    Amazon EC2 (AWS) | Git | GitHub | Jenkins | Tomcat

===============================================================================================================================

Work flow of the project :

[ Developer ]
     |
     | 1. Push code
     v
[ GitHub Repo ]
     |
     | 2. Jenkins pulls latest code
     v
[ Jenkins on EC2 ]
     |
     | 3. Builds app (creates .war file)
     v
[ Tomcat on EC2 ]
     |
     | 4. Deploy app automatically
     v
[ Web App Live 🎉 ]

==================================================================================================================================


Step by step execution :

Step 1; Launch an EC2 Instance --(cloud setup - virutal machine it might be any cloud (AWS,GCP or AZURE ) - Here i have taken AWS_cloud

            Go to AWS Console → EC2 → Launch Instance

            Choose:

            Amazon linux 2023

            Instance type: t2.micro

            Create a key pair (for SSH access)

            In the Security Group, allow:

            Port 22 → SSH

            Port 8080 → Tomcat

            Port 8081 → Jenkins

    Launch the instance

    Connect:ssh -i your-key.pem ubuntu@<EC2-Public-IP>


Step 2; Install Java on Amazon Linux 2023

        Why we need this ?

        -> Jenkins and Apache Tomcat both require Java to run.

        -> We’ll install OpenJDK 17, which is stable and supported by both Jenkins and Tomcat.


Always update packages before installing new ones. (dnf is the default package manager in Amazon Linux 2023 )
command : sudo dnf update -y

Check available Java versions

	command: sudo dnf search OpenJDK

Verify Java home (optional but useful for Tomcat)

Find Java installation path:

	command: readlink -f $(which java)


If not yet setup  , go with below setups

	command: echo 'export JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64' | sudo tee -a /etc/profile
	command: echo 'export PATH=$PATH:$JAVA_HOME/bin' | sudo tee -a /etc/profile
	command: source /etc/profile


step 3 : Step-by-Step: Installing Tomcat 10 on Amazon Linux 2023


	Command : sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.24/bin/apache-tomcat-10.1.24.tar.gz


	Extract the Tomcat files

        After it’s downloaded, extract it using below command :
	Command : sudo tar -xvzf apache-tomcat-10.1.24.tar.gz

	Create a directory called tomcat in opt directory and move the extracted directory folder and files to the newly created folder (tomcat)
        Command: sudo mv apache-tomcat-10.1.24 tomcat

        Give the required permissions for the users to start the tomcat service scripts
	Command: sudo chmod +x /opt/tomcat/bin/*.sh

        Start the tomcat manually
	Command: sudo /opt/tomcat/bin/startup.sh

	check if tomcat service is running by below command:
	Command: ps -ef | grep tomcat

	Check if Tomcat is listening on port 8080
	Command: sudo netstat -tulnp | grep 8080

	(or)
	Command: sudo ss -tulnp | grep 8080

	Optional ----> Tomcat logs everything during startup in the logs folder.

	Commandd :tail -f /opt/tomcat/apache-tomcat-10.1.24/logs/catalina.out
