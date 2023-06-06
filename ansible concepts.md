 ## Ansible playbook
 ``` yaml
 # playbook.yml

---
- name: Example Playbook
  hosts: all
  become: true

  tasks:
    - name: Install Apache web server
      apt:
        name: apache2
        state: present

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: true

    - name: Copy configuration file
      copy:
        src: files/httpd.conf
        dest: /etc/apache2/httpd.conf
        owner: root
        group: root
        mode: 0644

    - name: Restart Apache service if configuration changed
      service:
        name: apache2
        state: restarted
      when: "'httpd.conf' in ansible_facts.changed_files"

    - name: Enable firewall rule
      ufw:
        rule: allow
        port: 80
 ```
 ``` bash
 ansible-playbook playbook.yml

 ```
 
 ## Ansible Inventory with playbook
 
 
 ``` bash
 # inventory.ini

[webservers]
web1 ansible_host=192.168.0.10 ansible_user=admin

[webservers:vars]
http_port=80
https_port=443

[databases]
db1 ansible_host=192.168.0.20 ansible_user=dbadmin

[loadbalancers]
lb1 ansible_host=192.168.0.30 ansible_user=lbadmin 
 ```
 
 ``` yaml
 
 # inventory.yml

all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.0.10
          ansible_user: admin
      vars:
        http_port: 80
        https_port: 443

    databases:
      hosts:
        db1:
          ansible_host: 192.168.0.20
          ansible_user: dbadmin

    loadbalancers:
      hosts:
        lb1:
          ansible_host: 192.168.0.30
          ansible_user: lbadmin

 ```
 
 ``` bash
 ansible-playbook -i inventory.ini playbook.yml

 ```
 ## Ansiblehandler
 ``` yaml
 - name: Restart Apache
  service:
    name: apache2
    state: restarted
  notify: 
    - Restart Nginx
    - Reload Firewall

- name: Restart Nginx
  service:
    name: nginx
    state: restarted

- name: Reload Firewall
  command: firewall-cmd --reload

 
 ```
 
 ## Ansible notify
 
 ``` yaml
 - name: Install Nginx
  apt:
    name: nginx
    state: present
  notify:
    - Start Nginx Service

- name: Start Nginx Service
  service:
    name: nginx
    state: started

 ```
 
 ## Ansible Vault
 ### Commands of ansible-vault
 ``` bash
 ansible-vault create secret.yml
 ansible-vault encrypt secret.yml
ansible-vault decrypt secret.yml
ansible-vault edit secret.yml
ansible-playbook --ask-vault-pass playbook.yml
ansible-playbook --vault-password-file=my_vault_password.txt playbook.yml
 ```
 ``` yaml
 # secret.yml (Encrypted YAML file)
---
api_key: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          66346463306331396538313565383433623837313163393435373865396661666636333962313236
          6538653432356230376133643531363466343932363662320a376261653964633334373462646330
          36383038323562396234616166623762313537343134353030303631323762356162313931373366
          3530363661663136310a343739376462643733366562613532646632613835303763336636663032
          3365

 ```
 
 
 ## Ansible Galaxy
 ``` bash
 ansible-galaxy init <role-name>
 ansible-galaxy search <keywords>
 ansible-galaxy install <role-name>
 ansible-galaxy install <collection-name>
 ansible-galaxy remove <role-name>
 
 ```
 ``` yaml
 # playbook.yml

---
- name: Example Playbook
  hosts: all
  become: true

  roles:
    - { role: username.role-name, variable1: value1, variable2: value2 }

  tasks:
    - name: Task 1
      # Task details here

    - name: Task 2
      # Task details here

 
 ```
 
