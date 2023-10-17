Creating a VM Oracle box is tiresome; if you want a quick interactive shell, you can deploy a quick docker container in a few steps.

(An alternative to this is deploying an Amazon Lightsail environment; you can get provision very powerful instances for free (for the first 3 months) at the click of a button)

In this example, I am submitting the commands used to run a Redhat Linux container.

1. Download Docker from https://www.docker.com/products/docker-desktop/ or https://docs.docker.com/get-docker/ 
   `docker --version` once its downloaded and set up to ensure that everything is as expected
3. Create a Dockerfile, name it 'Dockerfile'

```
# Use Red Hat Universal Base Image 8 as a base
FROM registry.access.redhat.com/ubi8/ubi:latest
#change base image

# Update base image
RUN yum -y update && \
    yum clean all

# Non-root user called 'labman'
RUN useradd -m -s /bin/bash labman

# Setting Password for labman; Be weary of security concerns of plaintext passwords.
RUN echo "labman:password" | chpasswd 

# Setting environment variables
ENV HOME /home/labman

# Setting non-root user as the default user
USER labman

# Setting the working directory to the home directory of 'labman'
WORKDIR /home/labman

# Run a command when the container starts (optional)
CMD ["/bin/bash"]
```
3. Build the Image
   `docker build -t redhat_lab_image:latest .` . assumes you are running the command from the directory of this Dockerfile.
4. Run the container
 `docker run -it --name redhat_lab redhat_lab_image /bin/bash` this will run the image, and an interactive shell will be open. Exit it once you're done.
 `docker stop redhat_lab` to stop the container; `docker rm redhat_lab` to remove the container when you are done.
5. To restart the containter:
  `docker start redhat_lab`
  `docker attach redhat_lab` and the interactive shell will be available.

These are quick steps to create a quick working interactive shell.




   
