FROM ubuntu as nigamssh

MAINTAINER Nigam Rout <nigam.anand@gmail.com>
LABEL maintainer="nigam.anand@gmail.com"

# Upgrade Ubuntu
RUN apt-get update -y



# Install pre-requisites for OPEN SSH Server
RUN apt-get install --no-install-recommends -y software-properties-common

# Install OPEN SSH Server
RUN apt-get install openssh-server openssh-client -y

# Upgrade Ubuntu
RUN apt-get update -y



# Create the directory /var/run/sshd
RUN mkdir /var/run/sshd

# Set appropriate permission to Folder
RUN chmod 0755 /var/run/sshd



# Create new user 'devops' with its home directory
RUN useradd -m devops

# Set appropriate recursive permission to User Home directory
RUN chmod -R 0700 /home/devops

# Set the 'devops' user Password to 'devops123'
RUN echo 'devops:devops123' | chpasswd



# Check Current User
RUN whoami

# switch user to 'devops'
USER devops

# Check Current User
RUN whoami



# Generate RSA a key with out any password
RUN ssh-keygen -t rsa -f /home/devops/.ssh/id_rsa -P ""

# Creating 'authorized_keys' file
RUN touch /home/devops/.ssh/authorized_keys

# Printing the content of 'id_rsa.pub' file
RUN cat /home/devops/.ssh/id_rsa.pub



# switch user to 'root'
USER root
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile



# Exposing port 22
EXPOSE 22

# Run the SSH service
CMD ["/usr/sbin/sshd", "-D"]
