<h1>DNSSEC-Lab</h1>

<h2>Description</h2>
DNS is inherently insecure.  Recursive resolvers are unable to directly authenticate responses to their queries, so they rely on the source IP address of the response packet.  Because spoofing a source IP address is an elementary task for threat actors today, it's more important than ever to protect the critical service that is DNS.  DNSSEC, or DNS Security Extensions, uses public key cryptography to digitally sign the data within the child zone.  The zone's public key is also signed and stored with the data so any recursive resolver can authenticate the data that it receives.  The purpose of this lab is to set up and configure DNSSEC on a test-network's DNS server, create a group policy object for computers in the domain, and then confirm operability.
<br />


<h2>Environments Used </h2>

- <b>Windows Server 2019</b>
- <b>Windows 10 Pro</b>

<h2>Project walk-through:</h2>

<p align="center">
Launch Server Manager and select <b>DNS</b> from the Tools drop-down menu: <br/>
<img src="https://i.imgur.com/WtYALTY.png" height="80%" width="80%"/>
<br />
<br />
Right-click the desired domain and select 'Sign the Zone' to begin the DNSSEC Wizard:  <br/>
<img src="https://i.imgur.com/gq6vTGf.png" height="80%" width="80%"/>
<br />  
<br />
Select 'Customize zone signing parameters.' and click 'Next': <br/>
<img src="https://i.imgur.com/sqqmwke.png" height="80%" width="80%" />
<br />
<br />
This is the only authoritative server in this environment so this server will be the Key Master:  <br/>
<img src="https://i.imgur.com/1hcDbzN.png" height="80%" width="80%" />
<br />
<br />
Click 'Add' to create the Key Signing Key.  This is the root of trust that will be used to sign the public key that is attached to the zone:  <br/>
<img src="https://i.imgur.com/mHsr08T.png" height="80%" width="80%" />
<br />
<br />
For the initial implementation, select 'Generate new signing keys.' For this lab, other parameters will be left at the default settings:  <br/>
<img src="https://i.imgur.com/nxdEpcX.png" height="80%" width="80%" />
<br />
<br />
Once the Key Signing Key is created, it's time to create the Zone Signing Key that will be used to digitally sign the zone data:  <br/>
<img src="https://i.imgur.com/Jp3xfxA.png" height="80%" width="80%" />
<br />
<br />
Finally, use NSEC3 for authenticated denial of existence as a countermeasure against DNS enumeration:  <br/>
<img src="https://i.imgur.com/aTdmyTQ.png" height="80%" width="80%" />
<br />
<br />
With the initial configuration complete and the keys generated, it's time to create a new group policy for enforcement!:  <br />
<img src="https://i.imgur.com/v0HXFnK.png" height="80%" width="80%" />
<br />
<br />
Create a GPO in this domain, and Link it here... Name it and select OK:  <br />
<img src="https://i.imgur.com/bY66lBA.png" height="80%" width="80%" />
<br />
<br />
Once the GPO is created, right-click and edit. Under Computer Config, Windows Settings, Name Resolution Policy, enter the domain in the right pane and check 'Enable DNSSEC in this rule' and 'Validation'. 
Scroll down and click 'Apply':  <br />
<img src="https://i.imgur.com/JyOYEbn.png" height="80%" width="80%" />
<br />
<br />
A quick ipconfig /all over on the client machine to confirm some settings.  Use gpupdate /force to make sure the client has the current group policy settings:  <br/>
<img src="https://i.imgur.com/peLdN8i.png)" height="80%" width="80%" />
<br />
<br />
Finally, use Powershell to confirm DnsSecValidationRequired = True and that all of the parameters were properly configured and enforced.
<img src="https://i.imgur.com/SJOClXV.png" height="80%" width="80%" />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
