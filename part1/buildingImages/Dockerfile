# Useing alpine image for it's smaller size
FROM alpine:3.16.2

# Use /usr/src/app as our workdir. Rest of the instructions will be executed at this location
WORKDIR /usr/src/app

# Copy the hello.sh file from localhost to the images workdir
COPY hello.sh .

# Add permissions
RUN chmod +x hello.sh

# Execute a command with '/bin/sh -c' prefix
RUN touch additional.txt

# When running the image, execute the following command
CMD ./hello.sh
