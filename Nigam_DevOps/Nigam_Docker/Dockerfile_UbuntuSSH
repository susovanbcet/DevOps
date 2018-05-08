FROM ubuntu:16.04

# Ubuntu Upgrade
RUN apt-get update -y
#RUN apt-get upgrade -y
RUN apt-get install --no-install-recommends -y software-properties-common
RUN apt-get install openssh-server openssh-client -y
RUN apt-get update

RUN mkdir /var/run/sshd
RUN chmod 0755 /var/run/sshd

RUN useradd -m devops
RUN chmod -R 0700 /home/devops
RUN echo 'devops:devops123' | chpasswd

RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

##ENV NOTVISIBLE "in users profile"
##RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
