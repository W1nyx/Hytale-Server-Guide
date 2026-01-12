# Hytale-Server-Guide
This is a Guide/Notes on the hytale server system and self hosting them.
I will try to keep this updated as they release new server patches and features
---
Hytale servers can run on any device with atleast 4GB of ram/memory and java 25. Both x64 and amr64 archtectures are supported.

More CPU ussages will be used for High player or entity counts while RAM will be used for large loadded world areas.

---
# Install java 25

Intall java 25. They recommend https://adoptium.net/temurin/releases

Verify the installation by running:
```bash
java --version
```
Expected output
```bash
openjdk 25.0.1 2025-10-21 LTS
OpenJDK Runtime Environment Temurin-25.0.1+8 (build 25.0.1+8-LTS)
OpenJDK 64-Bit Server VM Temurin-25.0.1+8 (build 25.0.1+8-LTS, mixed mode, sharing)
```
# Server Files
They have made two way to get the server files:
  1. Manually copy from your Launcher installation
  2. Use the Hytale Downloader CLI
# Manually Copy From Launcher
  Best for quick testing, not good for long run
Find the file in your launcher installation folder:
```bash
Windows: %appdata%\Hytale\install\release\package\game\latest
Linux: $XDG_DATA_HOME/Hytale/install/release/package/game/latest
MacOS: ~/Application Support/Hytale/install/release/package/game/latest
```
List the content of the direcoty:
```bash
ls
```
Expected output:
```bash
    Directory: C:\Users\...\Hytale\install\release\package\game\latest

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        12/25/2025   9:25 PM                Client
d-----        12/25/2025   9:25 PM                Server
-a----        12/25/2025   9:04 PM     3298097359 Assets.zip
```
Coppy the Server folder and Asseets.zip to your destination server folder.
# Hytale Downloader CLI
  Best for production server, Easy to keep updated

 A command-line tool to download hytale server and assets files with 0Auth2 authentication.
 
 See QUCIKSTART.md inside the archive.
 
 You can download it at : https://downloader.hytale.com/hytale-downloader.zip
# ARM64 Hyale Downloader CLI
  if you are trying to run it on a ARM64 system there are no CLI versions for it at this moment.
 One way I found to get around it us using an emulator called qemu-user

to install the emulator use
```bash
sudo apt install qemu-user-static
```
Then just use the command 
```bash
./hytale-downloader-linux-amd64
```
To download the latest release

 ---

 Command-Description
	

./hytale-downloader: 	Download latest release

./hytale-downloader -print-version: 	Show game version without downloading

./hytale-downloader -version: 	Show hytale-downloader version

./hytale-downloader -check-update: 	Check for hytale-downloader updates

./hytale-downloader -download-path game.zip: 	Download to specific file

./hytale-downloader -patchline pre-release: 	Download from pre-release channel

./hytale-downloader -skip-update-check: 	Skip automatic update check

# Running a Hytale Server
Start the server with:
```bash
java -jar HytaleServer.jar --assets PathToAssets.zip
```
# Authentication
After the first launch, authenticate your server
```bash
> /auth login device
===================================================================
DEVICE AUTHORIZATION
===================================================================
Visit: https://accounts.hytale.com/device
Enter code: ABCD-1234
Or visit: https://accounts.hytale.com/device?user_code=ABCD-1234
===================================================================
Waiting for authorization (expires in 900 seconds)...

[User completes authorization in browser]

> Authentication successful! Mode: OAUTH_DEVICE
```
Once Authticated, your server can accept player connections.

This so so players do not abuse there systems

# Help
Review all arguments with: 
```bash
java -jar HytaleServer.jar --help
```

# Ports
Default port is 5520. Change it with the --bind argument:
```bash
java -jar HytaleServer.jar --assets PathToAssets.zip --bind 0.0.0.0:25565
```
# Firewall & Network Config
Hytale uses UDP not TCP. COnfigure your firewall and port fowarding accordingly (Look up how to do port fowarding if confused)

# Port Fowarding
If hosting behind a router, forward UDP port 5520 (or your custom port) to your server machine. TCP forwarding is not required.

# Firewall Rules

Windows Defnder Firewall:
```bash
New-NetFirewallRule -DisplayName "Hytale Server" -Direction Inbound -Protocol UDP -LocalPort 5520 -Action Allow
```
Linux(iptables):
```bash
sudo iptables -A INPUT -p udp --dport 5520 -j ACCEPT
```
LInux (ufw):
```bash
sudo ufw allow 5520/udp
```

# NAT Considerations
QUIC handles NAT traversal well in most cases. If players have trouble connecting:

 Ensure the port forward is specifically for UDP, not TCP
 
  Symmetric NAT configurations may cause issues - consider a VPS or dedicated server
  
   Players behind carrier-grade NAT (common on mobile networks) should connect fine as clients
   

  
