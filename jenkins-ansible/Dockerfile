FROM jenkins/jenkins                                

#switching to root user as installing packages with apt-get required admin priviledges
USER root                                           
 
#updates the debian packages and install pip(python package manager)
RUN apt-get update && apt-get install python3-pip -y 
 
#Creates an environment variable ANSIBLE_VENV pointing to /ansible_venv
ENV ANSIBLE_VENV=/ansible_venv

#Creates the directory for the virtual environment (/ansible_venv).
#Changes the ownership to the jenkins user (so Jenkins can write into it later).
#Installs the python3-venv package — required to create Python virtual environments
RUN mkdir $ANSIBLE_VENV && \
    chown jenkins:jenkins $ANSIBLE_VENV && \
    apt-get install python3-venv -y
 
#Running Jenkins jobs as root is discouraged for security reasons, 
#so you go back to the safer jenkins user after installations
USER jenkins
 
#Creates a Python virtual environment in /ansible_venv.
#This keeps Ansible and its dependencies isolated from the system Python
RUN python3 -m venv $ANSIBLE_VENV
 
#Installs Ansible inside the virtual environment using pip.
#The $ANSIBLE_VENV/bin/pip3 path ensures the installation happens in the virtual env, not system-wide.
RUN $ANSIBLE_VENV/bin/pip3 install ansible
 
# Ensure the Ansible binary is accessible
ENV PATH=$PATH:$ANSIBLE_VENV/bin