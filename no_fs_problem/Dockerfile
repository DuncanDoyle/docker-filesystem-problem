# VERSION 0.0.1
#
# Steps taken to create this image
# docker build --rm -t johndoe/jboss-as-7.1.1 .
# docker run -t -i johndoe/jboss-as-7.1.1 bash
# 
# Get required ZIP file from: http://repo2.maven.org/maven2/org/jboss/as/jboss-as-dist/7.1.1.Final/jboss-as-dist-7.1.1.Final.zip
#
FROM centos:centos6
MAINTAINER Duncan Doyle "Duncan.Doyle@gmail.com"

# First update to the latest packages.
#Install required software from CENTOS repo's. Note that these commands are cached by default. So when a container has already been built, it will not be built again.
RUN yum update -y
RUN yum install -y java-1.7.0-openjdk java-1.7.0-openjdk-devel unzip
RUN yum clean all

# Change root password.
RUN echo "root:password" | chpasswd

# Add additional software, which we don't fetch from the CENTOS repo's, to the /opt dir.
WORKDIR /opt

# Add the JBoss user.
RUN useradd -m -d /home/jboss -p jboss jboss
ENV HOME /home/jboss

# Create the target dir for the JBoss Server in /opt.
RUN mkdir /opt/jboss-as-7.1.1.Final
RUN chown jboss:jboss /opt/jboss-as-7.1.1.Final

USER jboss

# Install JBoss AS 7.1.1
ADD jboss-as-7.1.1.Final.zip /tmp/
RUN unzip -q /tmp/jboss-as-7.1.1.Final.zip

# Expose the required ports
EXPOSE 22
EXPOSE 9990
EXPOSE 9999
EXPOSE 8080

# Set the workdir to our JBoss directory.
WORKDIR /opt/jboss-as-7.1.1.Final
