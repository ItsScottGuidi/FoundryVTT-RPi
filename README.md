# FoundryVTT-RPi

I am going to show you how to setup FoundryVTT on a Raspberry Pi.

# Installing Raspberry Pi OS

<img width="678" alt="Pi Imager" src="https://github.com/user-attachments/assets/c50af991-14c9-4ff5-bba9-7633a6b9f462" />

<br>
</br>
Go to https://www.raspberrypi.com/software/ and download Raspberry Pi Imager for your OS. It is available on Windows, MacOS, Ubuntu for X86 and also for Raspberry Pi. 
<br>
</br>
To install on Raspberry Pi OS, type
<pre><code>sudo apt install rpi-imager</code></pre>
in a Terminal window.
<br>
</br>
Insert your Micro SD Card into your SD Card reader.
<br>
</br>
Next under Raspberry Pi Device press choose device, in this example I choose Raspberry Pi 4.
<br>
</br>
Next under Operation System press on the choose OS button go to and select Raspberry Pi OS (Other), then Raspberry Pi OS Lite (64-bit).
<br>
</br>
Next choose your storage and advance to the next screen pressing the Next button.
<br>
</br><img width="682" alt="OS Customiser" src="https://github.com/user-attachments/assets/801ead80-240f-46b3-bedd-d0ba046f14c0" />
<br>
</br>Press on Edit Settings.
<br>
</br><img width="539" alt="OS Customiser-p2" src="https://github.com/user-attachments/assets/062548c9-b44f-4bb2-9637-f475e7690b90" />
<br>
</br>
Now set a hostname for the Pi. In this example we choose pi.local then, set a username and password for logging in to the Pi. Assign your WiFi credentials. I do this even though I will connect the Pi via ethernet when I am done. Next set your Time Zone and Keyboard layout.
<br>
</br>
Now tab over to the services tab and turn on SSH, enabling Use Password Authentication.
<br>
</br>
<img width="540" alt="EnableSSH" src="https://github.com/user-attachments/assets/289b1508-ef5a-4ffb-be69-e84aac42ad26" />
<br>
</br>
Next tab over to Options and select all three boxes.
<br>
</br>
<img width="540" alt="Options" src="https://github.com/user-attachments/assets/8e8b1dd4-94e9-48d4-b861-f3cfb98b9636" />
<br>
</br>
Press save then select Yes.
<br>
</br>
<img width="679" alt="OS Customiser-p3" src="https://github.com/user-attachments/assets/fd246c9c-4fca-4143-95d8-da8e9256a1f3" />
<br>
</br>
Select storage and begin writing the OS. Once the Imager is finished, the storage device will automatically eject itself.
<br>
</br>
<h1>Update the Pi</h1>
<br>
</br>
Place the Micro SD Card in your Pi and switch on your device. From another PC or Mac launch the Terminal app and login to the Pi using SSH, using the following command. <pre><code>ssh piusername@ip-address-of-pi</code></pre>
When prompted type in your password for the Pi and answer Yes to fingerprint. Type in password when prompted.
<br>
</br>
Type in <pre><code>sudo apt update</code></pre> then <pre><code>sudo apt upgrade</code></pre> to update the Pi. Typing in 'Y' when appropriate.

<h2>Installing a Firewall</h2>

<pre><code>sudo apt install ufw</code></pre>

<pre><code>clear</code></pre>

<pre><code>sudo ufw limit 22/tcp</code></pre>     # make sure to add this so you don't lock yourself out of your raspberry (presumably headless rpi if you followed this tutorial series)

<pre><code>sudo ufw allow 80</code></pre>

<pre><code>sudo ufw allow 443</code></pre>

<pre><code>sudo ufw enable</code></pre> -> (y)es - we added port 22 over tcp - so our ssh session wont be terminated.

<h2>Installing Docker</h2>

<pre><code>sudo mkdir -m 1777 ~/share</code></pre>

<pre><code>sudo apt install samba samba-common-bin</code></pre>

<pre><code>sudo nano /etc/samba/smb.conf</code></pre>

