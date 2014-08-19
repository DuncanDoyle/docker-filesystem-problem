# Docker Filesystem problem

These 2 Dockerfile instructions demonstrate a problem with the filesystem in our Doccker containers. The problem can be reproduced on a Mac OS X system with 'boot2docker v1.1.2'. Strangely enough, the problem doesn't seem to occur when building the Docker images on a Red Hat Enterprise Linux 6 system.

## The problem
In the container that has the filesystem problem, we can't 'cd' into the directory '/opt/jboss-as-7.1.1.Final/standalone/tmp/auth' with the 'jboss' user, even though the 'jboss' user is the owner of that directory and permissions are set to 700. Also note that, although the user is not allowed to navigate into the directory, he is allowed to delete the directory.

The difference between the 2 Docker images is that in the image with the problem, we unzip JBoss AS 7 with the 'root' user and than do a recursive chown over the JBoss AS installation directory: 'chown -R jboss:jboss /opt/jboss-as-7.1.1.Final'. In the image that doesn't have the problem, we first create the '/opt/jboss-as-7.1.1.Final' directory, make 'jboss' the owner, change to the 'jboss' user, and unzip the JBoss AS 7 distribution with the 'jboss' user.

## Building the images
To build the images, the JBoss AS v7.1.1 distribution is required. The zip file can be downloaded from this location: http://repo2.maven.org/maven2/org/jboss/as/jboss-as-dist/7.1.1.Final/jboss-as-dist-7.1.1.Final.zip  

Copy the zip file to both 'fs_problem' and 'no_fs_problem' directory.

To build the Docker image that has the filesystem problem, navigate into the 'fs_problem' directory and execute the following command: `docker build --rm -t johndoe/jbossas-fs-problem .`

To build the Docker image that doesn't have the filesystem problem, navigate into the 'no_fs_problem' directory and execute the following command: `docker build --rm -t johndoe/jbossas-no-fs-problem .`

## Running the containers.
Start the container which doesn't have the filesystem problem with the following command:  `docker run -t -i --name jbossas-no-fs-problem -h jbossas-no-fs-problem johndoe/jbossas-no-fs-problem bash`
In the container shell, make sure that you are the 'jboss' user (i.e. by using the 'whoami' command). Navigate to the directory '/opt/jboss-as-7.1.1.Final/standalone/tmp/auth/'.

Next, start the container which does have the filesystem problem with the following command: `docker run -t -i --name jbossas-fs-problem -h jbossas-fs-problem johndoe/jbossas-fs-problem bash`
In the container shell, make sure that you're the 'jboss' user (i.e.  by using the 'whoami' command). Navigate to the directory '/opt/jboss-as-7.1.1.Final/standalone/tmp/auth/'. Observe that in this container you will get a 'Permission denied', even though the 'jboss' user is owner of the directory and permission of the directory are set to 700.

