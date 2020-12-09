# Step 1:

Create a named pipe on the docker host:

    # sudo mkfifo /root/docker_command_pipe

# Step 2:

Make a script that scans this pipe and executed command getting out of it

    # sudo vim /root/docker_command_executer.sh
    #!/bin/bash
    while true; do
        eval "$(cat /root/docker_command_pipe)";
    done

# Step 3:

Make the script from step 2 executable

    # sudo chmod +x /root/docker_command_executer.sh

# Step 4:

Add the script to cron

    # sudo crontab -e
    
    @reboot /root/docker_command_executer.sh

# Step 5:

Link the pipe to a docker container

    eg. docker run -v /root/docker_command_pipe:/root/docker_command_pipe -it ubuntu bash

# Step 6: 

Test if it works: (on host part is done by crontab normaly)

    on host: while true; do eval "$(cat /home/ycadmin/named_pipe)"; done
    in container: echo "ls -l" > /root/docker_command_pipe


(see https://stackoverflow.com/questions/32163955/how-to-run-shell-script-on-host-from-docker-container)
