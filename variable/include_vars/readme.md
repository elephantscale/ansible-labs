# **Using `include_vars` in Ansible - Simple Example Without Conditions**

## **Overview**

In this lab, you will learn how to use `include_vars` to include variable files into your playbook. This simple example will show you how to split variables across multiple files and load them using `include_vars`.

## **Objectives**

- Use `include_vars` to load variables from external files.

## **Prerequisites**

- Ansible installed on your control node.
- At least one managed host.

## **Duration**

15 minutes

## **Instructions**

### **Step 1: Create Variable Files**

1. **Create a General Variable File:**

   - Create a file named `general_vars.yml` with the following content:

     ```yaml
     app_name: "MyApp"
     app_port: 8080
     ```

2. **Create an Environment-Specific Variable File:**

   - Create a file named `env_vars.yml` with the following content:

     ```yaml
     env: "development"
     db_host: "localhost"
     db_user: "dev_user"
     db_password: "dev_password"
     ```

### **Step 2: Create the Main Playbook**

1. Create a playbook file named `include_vars_play.yml` with the following content:

   ```yaml
   ---
   - hosts: all
     gather_facts: no

     tasks:
       - name: Include general variables
         include_vars: general_vars.yml

       - name: Include environment-specific variables
         include_vars: env_vars.yml

       - name: Print app information
         debug:
           msg: "App: {{ app_name }}, Port: {{ app_port }}"

       - name: Print database information
         debug:
           msg: "Environment: {{ env }}, DB Host: {{ db_host }}, User: {{ db_user }}"
   ```

### **Step 3: Run the Playbook**

1. Save the playbook and variable files.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook include_vars_play.yml
   ```

3. The playbook will load variables from both `general_vars.yml` and `env_vars.yml`, and you will see the variable values printed in the debug messages.

## **Conclusion**

In this lab, you’ve learned how to use `include_vars` to load variables from multiple files into an Ansible playbook. This technique allows you to organize variables into separate files and load them as needed.
