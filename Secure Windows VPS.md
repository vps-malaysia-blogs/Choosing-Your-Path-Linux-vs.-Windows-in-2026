# Step-by-Step: How to Securely Configure Your New Windows VPS

The moment you log into a brand-new [Windows Virtual Private Server (VPS)](https://www.vpsmalaysia.com.my/windows-vps-hosting/) for the first time, you are handed a blank slate. It’s fast, pristine, and completely unconfigured.

But here is the catch: because it has a public IP address and is permanently connected to the internet, automated bots and scanners will likely find it within hours. Leaving it in its default state is an open invitation for brute-force attacks.

Securing your server shouldn’t feel intimidating. Here is a straightforward, step-by-step checklist to lockdown and securely configure your new Windows VPS right out of the box.

<img width="1376" height="768" alt="Server_protected_by_holographic_…_202606261356" src="https://github.com/user-attachments/assets/9e7163c3-1da0-49b8-9f18-410092deaf29" />

## Step 1: Immediately Update Windows Server

Hosting providers deploy virtual servers using pre-made templates. Even if that template was updated last month, critical security patches might have been released since then.

1. Click the **Start Menu** and open **Settings** (the gear icon).
2. Navigate to **Update & Security** > **Windows Update**.
3. Click **Check for updates**.
4. Download and install all critical security patches and cumulative rollups.
5. **Restart the server** if prompted. Repeat this process until it says *"You’re up to date."*




## Step 2: Change the Default Administrator Password

Most providers give you a random password to log in initially, but some might generate a simple default one. Never risk keeping it.

1. Press `Win + R` on your keyboard, type `lusrmgr.msc`, and press **Enter** to open Local Users and Groups.
2. Click on the **Users** folder.
3. Right-click the **Administrator** account and select **Set Password**.
4. Create a password that is at least 16 characters long, combining uppercase letters, lowercase letters, numbers, and special symbols.

> 💡 **Pro-Tip:** Better yet, create a *completely new* user account with administrative privileges for daily management, and disable the default "Administrator" user entirely. Bots constantly target the specific username "Administrator" during brute-force attacks; changing the username breaks their automation instantly.

---

## Step 3: Change the Default Remote Desktop (RDP) Port

By default, the Remote Desktop Protocol (RDP) listens on **Port 3389**. Because this is standard across all Windows systems, hackers constantly scan the internet looking for servers with 3389 open. Changing this to a random custom port (e.g., between 49152 and 65535) drastically reduces the volume of random attack attempts.

1. Press `Win + R`, type `regedit`, and hit **Enter** to open the Registry Editor.
2. Navigate to the following path:
`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp`
3. Find the registry key named **PortNumber**.
4. Right-click it, select **Modify**, and change the Base to **Decimal**.
5. Type your chosen custom port number (e.g., `55231`) and click **OK**.

⚠️ **CRITICAL STEP:** Do not disconnect from your RDP session yet! If you close your connection before completing Step 4, the firewall will block your new port, and you will lock yourself out of your server.

---

## Step 4: Update Your Windows Defender Firewall

Now that you've told Windows to listen on a new port, you must tell the built-in firewall to allow traffic through that specific port.

1. Open the Start menu, type **Windows Defender Firewall with Advanced Security**, and open it.
2. Click on **Inbound Rules** on the left panel, then click **New Rule...** on the right panel.
3. Choose **Port** and click Next.
4. Select **TCP** and type your custom port number under *Specific local ports* (e.g., `55231`). Click Next.
5. Choose **Allow the connection** and click Next.
6. Keep Domain, Private, and Public checked. Click Next.
7. Name the rule something clear, like `Custom RDP Port`, and click **Finish**.

*Test your new configuration:* Open a new Remote Desktop window on your local PC and try logging in using `YourServerIP:CustomPort` (e.g., `192.168.1.100:55231`). If it connects, your configuration was successful, and you can close your old session.

---

## Step 5: Implement an Account Lockout Policy

If a malicious bot manages to find your new custom port, you want to prevent them from guessing passwords indefinitely. An Account Lockout Policy will temporarily freeze an account if someone inputs the wrong password too many times.

1. Press `Win + R`, type `secpol.msc`, and press **Enter** to open the Local Security Policy window.
2. Expand **Account Policies** on the left menu and click on **Account Lockout Policy**.
3. Double-click **Account lockout threshold**.
4. Change the value to **5 invalid logon attempts** (meaning the account locks after 5 wrong guesses).
5. Set the **Account lockout duration** to 15 or 30 minutes, giving you a reasonable window while deterring automated scripts.

---

## Step 6: Configure an Automated Backup Schedule

Security isn't just about blocking hackers; it’s also about data resilience. If a software error, bad update, or ransomware attack occurs, backups are your safety net.

* **Provider-Level Backups:** Check if your hosting provider offers automated daily snapshots. This is the easiest, safest method because snapshots capture the entire server state.
* **Windows Server Backup:** If you are self-managing on a budget, install the **Windows Server Backup** feature via the *Server Manager Dashboard*. Configure a daily schedule to back up critical application data folders to an isolated secondary storage volume or an offsite cloud bucket.

---

## Final Verification Checklist

Before deploying your live websites, Forex bots, or game applications, perform one quick sweep:

* [ ] Is Windows fully updated?
* [ ] Is the default password changed to a secure passphrase?
* [ ] Does RDP connect seamlessly via your new custom port?
* [ ] Is the Account Lockout Policy enabled?
* [ ] Is an automated backup system actively running?

Taking 15 minutes to run through these hardening steps ensures your Windows VPS remains an isolated, high-performing fortress for your digital projects.
