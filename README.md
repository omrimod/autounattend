# autounattend
Using windows autounattend.xml
Hello!
I recently tried to play with windows unattended installation.
My goals were to create an image, deploy it with minimume user inputs, that the computer will automaticly join the domain.
My main problem was i do not have a DHCP server in my organization. :(
My wall was trying to give the computer an ip address automaticly and changing its name.
After alot of messing around i manged doing that with the follwing powershell scripts, that i embded in the specialize part of the xml.
  powershell.exe -c "$ip = read-host 'Enter ip address'; $Prefix = read-host 'Enter prifix (8, 16, 24, etc...)' ;$DG= read-host 'Enter default gateway ' ;New-NetIPAddress -InterfaceAlias Ethernet -IPAddress $ip -PrefixLength $prefix -DefaultGateway $DG"
   powershell.exe -c "$DNS = read-host 'Enter DNS Server (For multiple servers user ,)' ; Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses $DNS"
   powershell.exe -c "$name = read-host 'Enter computer name'; rename-computer -newname $name"
In the scripts above, after Windows will finish its installtion, the user will be promted to enter IP address, subnet mask, default gateway, DNS servers and a computer name.

To user the autounattend.xml i created, you will need to edit the following lines:
<Identification>
    <Credentials>
      <Domain>your.domain</Domain> #Enter your domain name here E.G, domain.local.
      <Password>password</Password> #Enter the password of the user that will enter the computer to the domain (must have approprite permissions).
      <Username>username</Username> #Enter the username of the user the will enter the computer to the domain.
  </Credentials>
  <JoinDomain>your.domain</JoinDomain> #Enter the domain name here.
  <MachineObjectOU>OU=Your,OU=PATH,DC=YOUR,DC=DOMAIN</MachineObjectOU> #You can delete this line, and the computer will be created in default computers OU. 
</Identification>
