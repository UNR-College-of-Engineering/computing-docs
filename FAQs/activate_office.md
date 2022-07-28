
# Activating Microsoft Office

Microsoft Office 2016 and 2019 require activation against the campus KMS servers. As long as the computer is connected to the campus network (via a wired (non-wifi) connection; or via the VPN) at least every 180 days, Office should remain active.  However, if the grace period lapsed, it must be manually activated.  

## Manual Activation of Microsoft Office 2016 and 2019:

1. If your computer is on wifi or off-campus, you must connect to the VPN before performing these steps.
2. Close out of any running MS Office programs.
3. Open a command prompt as Administrator.  To do this, click **Start**, search for **cmd**, then right-click on cmd.exe and choose **Run as administrator**.
4. copy and paste the following commands pressing the Enter key after each one:
    - if exist "C:\Program Files (x86)\Microsoft Office\Office16\ospp.vbs" (cd "C:\Program Files (x86)\Microsoft Office\Office16") else (cd "c:\Program Files\Microsoft Office\Office16")
    - cscript ospp.vbs /sethst:kms.unr.edu
    - cscript ospp.vbs /act
    - cscript ospp.vbs /dstatus

## Troubleshooting

If this isn't working, verify the following:

- The computer you're running this on is connected to a wired (non-wifi) connection on campus or is connected to the VPN.
- The command window is running as Administrator according to the directions above.  Even if logged in as a user in the Administrators group, it's still necessary to open the cmd window by clicking Run as administrator.
- There are no programs in the office suite (Word, Excel, Powerpoint, Outlook, etc.) running.

