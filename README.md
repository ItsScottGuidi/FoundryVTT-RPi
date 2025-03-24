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
<code>sudo apt install rpi-imager</code>
in a Terminal window.
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
Place the storage in your Pi and switch on your device. From another PC or Mac launch the Terminal app and login to the Pi using SSH, using the following command. <code>ssh piusername@ip-address-of-pi</code>
When prompted type in your password for the Pi and answer Yes to fingerprint.
<br>
</br>









