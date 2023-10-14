# Detecting the Analysis Toolkit

Malware can spot other way in which lab and sandbox systems are different from the normal ones

* **Is the clipboard empty?**
* **Is the mouse cursor moving?**
* **Does the CPU have more than one core?**
* **Does the color of the foreground window change?**
* **Has the system been running for a sufficiently long tim**e?

We can bypass these evasion techniques by patching the code or changing the analysis system's setup and behavior

Malware may do many things to check whether or not it should run

It may call **SetWindowsHookExa**, which can check if the mouse is moving, if a specific button has been pressed, etc

It may call **GetModuleHandleW** to see if it detects AVG software by looking for avghookx.dll

It may call **FindWindow** to see if the name of its window is Olydbg, or some other debugger

It may disable the **BlockInput** defense



## Additional Reading

{% embed url="https://archive.f-secure.com/weblog/archives/00002810.html" %}

{% embed url="https://www.vmware.com/solutions/nsx-firewall.html" %}

{% embed url="https://unit42.paloaltonetworks.com/ticked-off-upatre-malwares-simple-anti-analysis-trick-to-defeat-sandboxes/" %}
