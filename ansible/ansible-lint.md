# Introduction for Ansible Lint

### Ansible Lint is a command-line tool that checks Ansible playbooks, roles, and collections for best practices and potential issues. 
## Basic Usage: 
To lint a specific playbook: 
```bash
ansible-lint my_playbook.yml
```

## Common Commands and Examples: 
Linting a directory or multiple files. 
```bash
    ansible-lint . # Lints all playbooks and roles in the current directory
    ansible-lint playbook1.yml playbook2.yml # Lints specific playbooks
```

## Listing available rules. 
```bash
    ansible-lint -L
```

## This command displays a list of all built-in rules, their descriptions, and tags. Skipping specific rules. 
```bash
    ansible-lint --skip-list=no-changed-when,command-instead-of-module my_playbook.yml
```

## This command skips the no-changed-when and command-instead-of-module rules during the linting process. Enabling experimental rules. 
```bash
    ansible-lint --enable-list=experimental my_playbook.yml
```

## This command enables experimental rules for linting. Excluding paths from linting. 
```bash
    ansible-lint --exclude roles/old_role/ my_project/
```

## This command excludes the old_role directory within the roles directory when linting the my_project directory. Generating an ignore file.
```bash 
    ansible-lint --generate-ignore my_playbook.yml
```

## This command creates an .ansible-lint-ignore file, which lists all violations found in my_playbook.yml and can be used to permanently ignore them. Using a custom configuration file. 
```bash
    ansible-lint -c path/to/custom-ansible-lint.yml my_playbook.yml
```

## This command uses a specified custom configuration file instead of the default .ansible-lint file. Checking the version. 
```bash
    ansible-lint --version
```
## This command displays the installed version of Ansible Lint. 

