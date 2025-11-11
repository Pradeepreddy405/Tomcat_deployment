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


