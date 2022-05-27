# Transferring Files

## Transferring Files from Windows to Kali

### SMB

You can set up a quick SMB server on kali using impacket tools and use "copy" on windows to transfer from the smb server.

```
# On Kali
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .
# On windows
copy \\<IP>\kali\<filename> C:\<destination>
```

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

