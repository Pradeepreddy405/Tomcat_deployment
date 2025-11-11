Project info :

    How to build a CI/CD pipeline that automatically takes your code from GitHub → builds it → and deploys it to a Tomcat server running on AWS EC2.

Tools used   :
    Amazon EC2 (AWS) | Git | GitHub | Jenkins | Tomcat



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

