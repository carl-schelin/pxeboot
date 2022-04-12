### Overview

This repository contains all the scripts necessary to create the openshift cluster for each site.

### Directories

Each directory identifies the site being built. In each directory is a subdirectory that reflects the host where the guest will be built. This ensures balance across the infrastructure. Under each directory is a type of server directory. Management, Control, or Worker. In these directories are the scripts used to build the guests. There is a create and cleanup script for each server.

### Process

This assumes the terraform repo has been run against the hosts as the build server must be available as it will have the ignition and coreos images hosted on it.

This then assumes the playbooks repo has been run, all servers initialized, the dns, haproxy, and build servers provisioned, and the openview preparation playbook run.

Once all guests have been built and provisioned, you can then build the openshift cluster for the site.

Scripts are called create.[servername] and cleanup.[servername] in part because only one can be run at a time.

Step 1. The Bootstrap server has to be built and ready to start the control nodes. This configuration will be in the site/host/management directory which will be on the second host of the site.

Step 2. The Control servers will need to be built. This configuration is balanced and will be on different servers in the site/host/control directory.

Step 3. The Worker servers will then need to be built. Again the configurations are balanced so all workers aren't on one host. The configurations will be in the site/host/workers directory.

### Conclusion

Once you're done with the builds, return to the openshift playbooks in the playbooks repo and run the post installation process. This prepares the build server to access the cluster, approves CSRs, and patches the cluster for storage.

