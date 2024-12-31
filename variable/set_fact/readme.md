# **Ansible Playbook: Setting and Using Facts**

## **Duration**

Approximately 20 minutes

## **Objective**

The goal of this lab is to familiarize yourself with the `set_fact` module in Ansible. You'll learn how to dynamically define custom facts (variables) during playbook execution and then use them in
subsequent tasks. This is particularly useful for cases where values need to be calculated or assigned during runtime.

## **Prerequisites**

- Basic knowledge of Ansible (understanding playbooks and tasks).
- Ansible installed on your control node.
- Access to target hosts in the `webservers` inventory group.

## **Step-by-step Guide**

### **Step A: Set up the Ansible Playbook**

1. **Create a new YAML file:**
    - Open your text editor and create a new file named `facts_playbook.yml`.

2. **Define the play:**
    - In this file, specify the play's name, the hosts to target (in this case, `webservers`), and whether you want to gather facts. In this case, we will disable automatic fact-gathering by setting
      `gather_facts` to `no`. This is important when we want to control which facts are gathered or define custom ones ourselves.

   Example content for `facts_playbook.yml`:

   ```yaml
   ---
   - name: A play with custom variables (facts)
     hosts: webservers
     gather_facts: no
   ```

   **Explanation:**
    - `gather_facts: no` disables automatic gathering of facts like the host's IP address, OS version, etc. This keeps the playbook simple for this lab, but you can enable it if needed.

### **Step B: Define Facts Using the `set_fact` Module**

3. **Add tasks to define custom facts:**
    - Inside the `tasks` section, use the `set_fact` module to define new facts. Facts set using `set_fact` are essentially variables that are available for the rest of the playbook once defined. You
      can set them dynamically based on conditions, values from previous tasks, or as static values.

   Example content:

   ```yaml
     tasks:
       - name: Setting host facts using set_fact module
         set_fact:
           fact_one: "Hello"
           fact_other: "Bye"
           john_fact: "Doe"
   ```

   **Explanation:**
    - `fact_one`, `fact_other`, and `john_fact` are now custom facts that can be referenced later in the playbook.
    - The values you assign can be static (like in this case) or dynamic based on other variables or outputs of previous tasks.

### **Step C: Utilize Facts in Subsequent Tasks**

4. **Use the custom facts:**
    - Now that you've set the facts, let’s use the `debug` module to display them. The `debug` module is handy for troubleshooting and ensuring your facts are being set correctly.

   Example task to print the value of `john_fact`:

   ```yaml
       - name: Print the value of john_fact
         debug:
           var: john_fact
   ```

   **Explanation:**
    - The `debug` module will output the value of the `john_fact` variable. You’ll see the message `"Doe"` printed when this task runs.

5. **Print another fact:**
    - Similarly, let’s print the `fact_other`. The previous version had a typo (`fact_othert`), so be sure to correct it here.

   Correct example:

   ```yaml
       - name: Print the value of fact_other
         debug:
           var: fact_other
   ```

   **Explanation:**
    - This task will print the value of `fact_other`, which is `"Bye"`. If the variable name is incorrect, Ansible will throw an error or display `undefined`, helping you debug variable issues.

### **Step D: Execute the Playbook**

6. **Save the file:**
    - Once your playbook is complete, save it as `facts_playbook.yml`.

7. **Run the playbook:**
    - Open a terminal and run the playbook with the following command:

   ```bash
   ansible-playbook facts_playbook.yml
   ```

8. **Check the output:**
    - You should see the output from the `debug` tasks, displaying the values of `john_fact` (`"Doe"`) and `fact_other` (`"Bye"`).

## **Summary**

In this lab, you learned how to use the `set_fact` module in Ansible to dynamically define facts (variables) during playbook execution. You then saw how to use these facts in subsequent tasks to make
your playbooks more flexible and dynamic. Using `set_fact` allows you to capture and reuse values in your automation process, enhancing your ability to manage complex scenarios where variables might
not be known in advance or need to be set based on conditions during the playbook run.

By mastering the `set_fact` module, you can create more adaptable and powerful Ansible playbooks tailored to specific environments or hosts.