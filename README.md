# partner-deployment

Ansible playbook to perform automated deployment and update the SLV application.
Developed on Ansible version 2.7.6

## Tartget System requirements

| Requirement  | Version |
| ------------- | ------------- |
| Ubuntu  | 18.04.2 LTS+  |
| Python3  | 3.7.3  |
| Pip3     |  19.0.3 |


#### Local Machine
These tasks should be run on the machine where the playbook is being run from.

1. Install external dependencies by running the following command within cloned repository

    ```ansible-galaxy install -r roles/requirements.yml -p roles/external/ --force```

#### Target Machine
These tasks should be run on the machine that is being deployed to. They should only need to be done once when performing initial deployment.

1. Generate a SSH key for the server and add it to the deployed keys within the Git repository, see https://help.github.com/articles/connecting-to-github-with-ssh/ and https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys

2. Test ssh connection to Github, see https://help.github.com/articles/testing-your-ssh-connection/
   ```ssh -T git@github.com```
   You should see a message similar to "Hi there! You've successfully authenticated, but GitHub does not provide shell access".

## Running the Playbook

### Initial installation and clean/re-installation

   1. Run the playbook.
      NOTE: Running the below command will wipe out the existing data and the database.

      ```ansible-playbook site.yml -i hosts-<qa/staging> --extra-vars "clean_install=true sample_data=false superuser_password=<NEW PASSWORD>"```

      * `clean_install` - Setting this to true will perform ansible tasks tagged with `clean_install`. Example: Dropping existing data or database.
      * `sample_data` - Setting this to false will not upload seed data to the system.
      * `project_secret_key` - This should be a long random string and is used for security purposes by the app. The easiest way to supply this is to use LastPass to generate a 50 character string and then paste it in this command.

  2. Running the playbook to clean install and have sample data
    NOTE: Running the below command will wipe out the existing data and the database.

     ```ansible-playbook site.yml -i hosts-<qa/staging> --extra-vars "clean_install=true sample_data=true superuser_password=<NEW PASSWORD>"```

     * `clean_install` - Setting this to true will perform ansible tasks tagged with `clean_install`. Example: Dropping existing data or database.
     * `sample_data` - Setting this to true will upload seed data to the system.
     * `project_secret_key` - This should be a long random string and is used for security purposes by the app. The easiest way to supply this is to use LastPass to generate a 50 character string and then paste it in this command.


### Updating an existing installation

   The following command can be run to update an existing deployed instance of the application.
   ```ansible-playbook site.yml -i hosts-<qa/staging>```
