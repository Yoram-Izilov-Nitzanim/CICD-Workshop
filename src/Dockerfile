# Use the official Jenkins image
FROM jenkins/jenkins:lts

# Switch to root user to install packages
USER root

# Install socat
RUN apt-get update && \
    apt-get install -y socat && \
    apt-get clean

# Switch back to Jenkins user
USER jenkins

# Expose Jenkins port
EXPOSE 8080
