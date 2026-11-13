# 🔄 Fix Stuck Windows Update Prompts (Windows 10 & 11)

![Windows 10 | 11](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?logo=windows&logoColor=white)
![Troubleshooting guide](https://img.shields.io/badge/Type-troubleshooting-6f42c1)
[![Last updated](https://img.shields.io/github/last-commit/ImmuneMoon/Fix-Stuck-Windows-Update-Prompts?label=updated)](https://github.com/ImmuneMoon/Fix-Stuck-Windows-Update-Prompts/commits/main)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey)](LICENSE)

**📄 [Download the PDF](Fix-Stuck-Windows-Update-Prompts.pdf)**

## 📖 Overview
The **Fix Stuck Windows Update Prompts** guide is for the moment Windows Update takes over your screen and won't let go: a restart reminder with buttons that do nothing, a full-screen "finish setting up" page, a Windows 11 upgrade offer, a restart that sits on "Working on updates" for ages, or a pop-up that only looks like Windows Update.

It starts with the safe way out (no holding the power button), then helps you work out which prompt you have, install the waiting update properly, stop surprise restart times, and fix updates that keep failing.

## ⚠️ Important Disclaimer
Never turn off or unplug the PC while it shows **Working on updates**, **Installing**, or **Undoing changes**. Windows is changing system files at that point. The reset commands in **Phase 6** need an administrator account, and they only rename Windows Update's cache folders. Nothing in this guide edits the registry or turns Windows Update off. If a "Windows update" pop-up shows a phone number, it's a scam: don't call it.

## 🚀 How to Use
Download the PDF above, or read on below. If a prompt is stuck on your screen right now, do **Try This First**. Then use **Step 1** to identify the prompt and jump to the phase it points to.

This guide works alongside the [DISM, SFC, and CHKDSK Windows Repair Guide](https://github.com/ImmuneMoon/DISM-SFC-CHKDSK-Windows-Repair-Guide), the [Windows Malware Removal Guide](https://github.com/ImmuneMoon/Windows-Malware-Removal-Guide), and the [Windows Install Guide](https://github.com/ImmuneMoon/Windows-Install-Guide). For a different stuck-screen annoyance, see the [Stuck Taskbar Notification Badge Fix](https://github.com/ImmuneMoon/Stuck-Taskbar-Notification-Badge-Fix).

Learning IT support? Fixing Windows Update problems is everyday help desk work and good practice for CompTIA A+. See my [IT Support Careers and Certifications guide](https://github.com/ImmuneMoon/IT-Support-Careers-and-Certifications#112-practice-with-this-sites-guides).

---

<!-- guide:start -->

## The Problem
A Windows Update prompt fills the screen and you can't get rid of it. The most common version is the restart reminder:

> We've got an update for you. Windows is continuously improving, and updates are a normal part of keeping it running smoothly. We'll restart and install this update at 8:30 AM. Or, you can restart now if you're ready.

Normally it has buttons like **Restart now** and **Pick a time**. When it glitches, the buttons draw blank, clicking them does nothing, there's no X, and the window stays on top of everything else, including Task Manager.

> **Real example:** A Windows 11 PC showed the reminder above with three blank buttons (two gray, one green), no labels and no close button. None of the buttons responded, and it stayed in front of every window, even Task Manager. `Ctrl + Alt + Del` opened the security screen, but switching back put the dialog on top again. The owner held the power button to turn the PC off. The update itself was fine: it had already downloaded and was waiting for a restart.

## How It Actually Works
Windows Update works in stages:

1. **Download and prepare.** This happens in the background while you use the PC.
2. **Wait for a restart.** The update is staged and ready. Windows picks a restart time outside your **active hours** (8 AM to 5 PM by default) and shows reminders.
3. **Install during the restart.** This is the **Working on updates** / **Don't turn off your PC** screen. Only this stage is risky to interrupt.

The reminder window is a small separate program started by the **Update Orchestrator**, the Windows service that manages updates:

| Windows version | Reminder program | Folder |
|---|---|---|
| Newer Windows 11 builds | `MoNotificationUx.exe` | A folder under `C:\Windows\UUS` |
| Older Windows 10 and 11 builds | `MusNotificationUx.exe` or `MusNotification.exe` | `C:\Windows\System32` |

That program only **shows** the reminder. It doesn't install anything. So when the reminder freezes, closing it (or doing a clean restart) is safe: the update stays staged and installs on the next proper restart. In the real example above, powering off during the reminder didn't interrupt an install, because the install hadn't started.

## Try This First: Get Out Safely
Use this when a prompt is stuck on screen and its buttons don't work. It's a clean shutdown, so it's safe for Windows and for the waiting update.

1. Save anything you can. Then press `Ctrl + Alt + Del`. This switches to a separate security screen the stuck window can't cover.
2. Don't choose Task Manager. Instead, click the **power icon** in the bottom-right corner of that screen.
3. Choose one:
   * **Update and restart** installs the waiting update now. This is usually the best pick.
   * **Restart** or **Shut down** if you don't want to install it right now.
4. If Windows warns that apps are still open, choose to restart or shut down anyway.

Windows closes your apps and shuts down normally. If you picked **Update and restart**, you'll see **Working on updates** for a while: leave it alone until it finishes.

If the prompt isn't the restart reminder (for example, it's in a web browser), go to **Step 1** first.

## Step 1: Find Out Which One You Have
| What you see | What it is | Go to |
|---|---|---|
| "We've got an update for you", "Restart required", or a restart scheduled at a set time, and the buttons are blank or do nothing | **(a)** Windows Update's restart reminder, glitched | **Phase 1**, then **Phase 2** |
| A full-screen "Let's finish setting up your device" or "welcome" page after an update, offering Microsoft services | **(b)** The Windows welcome experience (sometimes called SCOOBE) | **Phase 4** |
| On Windows 10: a full-screen offer to upgrade to Windows 11, or a warning that Windows 10 support has ended | **(c)** The Windows 11 upgrade offer or an end-of-support notice | **Phase 4** |
| A blue or black screen saying **Working on updates**, **Don't turn off your PC**, or **Undoing changes** during a restart | **(d)** Windows installing or rolling back an update | **Phase 7** |
| An "update" warning inside a web browser: a browser tab, a full-screen browser, a phone number, or a button to download an "update" | **(e)** A tech support scam. Not Windows Update. | **Phase 5** |
| Windows keeps asking to restart or install the same update, and **Update history** shows it failed | **(f)** An update that keeps failing | **Phase 6** |

> **How to tell real from fake:** Real Windows Update prompts come from Windows itself and point you to **Settings** > **Windows Update**. They never show a phone number, never ask you to download a file, and never run inside a browser tab.

## Phase 1: Close a Frozen Restart Reminder
Work down this list. Stop at the first one that works.

1. **The clean restart.** `Ctrl + Alt + Del` > power icon > **Update and restart** (see **Try This First**). This is the most reliable option, because the security screen can't be covered by the reminder.
2. **Alt + F4.** Click on the reminder once, then press `Alt + F4` to close the active window. A glitched reminder often ignores this, but it's worth a try. `Windows + D` (show desktop) and `Windows + Tab` (Task View) usually don't help, because the reminder is set to stay on top.
3. **End the reminder in Task Manager.**
   1. Press `Ctrl + Shift + Esc` (or `Ctrl + Alt + Del` > **Task Manager**).
   2. If the reminder covers Task Manager, make Task Manager stay on top first:
      * **Windows 11:** In Task Manager, click **Settings** (the gear, bottom left). Under **Window management**, turn on **Always on top**.
      * **Windows 10:** Click **Options** > **Always on top**.
   3. Open the **Details** tab.
      * **Windows 11:** Type `notification` in the search box at the top.
      * **Windows 10:** Click the **Name** column to sort, and scroll to the M's.
   4. Find `MoNotificationUx.exe` or `MusNotificationUx.exe` (or `MusNotification.exe`). Right-click it and choose **Open file location** to make sure it's in `C:\Windows\UUS\...` or `C:\Windows\System32`. A copy anywhere else isn't the real one: see the [Windows Malware Removal Guide](https://github.com/ImmuneMoon/Windows-Malware-Removal-Guide#1-is-it-really-malware).
   5. Right-click it again and choose **End task**.

   Ending it only closes the reminder window. The update stays downloaded and waiting, and Windows will remind you again later. Go to **Phase 2** to install it properly.
4. **From a terminal**, if the reminder doesn't steal focus from it. In PowerShell, this ends the reminder:

   ```powershell
   Get-Process *NotificationUx* | Stop-Process
   ```

   Or restart the PC cleanly right away (save your work first):

   ```cmd
   shutdown /r /t 0
   ```

   `/r` restarts and `/t 0` means no countdown. After it restarts, check **Settings** > **Windows Update**. If it still says a restart is required, use **Update and restart** from the Start menu's **Power** button to install the update. For more everyday commands, see the [CLI Cheat Sheet](https://github.com/ImmuneMoon/CLI-Cheat-Sheet#what-is-covered).

### When holding the power button is OK
* **The reminder only:** Holding the power button is a last resort when nothing above works. Nothing is installing yet, so the update survives. You do lose unsaved work, and a hard power-off is never good for open files, so use `Ctrl + Alt + Del` first.
* **Never during Working on updates, Installing, or Undoing changes:** Windows is replacing system files. Cutting power here usually makes Windows roll the update back on the next start (**Undoing changes**), and in a bad case Windows won't start. If that happened, see **Phase 7**.

## Phase 2: Install the Waiting Update Properly
1. Save your work.
2. Select **Start** > **Power** > **Update and restart**.
3. Leave the PC alone during **Working on updates**. Keep a laptop plugged in. It may restart more than once.
4. When you're back at the desktop, check the result:
   * **Windows 11:** **Settings** > **Windows Update** > **Update history**.
   * **Windows 10:** **Settings** > **Update & Security** > **Windows Update** > **View update history**.
5. The update should show as successfully installed. If it says **Failed to install** with an error code, go to **Phase 6**.
6. Go back to **Windows Update** and click **Check for updates** once more, in case something else is waiting.

## Phase 3: Stop Surprise Restarts and Reminders
These settings decide when Windows restarts and how much it warns you. None of them turn updates off.

### Active hours
Windows won't restart automatically for updates during your active hours.

* **Windows 11:** **Settings** > **Windows Update** > **Advanced options** > **Active hours**. Choose **Automatically** to let Windows learn when you use the PC, or **Manually** to set a start and end time (up to 18 hours).
* **Windows 10:** **Settings** > **Update & Security** > **Windows Update** > **Change active hours**. Turn **Automatically adjust active hours for this device based on activity** on, or set your own hours.

If the reminder picks a restart time when you're usually at the PC (like 8:30 AM), set active hours manually so they cover that time.

### Pick the restart time yourself
When an update is waiting, go to **Settings** > **Windows Update** (Windows 10: **Settings** > **Update & Security** > **Windows Update**) and choose **Schedule the restart**. Use **Pick a time** and **Pick a day** to choose a time that suits you.

### Windows 11's restart options (Advanced options)
In **Settings** > **Windows Update** > **Advanced options**:

* **Get me up to date:** "Restart as soon as possible (even during active hours) to finish updating, and notify me 15 minutes before restarting." Turn this **off** if you don't want restarts during active hours.
* **Notify me when a restart is required to finish updating:**
  * **On:** Windows shows a notification when an update needs a restart, so the restart doesn't come as a surprise.
  * **Off:** You get fewer heads-ups, but the restart still happens, outside your active hours.
  * Leaving it on is usually better.

Windows 10 has a similar restart notification switch in **Advanced options**.

### Get the latest updates as soon as they're available
This switch is on the main **Windows Update** page. When it's on, you get new non-security features and fixes early, and Microsoft says there may be more restarts each month. Turning it **off** doesn't affect security updates, which you still get as usual.

### Pause updates
Pausing buys time, for example before a trip or a deadline.

* **Windows 11:** **Settings** > **Windows Update** > **Pause updates**. You can pause a week at a time, up to 35 days.
* **Windows 10:** **Settings** > **Update & Security** > **Windows Update** > **Pause updates for 7 days**, or pick a date under **Advanced options**.

When the pause ends, you have to install the latest updates before you can pause again.

### Windows Pro: Group Policy (optional)
On Pro, Enterprise and Education editions (not Home), the Local Group Policy Editor (`gpedit.msc`) has restart policies under **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows Update**. On Windows 11 they may be in a subfolder such as **Manage end user experience**.

* **Turn off auto-restart for updates during active hours** sets active hours by policy, so automatic restarts happen outside them.
* **Display options for update notifications** can turn off most update notifications while keeping restart warnings. Its other choice also hides restart warnings, which brings back surprise restarts, so avoid that one.

> **Caution:** Policies override the Settings app, and conflicting restart policies can behave in ways you don't expect. Change one at a time, and write down what you changed. On a work or school PC, leave these to your IT department.

### Don't turn updates off
Avoid registry hacks, scripts or tools that disable Windows Update. Updates carry the security fixes that keep the PC safe online, and a PC that skips them for months has a large, slow, failure-prone catch-up later. Microsoft's own description of the Windows Update service says that if it's disabled, you can't use Windows Update or its automatic updating. Use active hours, scheduling and pausing instead.

## Phase 4: "Finish Setting Up" Screens and the Windows 11 Upgrade Offer

### The "Let's finish setting up your device" screen (prompt b)
This full-screen page appears after some updates and promotes things like Microsoft account features, backup and Microsoft 365. It's safe to skip. On the screen, use its skip or remind-me-later option rather than continuing, if you don't want the offers. To stop it coming back:

* **Windows 11:** **Settings** > **System** > **Notifications** > **Additional settings**. Clear these three boxes:
  * **Show the Windows welcome experience after updates and when signed in to show what's new and suggested**
  * **Suggest ways to get the most out of Windows and finish setting up this device**
  * **Get tips and suggestions when using Windows**
* **Windows 10:** **Settings** > **System** > **Notifications & actions**. Clear the boxes for the Windows welcome experience, for suggesting ways to finish setting up your device, and for tips and suggestions.

### The Windows 11 upgrade offer and end-of-support notices (prompt c)
On Windows 10, Microsoft shows full-screen offers to upgrade to Windows 11, and notices that Windows 10 support has ended.

* **Support for Windows 10 ended on October 14, 2025.** It no longer gets regular security updates.
* **Consumer Extended Security Updates (ESU)** keep security fixes coming until **October 12, 2027**. Enrollment is free if you sync your PC settings, or costs 1,000 Microsoft Rewards points or $30. Enroll from **Settings** > **Update & Security** > **Windows Update** > **Enroll now**.
* **To decline the upgrade for now,** choose the option on the offer that keeps Windows 10 instead of the upgrade button. The wording changes between versions of the offer, and the offer can come back.
* **If your PC can run Windows 11,** upgrading is free. See [If Your PC Qualifies](https://github.com/ImmuneMoon/Windows-Install-Guide#if-your-pc-qualifies) in the Windows Install Guide.
* **If you're staying on Windows 10,** read the [Windows 10 Community Support Guide](https://github.com/ImmuneMoon/Windows-10-Community-Support-Guide#where-windows-10-stands-right-now) for your options and how to stay safe.

## Phase 5: Fake "Update" Pop-Ups in a Browser (prompt e)
A web page can go full screen and pretend to be Windows Update, an error screen, or a virus warning. Tell-tale signs:

* It's in a **browser** (you can see a tab or address bar, or it appeared while you were browsing).
* It shows a **phone number** to call for "support".
* It wants you to **download an update** or a "fix" file.
* It plays alarms or a recorded voice, or says you have minutes before something bad happens.

Microsoft says its real error messages never include phone numbers, and it won't call you to offer tech support unless you contacted it first.

To get out:

1. **Don't call the number, and don't download anything.**
2. Press `Esc` or `F11` to leave full screen, then close the tab.
3. If that doesn't work, press `Alt + F4` to close the browser.
4. If the page still won't let go, press `Ctrl + Shift + Esc`, select the browser on the **Processes** tab, and choose **End task**.
5. When you reopen the browser, don't restore the last session's tabs.

**If you called, let someone connect to your PC, or ran a file they gave you,** treat it as an infection. Disconnect from the internet and follow the [Windows Malware Removal Guide](https://github.com/ImmuneMoon/Windows-Malware-Removal-Guide#2-before-you-clean-anything), then [secure your accounts](https://github.com/ImmuneMoon/Windows-Malware-Removal-Guide#7-secure-your-accounts). You can report the scam at microsoft.com/reportascam.

## Phase 6: An Update That Keeps Failing (prompt f)
Work through these in order. Retry **Check for updates** after each one.

1. **Read the error code.** In **Update history**, a failed update shows **Failed to install on** a date, with a code that usually starts with `0x8`. Note the code and the update's KB number, and search them on support.microsoft.com.
2. **Free up space.** Microsoft recommends at least **16 GB free for a 32-bit system, or 20 GB for 64-bit**, to upgrade Windows. Delete files you don't need, uninstall apps you don't use, or move files to an external drive or OneDrive.
3. **Run the Windows Update troubleshooter.**
   * **Windows 11:** Microsoft points you to the troubleshooter in the **Get Help** app. You can also go to **Settings** > **System** > **Troubleshoot** > **Other troubleshooters**, find **Windows Update**, and select **Run**.
   * **Windows 10:** **Settings** > **Update & Security** > **Troubleshoot** > **Additional troubleshooters** > **Windows Update** > **Run the troubleshooter**.
4. **Repair system files with DISM and SFC.** In an administrator Command Prompt, run `DISM.exe /Online /Cleanup-image /Restorehealth`, then `sfc /scannow`. The [DISM, SFC, and CHKDSK Windows Repair Guide](https://github.com/ImmuneMoon/DISM-SFC-CHKDSK-Windows-Repair-Guide) walks through it.
5. **Reset the Windows Update components.** This clears Windows Update's download cache and signature catalog so they're rebuilt. You need an administrator account.
   1. Select **Start**, type `cmd`, right-click **Command Prompt**, and choose **Run as administrator**.
   2. Run these one at a time, letting each finish:

      ```cmd
      net stop bits
      net stop wuauserv
      net stop cryptsvc
      ren %systemroot%\SoftwareDistribution SoftwareDistribution.bak
      ren %systemroot%\System32\catroot2 catroot2.bak
      net start cryptsvc
      net start wuauserv
      net start bits
      ```

   3. Restart the PC, then check for updates.
   * If a `ren` line says **Access is denied**, run its `net stop` line again and retry the rename.
   * If a `.bak` folder from an earlier reset is in the way, delete the old one first.
   * Once updates install normally, you can delete the `.bak` folders.
6. **Uninstall a bad update.** If problems started right after an update installed:
   * **Windows 11:** **Settings** > **Windows Update** > **Update history** > **Uninstall updates**.
   * **Windows 10:** **Settings** > **Update & Security** > **Windows Update** > **View update history** > **Uninstall updates**.
   * Select the update and choose **Uninstall**. Not every update can be removed, and Microsoft advises removing one only when you need to. Pause updates afterward (see **Phase 3**) so it doesn't reinstall straight away.
7. **Install it by hand from the Microsoft Update Catalog.** Go to [catalog.update.microsoft.com](https://www.catalog.update.microsoft.com), search for the KB number from **Update history**, and download the file that matches your Windows version (shown in **Settings** > **System** > **About**) and system type (x64 or ARM64). Double-click the downloaded file to install it.
8. **Reinstall Windows without losing anything (Windows 11).** Go to **Settings** > **System** > **Recovery**, and under **Fix problems using Windows Update**, select **Reinstall now**. It reinstalls your current version of Windows and keeps your apps, files and settings. Keep the PC plugged in and online. It needs Windows 11 version 22H2 or later with recent updates, and it isn't available on work or school PCs managed by an organization.
9. **Last resort: an in-place repair install.** Make Windows installation media (see [Using the Media Creation Tool](https://github.com/ImmuneMoon/Windows-Install-Guide#using-the-media-creation-tool) in the Windows Install Guide). With Windows running, open the media in File Explorer and run `setup.exe`. Select **Change what to keep** > **Keep personal files and apps**, then **Install**. Use the same edition you have now (Home or Pro), and back up your files first.

## Phase 7: Stuck on "Working on Updates" or "Undoing Changes" (prompt d)
These screens mean Windows is installing an update, or removing one that failed. How long it takes depends on the size of the update and the speed of the drive, and a big update on an older hard drive can take a long time.

1. **Wait.** Keep a laptop plugged in. A percentage that changes now and then, or a flickering drive light, means it's still working. Give it hours, ideally overnight, before you decide it's stuck.
2. **"Undoing changes" is normal after a failed install.** Let it finish. When Windows starts, go to **Phase 6** to find out why the update failed.
3. **A forced restart is the last resort.** Only if the screen hasn't changed at all for many hours, hold the power button until the PC turns off, then turn it on again. Windows may roll the update back, which is fine.

### If Windows won't start afterward
Windows opens the **Windows Recovery Environment** (WinRE) by itself after two failed attempts to start. If it doesn't, turn the PC on and hold the power button to interrupt startup before Windows loads. After two interruptions, Windows should open WinRE on the third start.

From WinRE's blue **Choose an option** screen:

1. **Troubleshoot** > **Advanced options** > **Startup Repair** fixes common problems that stop Windows from starting.
2. If that doesn't work, **Troubleshoot** > **Advanced options** > **Uninstall Updates** removes the update that was just installed. Your files and apps aren't affected.
3. **Troubleshoot** > **Advanced options** > **System Restore** is another option, if you have restore points.

If the drive is encrypted with BitLocker, these tools may ask for your recovery key. If you sign in with a Microsoft account, you can find it at account.microsoft.com/devices/recoverykey from another device.

Once Windows starts again, pause updates (see **Phase 3**) and work through **Phase 6** before trying the update again.

## Fixes That Don't Help
* **Disabling the Windows Update service.** It doesn't fix a stuck prompt. It stops security updates, and turning it back on later leaves a large backlog that's more likely to fail. Use active hours, scheduling and pausing instead.
* **Deleting the reminder program or other system files.** Removing `MoNotificationUx.exe`, `MusNotificationUx.exe`, or folders in `C:\Windows` can break Windows Update and other parts of Windows, and repairs or updates may put them back. If the reminder freezes, end it in Task Manager (**Phase 1**) instead.
* **"Debloat" scripts.** Many turn off update services, remove components, or set update policies. The result is updates that fail with errors, and **Fix problems using Windows Update** isn't available when certain Windows Update policies are set. If you've run one and updates now fail, a repair install (**Phase 6**, step 9) is often the cleanest fix.
* **Third-party "update blocker" tools.** They work by disabling services or forcing policies, with the same problems as above. Updates pile up, and you go without security fixes until you remember to undo it.

## Quick Checklist
- [ ] Prompt stuck on screen → `Ctrl + Alt + Del` > power icon > **Update and restart** (**Try This First**)
- [ ] Identified the prompt with the table in **Step 1**
- [ ] Frozen reminder → ended `MoNotificationUx.exe` or `MusNotificationUx.exe` in Task Manager (**Phase 1**)
- [ ] Installed the waiting update with **Start** > **Power** > **Update and restart**, then checked **Update history** (**Phase 2**)
- [ ] Set active hours, turned off **Get me up to date** if needed, and scheduled restarts myself (**Phase 3**)
- [ ] Cleared the welcome experience and "finish setting up" boxes in **Additional settings** (**Phase 4**)
- [ ] Browser pop-up with a phone number → closed it, didn't call (**Phase 5**)
- [ ] Failing update → error code, free space, troubleshooter, DISM and SFC, component reset (**Phase 6**)
- [ ] Never held the power button during **Working on updates** or **Undoing changes** (**Phase 7**)

## Sources
* Microsoft Support: [Windows Update: FAQ](https://support.microsoft.com/windows/windows-update-faq-8a903416-6f45-0718-f5c7-375e92dddeb2)
* Microsoft Support: [Manage updates in Windows](https://support.microsoft.com/en-us/windows/manage-updates-in-windows-643e9ea7-3cf6-7da6-a25c-95d4f7f099fe)
* Microsoft Support: [Keep your PC up to date with active hours](https://support.microsoft.com/en-us/help/17154)
* Microsoft Support: [Get Windows updates as soon as they're available for your device](https://support.microsoft.com/help/5026679)
* Microsoft Learn: [Manage device restarts after updates](https://learn.microsoft.com/en-us/windows/deployment/update/waas-restart)
* Microsoft Support: [Notifications and Do Not Disturb in Windows](https://support.microsoft.com/en-us/windows/notifications-and-do-not-disturb-in-windows-feeca47f-0baf-5680-16f0-8801db1a8466)
* Microsoft Support: [Keyboard shortcuts in Windows](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)
* Microsoft Learn: [shutdown](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/shutdown)
* Microsoft Support: [Troubleshoot problems updating Windows](https://support.microsoft.com/en-us/windows/resolve-problems-updating-windows-188c2b0f-10a7-d72f-65b8-32d177eb136c)
* Microsoft Learn: [Additional resources for Windows Update](https://learn.microsoft.com/en-us/troubleshoot/windows-client/installing-updates-features-roles/additional-resources-for-windows-update) (resetting Windows Update components)
* Microsoft: [Microsoft Update Catalog](https://www.catalog.update.microsoft.com)
* Microsoft Support: [Fix issues by reinstalling the current version of Windows](https://support.microsoft.com/en-us/topic/497ac6da-7cac-4641-82a5-f50398d879a0)
* Microsoft Support: [Reinstall Windows with the installation media](https://support.microsoft.com/en-us/windows/reinstall-windows-d8369486-3e33-7d9c-dccc-859e2b022fc7)
* Microsoft Support: [Recovery options in Windows](https://support.microsoft.com/en-us/windows/recovery-options-in-windows-31ce2444-7de3-818c-d626-e3b5a3024da5)
* Microsoft Support: [Windows Recovery Environment](https://support.microsoft.com/en-us/windows/windows-recovery-environment-0eb14733-6301-41cb-8d26-06a12b42770b)
* Microsoft Learn: [Windows Recovery Environment (Windows RE)](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
* Microsoft Support: [Startup Repair](https://support.microsoft.com/topic/85deb0b9-fa3d-44a3-a3d0-d0f1515c2c9b)
* Microsoft Support: [Protect yourself from tech support scams](https://support.microsoft.com/en-us/windows/protect-yourself-from-tech-support-scams-2ebf91bd-f94c-2a8a-e541-f5c800d18435)
* Microsoft: [Windows 10 Extended Security Updates](https://www.microsoft.com/en-us/windows/extended-security-updates)

<!-- guide:end -->

## ☕ Support the Project

If you find this project helpful and want to support further development by Fulllion Creative Works, consider leaving a tip!

* [Donate via PayPal](https://www.paypal.com/donate/?hosted_button_id=LCDZX75HR4CLC)
* [Support on Ko-fi](https://ko-fi.com/fulllion)

---
© 2026 Fulllion Creative Works. All rights reserved.
