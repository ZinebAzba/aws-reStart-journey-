
---

#  Managing Resources with Tagging — Student Lab Summary  

---

##  **What This Lab Was About**  
In this lab, I learned how powerful AWS resource tagging can be when it comes to organizing, automating, and securing cloud environments.  
Instead of just clicking around the console, I worked directly with:

- AWS CLI  
- JMESPath queries  
- Bash scripting  
- PHP automation scripts  

By the end, I was able to identify resources by tag, update them in bulk, automate start/stop operations, and even enforce a security rule that terminates non‑compliant instances. This lab really helped me understand how tagging ties into real operational tasks.

---

##  **Task 1 — Finding Development Instances Using Tags**

###  What I Did  
I used AWS CLI filters to locate all EC2 instances belonging to the **ERPSystem** project. Then I narrowed the results to only the **development** environment. This was my first time using JMESPath to format CLI output, and it made the results much easier to read.

###  What I Learned  
- How to filter EC2 instances by tag  
- How to extract specific fields (ID, AZ, tag values)  
- How to combine multiple filters (Project + Environment)  



<img width="1349" height="429" alt="Screenshot 2026-04-27 144424" src="https://github.com/user-attachments/assets/f79039ce-1efe-41f3-a466-0154ab307d84" />


---

##  **Task 2 — Updating Tags Using a Bash Script**

###  What I Did  
I opened a pre‑written Bash script that automatically updated the **Version** tag for all development instances. The script used AWS CLI with `--output text` to collect instance IDs and then applied the new tag to all of them at once.

###  What I Learned  
- How to automate tag updates  
- Why `--output text` is useful for scripting  
- How to pass CLI results into another command  


<img width="1345" height="621" alt="image" src="https://github.com/user-attachments/assets/cd6912f4-636e-485e-9ce0-4ee61d09626a" />

---

##  **Task 3 — Automating Stop/Start Operations (Stopinator)**

###  What I Did  
I used a PHP script called **stopinator.php** to stop and restart all development instances. The script searched across all AWS regions and only acted on instances matching the tags I provided.

###  What I Learned  
- How automation scripts can manage resources across regions  
- How tags can control operational behavior  
- How to use command‑line arguments to customize script behavior  



<img width="779" height="119" alt="Screenshot 2026-04-27 145937" src="https://github.com/user-attachments/assets/f0394fe7-d570-4ba5-b460-29a79cf6074e" />
<img width="1526" height="561" alt="Screenshot 2026-04-27 150126" src="https://github.com/user-attachments/assets/07347db3-065b-4470-b3b3-fc849be192f0" />
<br><br>
<img width="854" height="119" alt="Screenshot 2026-04-27 150235" src="https://github.com/user-attachments/assets/299a25f8-b060-40ad-bb14-5c591696bdf2" />
<img width="1524" height="543" alt="Screenshot 2026-04-27 150307" src="https://github.com/user-attachments/assets/b76265df-9ceb-4fae-8b77-0600a6217d66" />


---

##  **Task 3.1 — Challenge: Tag‑or‑Terminate Policy**

###  What I Did  
This was the most interesting part of the lab. I had to enforce a security rule:  
**Any instance in the private subnet that does NOT have an Environment tag must be terminated.**

To test the script, I removed the Environment tag from two private‑subnet instances. Then I ran the termination script with my region and subnet ID.

The script scanned the subnet, identified the non‑compliant instances, and terminated them automatically.

###  What I Learned  
- How to apply security policies using automation  
- How to identify private‑subnet instances  
- Why subnet‑scoped scripts only need to run once per subnet  
- How tag compliance can prevent misconfigured resources from staying online  


<img width="958" height="183" alt="Screenshot 2026-04-27 153347" src="https://github.com/user-attachments/assets/88c3663b-5dc9-4d8b-9073-c92ae5e91516" />

<img width="1285" height="482" alt="Screenshot 2026-04-27 153401" src="https://github.com/user-attachments/assets/aa103171-bcff-421d-9011-635ed7809d2b" />

---

##  **Key Takeaways**

- Tags are not just labels — they drive automation, cost management, and security.  
- JMESPath is extremely useful for shaping AWS CLI output.  
- Scripts can save a lot of time when managing multiple resources.  
- Automating compliance (like tag‑or‑terminate) is a real‑world cloud operations skill.  
- Understanding subnets and tag structure is essential for safe automation.

---

##  **Final Reflection**  
This lab helped me move from simply “using AWS” to actually **managing AWS resources at scale**.  
I now understand how tagging ties into:

- Automation  
- Security  
- Operational efficiency  
- Environment separation (dev vs prod)  

It also gave me hands‑on experience with CLI tools and scripting, which I might use again in future labs or real cloud environments.

---

