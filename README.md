# # Django Application Containerized with Docker

This repository contains a simple Django application with one app that is containerized using Docker. The goal is to make it easy to run the Django application in a consistent environment using containers.

# Dockerfile
Dockerfile is a file where you provide the steps to build your Docker Image.


FROM ubuntu:latest
# Set the working directory inside the container
WORKDIR /app

# Copy the entire project directory to /app
COPY . /app

# Update and install Python and pip
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3-venv && \
    apt-get clean

# Create a virtual environment and install the required dependencies
RUN python3 -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install -r requirements.txt

# Set the environment path to the virtual environment
ENV PATH="/opt/venv/bin:$PATH"

# Expose the port your app runs on
EXPOSE 8000

# Define the entry point to run your Python app
ENTRYPOINT [ "python3" ]
CMD [ "manage.py", "runserver", "0.0.0.0:8000" ]

Setup Instructions
1.## Setting up the EC2 Instance

### 1. Launch an EC2 Instance

- Create an EC2 instance in your preferred region using the AWS Management Console.
- Choose a base AMI (e.g., Amazon Linux 2, Ubuntu).
- Make sure your security group allows inbound traffic on port 8000 (or the port your Django app will use).

### 2. SSH into the EC2 Instance

Use the following command to connect to your EC2 instance:
ssh -i <your-key.pem> ec2-user@<your-ec2-public-ip>

3. Install Docker on EC2
Once you're connected to the EC2 instance, install Docker using the following commands:

sudo apt update -y
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
Log out and log back in to apply the user group changes.

5. Clone Your Project to EC2
Clone your Django project from your GitHub repository:
git clone https://github.com/anjana2468/django-docker.git

cd django-docker

7. Build the Docker Image
Now, build your Docker image directly on the EC2 instance:
docker build -t django-app .

8. Run the Container
docker run -p 8000:8000 -it django-app

Project Structure
Here is a basic breakdown of the project files:

python-web-app/
├── Dockerfile                    # Docker configuration file for building the Django app image
├── manage.py                     # Django's primary command-line utility
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation and setup guide
├── .env                          # Environment variables (excluded from version control)
├── project/                      # Django project directory
│   ├── __init__.py               # Marks this directory as a Python package
│   ├── asgi.py                   # ASGI configuration (for async support)
│   ├── settings.py               # Main project settings
│   ├── urls.py                   # Root URL routing for the project
│   ├── wsgi.py                   # WSGI configuration for running Django with a web server
│                
│                  
└── demo/                          # Django application directory
    ├── __init__.py               # Marks this directory as a Python package
    ├── admin.py                  # Admin interface settings for this app
    ├── apps.py                   # App-specific configuration
    ├── migrations/               # Directory for Django migrations
    │   └── __init__.py
    ├── models.py                 # Database models for this app
    ├── tests.py                  # Unit tests for this app
    ├── views.py                  # Logic for handling web requests
    ├── urls.py                   # URL routing for this app
    └── templates/                # App-level templates directory (specific to this app)
        └── index.html/           # Example template file for this app


How to Contribute
Contributions are welcome! If you'd like to improve this project or add new features, feel free to fork the repository and submit a pull request.

1.Fork the repository.
2.Create a new branch with your feature/bug fix.
3.Push the branch and open a pull request.

License
This project is open-source and available under the MIT License.  

Screenshots

  ![application-composer-cloudacademylabs yaml](https://github.com/user-attachments/assets/76028ca9-8bf4-449f-b7c6-12b68e9ff799)
