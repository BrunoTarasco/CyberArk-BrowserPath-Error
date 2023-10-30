# CyberArk-BrowserPath-Error
This guide provides you with a troubleshooting when facing a PSM error related to the BrowserPath

# Probable Cause
CyberArk PSMs are hardened with multiple restrictions on which software and communications are allowed to them, and one od those restrictions is based on Microsoft's AppLocker.
AppLocker gathers information regarding the authorized applications and can restrict their usage based on the Publisher's Signature, Hash, Path and much more.
If you are using Google Chrome + ChromeDriver to run your connector automations, if not disabled, the automatic update will upgrade your Chrome version, which will require an update on the ChromeDriver. Such upgrade modifies the chromedriver.exe hash, which will cause a restriction from AppLocker and your sessions will start to fail.

# Solution (All Steps should be done on the relevant PSM)
The First Step is to put Applocker into Audit Only mode using SecPol.msc and test the connection again.
If the connection succeeds, you may proceed to the next steps. If not, you may have another issue, like wrong path to Chrome on BrowserPath (check if your Chrome is inside "Program Files" or "Program Files (x86)".

After validating that AppLocker is preventing ChromeDriver.exe from running (you can check that also on Event Viewer via Application and Services Logs\Microsoft\Windows, double-click AppLocker and filter the logs to see the errors), you should go to C:\Windows\System32\Applocker and delete all the files. After that, browse to C:\Program Files (x86)\CyberArk\PSM\Hardening and open a Powershell as Admin from there. On the recently opened Powershell, run the script PSMConfigureAppLocker.ps1.

If the script runs successfully, retry the connection as it should get back to work again.
If not, please reach CyberArk Support.
