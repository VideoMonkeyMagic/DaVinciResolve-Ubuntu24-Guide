# DaVinciResolve-Ubuntu24-Guide
📌 README.md – DaVinci Resolve Ubuntu 24.04 Installation Guide
md
Copy
Edit
# 🎬 DaVinci Resolve Ubuntu 24.04 Installation Guide 🚀  

This guide provides **step-by-step instructions** for installing **DaVinci Resolve Studio** on **Ubuntu 24.04**, including fixes for common issues like dependency errors, missing libraries, and broken installations.

---

## **🔹 Step 1: Download DaVinci Resolve Studio**
1. Go to the **[Blackmagic Design website](https://www.blackmagicdesign.com/products/davinciresolve/)**  
2. Download the **Linux version** of DaVinci Resolve Studio.  
3. Move the `.zip` file to your `Downloads` folder and extract it:  
   ```bash
   cd ~/Downloads
   unzip DaVinci_Resolve_Studio_*.zip
You should now have the installer file:
bash
Copy
Edit
DaVinci_Resolve_Studio_19.1.3_Linux.run
🔹 Step 2: Make the Installer Executable
Before running the installer, make sure it’s executable:

bash
Copy
Edit
chmod +x DaVinci_Resolve_Studio_*.run
🔹 Step 3: Install Required Dependencies
DaVinci Resolve requires some older system libraries that Ubuntu 24.04 doesn’t include by default.

1️⃣ Update system package lists:

bash
Copy
Edit
sudo apt update
2️⃣ Install essential dependencies:

bash
Copy
Edit
sudo apt install -y libapr1 libaprutil1 libglib2.0-0 libc6 libgomp1 ocl-icd-libopencl1
3️⃣ If libasound2 is missing, install the t64 version manually:

bash
Copy
Edit
sudo apt install libasound2t64
🔹 Step 4: Run the Installer (Bypassing Package Check)
Ubuntu 24.04 has newer system libraries that break Resolve’s package check, so we force the installation:

bash
Copy
Edit
sudo env SKIP_PACKAGE_CHECK=1 ./DaVinci_Resolve_Studio_*.run
⏳ The installer may get stuck at 99% (Installing Application Icons).
If this happens, open a new terminal and run:

bash
Copy
Edit
sudo killall update-desktop-database
🔹 Step 5: Fix libpango and glib Issues
If DaVinci Resolve fails to launch with this error:

typescript
Copy
Edit
symbol lookup error: /lib/x86_64-linux-gnu/libpango-1.0.so.0: undefined symbol: g_once_init_leave_pointer
Run the following commands to move conflicting system libraries:

bash
Copy
Edit
cd /opt/resolve/libs
sudo mkdir disabled-libraries
sudo mv libglib* disabled-libraries/
sudo mv libgio* disabled-libraries/
sudo mv libgmodule* disabled-libraries/
Then try launching Resolve:

bash
Copy
Edit
/opt/resolve/bin/resolve
🔹 Step 6: Create a System-Wide Shortcut
If DaVinci Resolve launches correctly, create a system-wide shortcut so you can run it from anywhere:

bash
Copy
Edit
sudo ln -s /opt/resolve/bin/resolve /usr/bin/davinci-resolve
Now, you can start Resolve using:

bash
Copy
Edit
davinci-resolve
🔹 Step 7: Fix the Application Icon
By default, Resolve might show a generic cog icon instead of its actual logo. To fix this:

1️⃣ Edit the Resolve .desktop file:

bash
Copy
Edit
sudo nano /usr/share/applications/com.blackmagicdesign.resolve.desktop
2️⃣ Find the line starting with Icon=
If it's incorrect, change it to:

ini
Copy
Edit
Icon=/opt/resolve/graphics/DV_Resolve.png
3️⃣ Save the file (CTRL + X, then Y, then Enter).

4️⃣ Update the application menu:

bash
Copy
Edit
sudo update-desktop-database /usr/share/applications/
5️⃣ Restart your desktop session or reboot:

bash
Copy
Edit
reboot
🔹 Step 8: Verify the Installation
After completing all steps, try running:

bash
Copy
Edit
davinci-resolve
✅ If it opens without errors, you’re all set! 🎬🚀

🎯 Troubleshooting
🔴 If DaVinci Resolve still doesn’t launch, check for missing libraries:

bash
Copy
Edit
ldd /opt/resolve/bin/resolve | grep "not found"
This will show any missing dependencies. Install them manually using:

bash
Copy
Edit
sudo apt install <missing-package-name>
💡 Contributing
If you find new fixes or improvements, feel free to open an issue or pull request on this repo!
