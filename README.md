
![WhatsApp Image 2024-07-26 at 22 03 54](https://github.com/user-attachments/assets/db0dca20-9529-42a5-8f44-d62ef1f6b819)

### Script Breakdown

#### **1. Function Definitions**

**`Test-Proxy` Function**

```powershell
function Test-Proxy {
    param (
        [string]$proxy
    )

    $url = "http://google.com" 
    $proxyUri = "http://$proxy"

    try {
        $request = [System.Net.HttpWebRequest]::Create($url)
        $request.Proxy = New-Object System.Net.WebProxy($proxyUri, $true)
        $request.Timeout = 5000  

        $response = $request.GetResponse()
        $response.Close()
        return $true
    } catch {
        return $false
    }
}
```

- **Purpose**: This function tests if a given proxy server can successfully make a request to `http://google.com`.
- **Parameters**: 
  - `$proxy`: A string representing the proxy server in the format `address:port`.
- **Details**:
  - `HttpWebRequest` is used to create an HTTP request.
  - `New-Object System.Net.WebProxy($proxyUri, $true)` sets the proxy for the request.
  - `Timeout` is set to 5000 milliseconds (5 seconds).
  - `GetResponse()` attempts to get a response from the URL.
  - If successful, the function returns `$true`; otherwise, it returns `$false` if an error occurs.

**`Check-ProxiesFromFile` Function**

```powershell
function Check-ProxiesFromFile {
    param (
        [string]$filePath
    )

    if (-Not (Test-Path $filePath)) {
        Write-Host "File not found!" -ForegroundColor Red
        return
    }

    $proxies = Get-Content $filePath
    foreach ($proxy in $proxies) {
        if (Test-Proxy -proxy $proxy) {
            Write-Host "$proxy > Google: yes | Chk by Spishere" -ForegroundColor Green
        } else {
            Write-Host "$proxy > Google: no | Chk by Spishere" -ForegroundColor Red
        }
    }
}
```

- **Purpose**: This function reads a list of proxy servers from a file and checks each one to see if it can connect to Google.
- **Parameters**:
  - `$filePath`: The path to the file containing the list of proxies.
- **Details**:
  - `Test-Path` checks if the file exists.
  - `Get-Content` reads the content of the file (assumed to be a list of proxy addresses).
  - For each proxy in the file, `Test-Proxy` is called to check its connectivity.
  - `Write-Host` outputs the result of each proxy check, with color-coded messages indicating success or failure.

**`Show-AsciiArt` Function**

```powershell
function Show-AsciiArt {
    $asciiArt = @"
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
██░▄▄░█░▄▄▀█▀▄▄▀█▀▄▀█░█░█░██░██
██░▀▀░█░▀▀▄█░██░█░█▀█▀▄▀█░▀▀░██
██░████▄█▄▄██▄▄███▄██▄█▄█▀▀▀▄██
▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
"@

    Write-Host $asciiArt -ForegroundColor Cyan
}
```

- **Purpose**: This function displays ASCII art in the PowerShell console.
- **Details**:
  - `$asciiArt` contains a block of text representing the ASCII art.
  - `Write-Host` outputs the ASCII art with cyan color.

#### **Script Execution**

```powershell
Show-AsciiArt
Write-Host "example 102.xxx:80" -ForegroundColor Yellow
$filePath = Read-Host "Input filename"
Check-ProxiesFromFile -filePath $filePath
```

- **Show-AsciiArt**: Calls the function to display the ASCII art.
- **Write-Host "example 102.xxx:80"**: Prints an example proxy address in yellow.
- **Read-Host "Input filename"**: Prompts the user to input the filename containing the proxy list.
- **Check-ProxiesFromFile -filePath $filePath**: Calls the function to check proxies from the provided file.

---

## Proxy Checker Script

This PowerShell script checks the connectivity of proxies listed in a file to see if they can access `http://google.com`.

### Functions

- **`Test-Proxy`**: 
  - Tests if a given proxy can access `http://google.com`.
  - Returns `true` if successful, `false` otherwise.

- **`Check-ProxiesFromFile`**: 
  - Reads a list of proxies from a file.
  - Checks each proxy's connectivity using `Test-Proxy`.
  - Outputs results to the console.

- **`Show-AsciiArt`**: 
  - Displays a block of ASCII art in the console.

### Usage

1. **Display ASCII Art**:
   ```powershell
   Show-AsciiArt
   ```

2. **Check Proxies**:
   ```powershell
   Write-Host "example 102.xxx:80" -ForegroundColor Yellow
   $filePath = Read-Host "Input filename"
   Check-ProxiesFromFile -filePath $filePath
   ```

   - Replace `102.xxx:80` with an example proxy address.
   - Provide the path to the file containing proxy addresses when prompted.

### Notes

- Ensure the proxy list file exists and is accessible.
- The script outputs results with color-coded messages: green for success and red for failure.

---

**Download the Executable:**

<a href="https://github.com/hy011121/PxChk/releases/tag/Download">
    <img src="https://github.com/user-attachments/assets/4f3a403f-8a45-417d-be60-84cb1f1b59b7" width="50" alt="Windows" /> 
</a>

**Click the image above to go to the latest release page and download the executable.**

**SHA256** ```af3715982b12ba687572176ad36f267d00951f6bb6a5e95255d1818f00341609:PxChk.exe;```


**Video Demo:** ```https://crax.tube/watch/proxy-checker-pxchk_GzfZvC49OqKD15L.html```
