<<<<<<< HEAD
# CyberSec-Memory-Dump

Memory Forensics - Hands-On

Scenario: Recovering Sensitive Data from Process Memory


Part 1: Extracting Data from a Running Text Editor

For part 1 , we created a text file containing the given credentials and we kept it open (text file).

Username: forensic_user
Password: SuperSecretPass123!

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture1.png?raw=true)


and started to dump the system memory 

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture2.png?raw=true)


and filtered the results to find the ‘Username’

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture3.png?raw=true)

and the ‘Password’

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture4.png)


Part 2: Extracting Data from a Web Browser’s Memory

For this task we provided some credentials to any login page on a web browser and tried to get the credentials from memory dumps.

we can see clear results in the parrot linux terminal:





COMMAND: ps aux | grep firefox

The PID was 2381 as found in the terminal for firefox process:


Then dumped the browser memory using this command: 
COMMAND: sudo gcore -o firefox_memory 2381

and extracted the Potential Credentials
Search for sensitive data:
COMMAND: strings firefox_memory.2381 | grep "forensic_user"
COMMAND: strings firefox_memory.2381 | grep "TestPassword"



we can see clear results in the parrot linux terminal:


![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture5.png)






Part 3: Analyzing a Suspicious Running Process


For this task, we are creating a backdoor.txt file and running the .txt file in the background for forensics.

These are the commands used:

Simulate an attacker’s script:
COMMAND: echo "This is a secret backdoor key: R3dT34m123!" > backdoor.txt
COMMAND: cat backdoor.txt > /dev/null &
Find the process:
COMMAND: ps aux | grep cat

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture6.png)

In my case the command: 
COMMAND: ps aux | grep cat

was not working well probably because of compatibility issues on my arm64 CPU. 

so: 
COMMAND: ps aux | grep ‘[c]at’

ps aux | grep cat is not exactly the same as ps aux | grep '[c]at' because the later doesn’t match itself with the grep process. The grep command also exists in the results of ps aux | grep cat, as its string part is "cat".  Here using regular expression [c]at overcomes this, and only the processes containing cat (such as cat command) will be listed without grep command in the results.

And then I dumped and extracted Strings using commands: 
COMMAND: sudo gcore -o backdoor_memory 31877
COMMAND: strings backdoor_memory.31877 | grep "R3dT34m123!"


![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture7.png)

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture8.png)



Here [1]+ done shows that the string was present in the memory dump file but could not display it on the terminal (compatibility issues)



Confirming on other linux:

![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture9.png)


![image alt](https://github.com/DpkYdv887/DpkYdv887-CyberSec-Memory-Dump/blob/main/images/Picture10.png)


Here too , [1]+ done shows that the string was present in the memory dump file but could not display it on the terminal.