<pre><code>[share]
Comment = pi shared folder
Path = /home/pi/share
Browseable = yes
Writeable = yes
only guest = no
create mask = 0777
directory mask = 0777
Public = yes</code></pre>

<pre><code>sudo smbpasswd -a pi</code></pre>

<pre><code>sudo service smbd restart</code></pre>

<pre><code>mkdir ~/share/foundrydata</code></pre>

<pre><code>mkdir ~/docker</code></pre>

<pre><code>sudo nano ~/docker/docker-compose.yml</code></pre>

<pre>
  <code>
version: '3.9'

services:
  foundry:
     image: felddy/foundryvtt:release
     hostname: my_foundry_host      # change
     network_mode: host
     init: true
     restart: "unless-stopped"
     volumes:
       - type: bind
         source:     # change
         target: /data
     environment:
       - FOUNDRY_PASSWORD=          # change
       - FOUNDRY_USERNAME=          # change
       - FOUNDRY_ADMIN_KEY=         # change
     ports:
       - target: 30000
         published: 30000
         protocol: tcp
     </code>
</pre>

<pre><code>ls</code></pre>
<pre><code>cd docker</code></pre>
<pre><code>curl -sSL https://get.docker.com | sh</code></pre>
<pre><code>sudo usermod -aG docker ${USER}</code></pre>

<pre><code>man usermod</code></pre>

<pre><code>groups ${USER}</code></pre>

<pre><code>sudo apt-get install python3 python3-pip</code></pre>

<pre><code>sudo apt install docker-compose</code></pre>

<pre><code>sudo systemctl enable docker</code></pre>

<pre><code>docker compose up -d</code></pre>

<pre><code>sudo ufw allow 30000</code></pre>

<pre><code>sudo ufw allow samba</code></pre>

<pre><code>sudo ufw reload</code></pre>

<h1>Cloudflare Tunnel</h1>
<p>In order to use cloudflare tunnel you will need to purchase a domain name. You can buy one directly from Cloudflare or any other domain name provider of your choice. If you choose to use a domain name provider other than Cloudflare, you must switch the DNS records over from your provider to Cloudflare otherwise the Tunnel won't work.</p>
<p>A DNS switch over will generally take between two hours to two days to complete.</p>
<p>Login to Cloudflare and nagivate to Zero Trust.</p>

<img width="1440" alt="Screenshot 2025-03-24 at 15 10 43" src="https://github.com/user-attachments/assets/f9cea414-17c2-4ea1-969f-5005307c98dc" />

<P>Navigate to Network, Tunnel then Create a Tunnel. Then name the Tunnel "Foundry" or any name of your choice.</P>

<img width="1440" alt="Screenshot 2025-03-24 at 15 18 55" src="https://github.com/user-attachments/assets/e7f0d7cc-d72f-486d-96cf-720885d35362" />
<br></br>
<p>Proceed to the next screen, select Debian, then Arm-64.</p>
<br></br>
<img width="1440" alt="Screenshot 2025-03-24 at 15 11 25" src="https://github.com/user-attachments/assets/7aa5312a-2002-439e-9751-b07118f0bb11" />
<br></br>
<p>Copy and paste in the command underneath the heading 'if you don't have cloudflare install on your machine" into the terminal on your Mac or PC. Go back to your browser and press next to advance to the next screen.</p>
<br></br>
<img width="1440" alt="Screenshot 2025-03-24 at 15 11 56" src="https://github.com/user-attachments/assets/600bf564-b959-42b7-840c-d9d9e4be0b6e" />
<br></br>
<p>Enter the subdomain you wish your foundry instance to be called. Select your domain, then service type as HTTP and the URL the local IP Address of your Pi along with the port number of your Foundry Instance which is 30000. So in my example the IP Address would be http://192.168.4.101:30000</p>
<br></br>
<img width="1440" alt="Screenshot 2025-03-24 at 15 22 04" src="https://github.com/user-attachments/assets/c5028f29-8e51-411b-9760-379908c0ea94" />
<p>Now save your Tunnel. You should now be able to access Foundry VTT through your own unique URL.</p>







