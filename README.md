<p align="center">
  <img src="res/banner.png?v=2" alt="SSDWatch: see what your SSDs and your programs write, and know when to worry">
</p>

# SSDWatch

**SSDWatch shows how much your SSD receives every day, which programs wrote it, and how worn and healthy the drive is.** It sits in the tray, keeps a day-by-day history, and warns you when a program writes too fast, writes too much in a day, or when a whole day is twice your usual one. It is a small native Windows program: no account, no telemetry, nothing sent anywhere.

<!-- DRAFT: this story is a first draft written for you to correct or rewrite in your own words. -->
## The story behind SSDWatch

SSDs wear out as they are written, and I never knew how much mine was being written, or by whom. Windows shows that a disk is busy, but not what it received over a day, which program was behind it, or whether today is normal. The tools that tell you the health of a drive show one number; the ones that show per-program activity forget everything when you close them.

I wanted one small program that answers the simple questions: how much did my SSD get today, who wrote it, is that a normal day, and is the drive still healthy? SSDWatch is that program. Like Vizor, it was built with AI, but every design decision and every detail was mine, and each change was tried in the real program before it was saved.

## Screenshots

<p align="center"><img src="res/readme/overview.png" alt="Overview: the drive's odometer, wear and health, what is being written now, who wrote today and the last 14 days" width="80%"></p>

<table>
<tr>
<td width="50%"><img src="res/readme/programs.png" alt="Programs: every program that wrote today, in 7 or in 30 days"></td>
<td width="50%"><img src="res/readme/history.png" alt="History: 14, 30 or 90 days, the usual day and the programs of any day"></td>
</tr>
<tr>
<td><img src="res/readme/settings.png" alt="Settings: which alerts you get, which drives are watched, how it measures"></td>
<td valign="top">
<img src="res/readme/alert_busy_day.png" alt="An alert: your SSD is having a busy day, twice the usual one"><br><br>
<img src="res/readme/flyout.png" alt="The tray flyout: health, writing now and what was written today" width="60%">
</td>
</tr>
</table>

## Features

- **Who wrote today.** Every program, with how much it wrote today, in the last 7 days or in 30 days, and a 30-day chart for each one.
- **Real disk writes.** With administrator rights, SSDWatch counts what really reaches the drive, including what Windows writes later from its cache, and gives it to the program that wrote the file. Without them, it estimates from the programs' own counters.
- **Odometer and written today.** The total the drive has received in its life, and what it received today, against your usual day (the median of the last 30 days).
- **Health and wear.** For NVMe drives: percentage used, spare blocks, temperature, hours on and critical warnings, read from the drive itself.
- **History.** One small file per day, kept 30, 90 or 365 days. Charts of 14, 30 or 90 days, the busiest day, and the programs of any day. Export to CSV.
- **Alerts you choose:**
  - a program writing faster than a limit (10 to 1000 MB/s) for a while (30 s to 30 min);
  - a program writing more than an amount in one day (10 to 500 GB);
  - a day that doubles your usual one.
  An alert can be closed, or a program quieted until tomorrow.
- **Several SSDs.** Every NVMe and SATA SSD is found and can be watched or left out. Pick one drive or all of them in every view. Hard disks are left out.
- **Tray.** It keeps watching from the tray. The icon turns orange during an alert; a click shows the health, what is being written now and what was written today.
- **Starts with Windows,** with administrator rights and without asking each time, through a scheduled task that you turn on once in Settings.
- **Light and dark,** following Windows or chosen by you. Tips on every control, which you can turn off.

## Download and install

Download `SSDWatch-0.2.0-Setup.exe` from the [Releases](https://github.com/edukode-lab/SSDWatch/releases) page and run it. It asks for administrator rights once, installs SSDWatch in `Program Files`, adds it to the Start menu and, if you leave the box checked, sets it to start with Windows. Updating is the same: run the new installer over the old one. Your history and settings are kept.

To remove it, use **Installed apps** in Windows. The uninstaller also removes the "start with Windows" task, and asks whether to delete your history and settings too.

**Check your download.** Every release lists the **SHA-256** of the installer. After downloading, compare it with the one on the Releases page. In PowerShell:

```powershell
Get-FileHash .\SSDWatch-0.2.0-Setup.exe -Algorithm SHA256
```

If the two values are not identical, do not run the program. Delete it and download it again from this page.

> [!WARNING]
> **Download SSDWatch only from this repository or from well-known, trusted sites.** Unofficial copies, mirrors and files shared by strangers can hide malware. If you are not sure where a file came from, do not run it.

> [!NOTE]
> **Windows SmartScreen or your antivirus may warn you.** SSDWatch is a new program from an independent developer and is not yet signed with a code-signing certificate. It also reads disk activity and, if you ask it to, creates a task that starts it with Windows, which some antivirus programs treat with suspicion. If you downloaded it from this page, choose **More info** and then **Run anyway**.

**Administrator rights.** SSDWatch works without them, estimating what each program writes. To count the real writes to the drive it needs administrator rights: use **Restart as administrator** in Settings, or turn on **Start with Windows**, which asks once and then starts it with those rights at every sign-in.

**SSDWatch is version 0.2.0. Use it at your own risk.** It only reads information from your drives and from Windows, and it does not change your files, but its numbers are measurements and estimates, not guarantees. It is provided "as is", without warranty, as the [license](LICENSE.md) explains. If you find a problem, please [open an issue](https://github.com/edukode-lab/SSDWatch/issues).

## Technical details

- **Requirements:** Windows 10 or 11, 64-bit. Health and wear need an NVMe SSD; SATA SSDs show their writes only.
- **A native Windows program,** written in C with Win32, Direct2D and DirectWrite. It has no framework to install and needs no Visual C++ redistributable.
- **How it measures:** with administrator rights, Windows' own event tracing (ETW) gives every write that reaches a disk and the program behind it; the drive's SMART log gives the counters of the drive itself. Without administrator rights, the programs' I/O counters are used, which include writes that never reach the disk.
- **Where it keeps things:** the history (one CSV file per day) and a log of the alerts in `%LOCALAPPDATA%\SSDWatch`; the settings in `%APPDATA%\SSDWatch`. Both survive an update, and a version that kept its history next to the program moves it there by itself. "Start with Windows" is a scheduled task named `SSDWatch`, which you can see and remove in Task Scheduler.
- **Light on the computer:** it looks once a second and keeps its history in a few kilobytes a day.

### Privacy

SSDWatch does not collect or send any data about you. It has no telemetry and no accounts, and it never connects to the internet. The only link is the **Open on GitHub** button of the About window, which opens this page in your browser when you press it.

## License

SSDWatch is free for personal, educational and non-profit use. Commercial use needs a commercial license.
Read the full terms in [LICENSE.md](LICENSE.md) (English) or [LICENCIA.md](LICENCIA.md) (Spanish; it prevails if they differ).

## Support

If you like SSDWatch and want to support its development, you can buy me a coffee.

<a href="https://ko-fi.com/edukode"><img src="res/support-kofi.svg" alt="Buy me a coffee on Ko-fi" width="220"></a>

