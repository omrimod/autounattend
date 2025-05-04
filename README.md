# autounattend
Using windows autounattend.xml

I recently tried to play with windows unattended installation.

My goals were:
  * To create an image.
  * To depoly it with minimum user interaction.
  * To join the computer to the domain.

My main problem was i do not have a DHCP server in my organization. Because of that, i did not manage to find an easy way to give a static IP address during an unattended installation.

After alot of messing and using both ChatGPT and googling i managed doing that with the follwing powershell scripts, that i embded in the specialize part of the xml:

	1. powershell.exe -c "$ip = read-host 'Enter ip address'; $Prefix = read-host 'Enter prifix (8, 16, 24, etc...)'; $DG= read-host 'Enter default gateway '; New-NetIPAddress -InterfaceAlias Ethernet -IPAddress $ip -PrefixLength $prefix -DefaultGateway $DG"
 
	2. powershell.exe -c "$DNS = read-host 'Enter DNS Server (For multiple servers user ,)'; Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses $DNS"
 
	3. powershell.exe -c "$name = read-host 'Enter computer name'; rename-computer -newname $name"
     
In the scripts above, after Windows will finish its installtion, the user will be promted to enter IP address, subnet mask, default gateway, DNS servers and a computer name.

To use the autounattend.xml i created, you will need to edit the xml with your domain name and credantials.

