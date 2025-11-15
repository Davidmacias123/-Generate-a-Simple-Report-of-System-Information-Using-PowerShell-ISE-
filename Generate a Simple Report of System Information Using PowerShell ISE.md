# Simple System Report Using PowerShell ISE

A simple PowerShell ISE project that gathers system information, including computer name, OS details, and disk space. Each command is broken down step-by-step, with easy explanations and Windows vs Linux comparisons, creating a clear and beginner-friendly system report script.

> Success Coach Challenge – Generate a Simple Report of System Information Using PowerShell ISE  

This project walks through building a small PowerShell script that collects system information and saves it into a text file.  
Everything is explained in a simple, step-by-step way so that even someone new to IT can follow along.

---

## Purpose

Success Coach Challenges help learners gain hands-on experience with real IT tasks in a supported environment.  

This challenge focuses on creating a simple PowerShell script that:

- Collects basic system information  
- Shows computer and operating system details  
- Calculates free and total disk space  
- Saves everything into a text file on the Desktop  

This kind of report helps an IT team:

- Understand how each system is configured  
- Monitor disk space for possible issues  
- Keep an updated inventory for troubleshooting and audits  

---

## Learning Objectives

By completing this project, a learner practices how to:

1. Use PowerShell cmdlets to gather system information.  
2. Store important values inside variables for later use in the script.  
3. Format numbers so they are easy to read and interpret.  
4. Send output into a text file for baselining and record keeping.  
5. Use PowerShell ISE to write, run, and debug a script.  
6. Connect PowerShell concepts with similar Linux shell concepts (like `echo`, environment variables, and redirection).

---

## Tools Used

- **PowerShell ISE**  
  Editor and console used to write and run the PowerShell script.

- **PowerShell scripting engine**  
  Executes the `.ps1` script and processes cmdlets and commands.

- **WMI (Windows Management Instrumentation)**  
  Provides detailed information about the operating system through WMI classes, such as `Win32_OperatingSystem`.

- **Windows file system**  
  Stores the final report on the Desktop as a `.txt` file.

---

# Challenge Steps – Explained in Success Coach Order

---

## Step 1 – Open PowerShell ISE and Prepare the Script

1. Open **PowerShell ISE as Administrator**.  
   - Running as Administrator gives the script permission to access system details that may require elevated rights.  
   - This is similar to using `sudo` in Linux to run commands with higher privileges.

2. In the script pane of PowerShell ISE, create a new script file and save it as:

   ```text
   Simple_System_Report.ps1
   ```

   - `.ps1` is the standard file extension for PowerShell scripts.  
   - This is similar to how `.sh` is commonly used for shell scripts in Linux.



<img width="1058" height="787" alt="image" src="https://github.com/user-attachments/assets/de9111d8-3c90-44b7-8c3b-9840c3bbae04" />


---

## Step 2 – Create Variables for System Information

A **variable** is a named container in memory that holds a value for later use.  
In PowerShell, every variable name begins with a dollar sign `$`.

This script uses these main variables:

- `$reportPath` – where the report file will be saved.  
- `$computerName` – the computer’s name.  
- `$os` – the operating system information.  
- `$freeSpace` – free disk space on drive `C:` in gigabytes.  
- `$totalSpace` – total disk space on drive `C:` in gigabytes.

Each variable line is broken down and then described in terms of what it does as a whole.

---

### Step 2a – Store the Computer Name

```powershell
$computerName = $env:COMPUTERNAME
```

<img width="707" height="40" alt="image" src="https://github.com/user-attachments/assets/1479a1f0-9f99-432c-a424-dec5ea9d4db2" />


#### Breaking Down Each Part

- **`$computerName`**  
  Variable that will store the name of the computer. This value will later be printed into the report.

- **`=`**  
  Assignment operator. It means “take what is on the right side and store it in the variable on the left side.”

- **`$env:`**  
  PowerShell prefix used to access **environment variables**.  
  Environment variables are system-wide values that the operating system always keeps in memory, such as the username, home directory, and computer name.

- **`COMPUTERNAME`**  
  Name of the specific environment variable that stores the Windows machine name.  
  Example value: `WIN-ABC12345`.

#### What This Command Does

This command reads the computer name from the Windows environment (`$env:COMPUTERNAME`) and stores it into the variable `$computerName`.  
Later, this stored value is used when writing the report.

#### Windows vs Linux Comparison

| Purpose                | Windows PowerShell               | Linux Bash               |
|------------------------|----------------------------------|--------------------------|
| Get computer name      | `$env:COMPUTERNAME`             | `$HOSTNAME`              |
| Store in a variable    | `$computerName = $env:COMPUTERNAME` | `computerName=$HOSTNAME` |

---

### Step 2b – Store Operating System Information

```powershell
$os = Get-WmiObject -Class Win32_OperatingSystem
```

<img width="706" height="17" alt="image" src="https://github.com/user-attachments/assets/90d09bca-d054-482e-b8b4-7b141b4c954d" />



#### Breaking Down Each Part

- **`$os`**  
  Variable that will hold the operating system information.

- **`=`**  
  Assignment operator that stores the result of the command on the right side into the variable on the left.

- **`Get-WmiObject`**  
  PowerShell cmdlet that retrieves information from **WMI** (Windows Management Instrumentation).  
  WMI is like an internal database for Windows that holds detailed information about hardware, software, and the operating system.

- **`-Class`**  
  Parameter that tells `Get-WmiObject` which WMI class to query.

- **`Win32_OperatingSystem`**  
  The WMI class that returns details about the current operating system such as:
  - `Version` – Operating system version.  
  - `BuildNumber` – Build number of the OS.  
  - `SystemDirectory` – Location of important system files.  
  - `SerialNumber` – OS serial number.  
  - `RegisteredUser` – Name of the registered user.

#### What This Command Does

This command asks Windows (through WMI) for information about the operating system and stores the full OS object in the `$os` variable.  
That object is later written directly into the report text file.

#### Windows vs Linux Comparison

| Purpose         | Windows PowerShell                           | Linux Shell                          |
|-----------------|----------------------------------------------|--------------------------------------|
| Show OS details | `Get-WmiObject -Class Win32_OperatingSystem` | `uname -a`, `lsb_release -a`, `cat /etc/os-release` |

---

### Step 2c – Store Free Disk Space on C: (in Gigabytes)

```powershell
$freeSpace = (Get-PSDrive C).Free / 1GB
```

<img width="720" height="15" alt="image" src="https://github.com/user-attachments/assets/c0843150-9ff5-449e-98a3-fe530f786431" />


#### Breaking Down Each Part

- **`$freeSpace`**  
  Variable that will store the free space on drive `C:` in gigabytes.

- **`(Get-PSDrive C)`**  
  PowerShell cmdlet that retrieves information about the `C:` drive as a drive object.  
  This object has properties like `Name`, `Free`, and `Used`.

- **`.Free`**  
  Property of the drive object that shows how many **bytes** of free space are available on the drive.

- **`/ 1GB`**  
  Division operator combined with the size constant `1GB`.  
  PowerShell size constants (like `1KB`, `1MB`, `1GB`) represent a number of bytes.  
  Dividing by `1GB` converts the free space value from bytes into gigabytes.

#### What This Command Does

This command looks up the free space in bytes on drive `C:` and converts that value into gigabytes.  
The result (for example, `35.20`) is stored in the `$freeSpace` variable.

#### Windows vs Linux Comparison

| Purpose                     | Windows PowerShell          | Linux Command             |
|-----------------------------|-----------------------------|---------------------------|
| View disk usage             | `Get-PSDrive C`             | `df -h /`                 |
| View free space (raw)       | `(Get-PSDrive C).Free`      | `df` without `-h`         |
| View free space (readable)  | `(Get-PSDrive C).Free / 1GB`| `df -h /` (already human-readable) |

---

### Step 2d – Store Total Disk Space on C: (in Gigabytes)

```powershell
$totalSpace = ((Get-PSDrive C).Used + (Get-PSDrive C).Free) / 1GB
```

<img width="1176" height="22" alt="image" src="https://github.com/user-attachments/assets/298d90ed-fb4f-46e6-8e0c-85ebabedb60b" />


#### Breaking Down Each Part

- **`$totalSpace`**  
  Variable that will store the total size of drive `C:` in gigabytes.

- **`(Get-PSDrive C).Used`**  
  Property that shows how many bytes are already used on `C:`.

- **`(Get-PSDrive C).Free`**  
  Property that shows how many bytes are still available on `C:`.

- **`(Get-PSDrive C).Used + (Get-PSDrive C).Free`**  
  Adds used bytes and free bytes together to calculate the total number of bytes on the drive.

- **`/ 1GB`**  
  Converts the total bytes into gigabytes using the `1GB` size constant.

#### What This Command Does

This command calculates the full size of the `C:` drive by adding the used space and the free space together, then converts that total into gigabytes.  
The result (for example, `49.39`) is stored in the `$totalSpace` variable.

#### Windows vs Linux Comparison

| Purpose               | Windows PowerShell                                 | Linux                          |
|-----------------------|----------------------------------------------------|--------------------------------|
| Total disk size       | `(Used + Free) / 1GB` from `Get-PSDrive C`        | `df -h` (Size column)          |

---

## Step 3 – Output the Information

PowerShell uses `Write-Output` to send text or objects into the pipeline.  
Linux shells commonly use `echo` to print text to the terminal.

### Windows vs Linux – Printing and Redirection

| Purpose                       | Windows PowerShell                          | Linux Bash                       |
|-------------------------------|---------------------------------------------|----------------------------------|
| Print simple text             | `Write-Output "Hello"`                      | `echo "Hello"`                   |
| Print variable                | `Write-Output $computerName`                | `echo "$HOSTNAME"`               |
| Send output to a file (overwrite) | `... \| Out-File report.txt`             | `... > report.txt`               |
| Append output to a file       | `... \| Out-File report.txt -Append`        | `... >> report.txt`              |

---

### Step 3a – Output the Computer Name to the Report File

```powershell
Write-Output "Computer Name: $computerName" | Out-File $reportPath
```

<img width="1145" height="21" alt="image" src="https://github.com/user-attachments/assets/e3595274-d61a-48a6-9ebc-188b13e17a8d" />



#### Breaking Down Each Part

- **`Write-Output`**  
  PowerShell cmdlet that prints text or values into the pipeline.  
  It is similar in purpose to the `echo` command in Linux.

- **`"Computer Name: $computerName"`**  
  String that combines a label (`Computer Name:`) with the actual computer name stored in `$computerName`.  
  Example output: `Computer Name: WIN-ABC123`.

- **`|` (pipe)**  
  Sends the output from `Write-Output` into the next command on the right side.

- **`Out-File $reportPath`**  
  Takes the incoming text and writes it to the file defined by `$reportPath`.  
  If the file does not exist, it creates it.  
  Without `-Append`, it overwrites any existing content.

#### What This Command Does

This command prints a line that shows the computer name and then creates (or overwrites) the report file at `$reportPath` with that line as the first entry.

#### Windows vs Linux Comparison

| Purpose         | Windows PowerShell                                        | Linux Bash                              |
|-----------------|-----------------------------------------------------------|-----------------------------------------|
| Print & write   | `Write-Output "text" \| Out-File report.txt`             | `echo "text" > report.txt`             |

---

### Step 3b – Output the Operating System Information

```powershell
Write-Output $os | Out-File $reportPath -Append
```

<img width="1127" height="20" alt="image" src="https://github.com/user-attachments/assets/890e63ca-eda3-4125-b68a-8f5bd586a319" />


#### Breaking Down Each Part

- **`Write-Output $os`**  
  Sends the OS information object stored in `$os` through the pipeline.  
  PowerShell automatically converts the object into readable text.

- **`|`**  
  Pipes the text into the	next command.

- **`Out-File $reportPath -Append`**  
  Writes the text to the same report file, but uses `-Append` so the new content is added at the end of the file instead of replacing it.

#### What This Command Does

This command prints all the operating system details and adds them below the computer name in the existing report file.

#### Windows vs Linux Comparison

| Purpose                 | Windows PowerShell                             | Linux Bash             |
|-------------------------|------------------------------------------------|------------------------|
| Append information      | `... \| Out-File report.txt -Append`          | `... >> report.txt`    |

---

### Step 3c – Output Free Space (Formatted)

```powershell
Write-Output ("Free Space(GB):{0:N2}" -f $freeSpace) | Out-File $reportPath -Append
```

<img width="1352" height="26" alt="image" src="https://github.com/user-attachments/assets/58cbb442-6690-44a2-bcbe-178184ba13b8" />


This line uses the PowerShell **format operator** `-f`.

#### Breaking Down Each Part

- **`"Free Space(GB):{0:N2}"`**  
  Format string that contains:
  - A label: `Free Space(GB):`  
  - A placeholder: `{0:N2}`  

  `{0:N2}` means:  
  - `0` → first value after the `-f` operator.  
  - `N2` → numeric format with 2 decimal places.

- **`-f $freeSpace`**  
  The **format operator** in PowerShell.  
  It takes the value in `$freeSpace` and inserts it where `{0:N2}` is in the string, applying the number formatting.

- **`Write-Output (...)`**  
  Prints the formatted string into the pipeline.  
  Example result: `Free Space(GB):35.20`.

- **`| Out-File $reportPath -Append`**  
  Appends this new line to the report file.

#### What This Command Does

This command formats the free disk space value into a friendly text line like:

```text
Free Space(GB):35.20
```

and then adds that line to the end of the report file.

#### Extra Examples of the `-f` Format Operator

```powershell
"Value: {0}" -f 10
# Output: Value: 10

"Rounded: {0:N2}" -f 12.3456
# Output: Rounded: 12.35

"{0} + {1} = {2}" -f 3, 4, (3 + 4)
# Output: 3 + 4 = 7
```

---

### Step 3d – Output Total Space (Formatted)

```powershell
Write-Output ("Total Space(GB):{0:N2}" -f $totalSpace) | Out-File $reportPath -Append
```

<img width="1527" height="36" alt="image" src="https://github.com/user-attachments/assets/97e72cd4-aa09-4469-8322-a28fd17f035b" />


#### Breaking Down Each Part

- **`"Total Space(GB):{0:N2}"`**  
  Format string with a label and placeholder for a number with 2 decimal places.

- **`-f $totalSpace`**  
  Inserts the total disk space value (rounded to 2 decimals) into the placeholder.

- **`Write-Output (...)`**  
  Prints the final formatted string, for example:  
  `Total Space(GB):49.39`.

- **`| Out-File $reportPath -Append`**  
  Appends this line to the existing report file.

#### What This Command Does

This command creates a line showing the total size of the `C:` drive in gigabytes (rounded to two decimal places) and adds it to the report.

---

## Step 4 – Set the Report Path and Display a Final Message

### Step 4a – Define the Report Path

```powershell
$reportPath = "$env:USERPROFILE\Desktop\Simple_System_Report.txt"
```

<img width="1241" height="37" alt="image" src="https://github.com/user-attachments/assets/f0ab6058-41a6-4f78-b8b6-1a09265f3440" />



#### Breaking Down Each Part

- **`$reportPath`**  
  Variable that stores the full file path where the report will be saved.

- **`$env:USERPROFILE`**  
  Environment variable that contains the path to the current user’s home directory.  
  Example: `C:\Users\Student`.

- **`\Desktop\Simple_System_Report.txt`**  
  Relative path that adds the Desktop folder and the name of the report file.

Combining these pieces often results in:

```text
C:\Users\Student\Desktop\Simple_System_Report.txt
```

#### What This Command Does

This command sets `$reportPath` so the script knows exactly where to create and update the report file on the Desktop.

#### Windows vs Linux Comparison

| Purpose                     | Windows PowerShell                                  | Linux Bash                         |
|-----------------------------|-----------------------------------------------------|------------------------------------|
| Home directory variable     | `$env:USERPROFILE`                                  | `$HOME`                            |
| Desktop path example        | `$env:USERPROFILE\Desktop\Simple_System_Report.txt` | `$HOME/Desktop/simple_report.txt`  |

---

### Step 4b – How the Report File Is Built

- The **first** `Out-File` call (without `-Append`) creates the file and writes the computer name.  
- All later `Out-File` calls use `-Append` so that OS information, free space, and total space are stacked line by line under the first entry.

This builds the full system report in a readable order.

---

### Step 4c – Print the Final Message to the Console

```powershell
Write-Output "The system report will be saved to: $reportPath"
```

<img width="1522" height="60" alt="image" src="https://github.com/user-attachments/assets/9f8ccca1-f3ab-4de2-9523-9ca7163470ba" />


#### Breaking Down Each Part

- **`Write-Output`**  
  Prints text to the PowerShell console.

- **`"The system report will be saved to: $reportPath"`**  
  Message that includes the full file path stored in `$reportPath`.

#### What This Command Does

This command prints a friendly confirmation message in the PowerShell console that tells exactly where the system report file is located.

Example console output:

```text
The system report will be saved to: C:\Users\Student\Desktop\Simple_System_Report.txt
```

---

## Final Script (Complete)

```powershell
$reportPath   = "$env:USERPROFILE\Desktop\Simple_System_Report.txt"

$computerName = $env:COMPUTERNAME
Write-Output "Computer Name: $computerName" | Out-File $reportPath

$os           = Get-WmiObject -Class Win32_OperatingSystem
Write-Output $os | Out-File $reportPath -Append

$freeSpace    = (Get-PSDrive C).Free / 1GB
$totalSpace   = ((Get-PSDrive C).Used + (Get-PSDrive C).Free) / 1GB

Write-Output ("Free Space(GB):{0:N2}"  -f $freeSpace)  | Out-File $reportPath -Append
Write-Output ("Total Space(GB):{0:N2}" -f $totalSpace) | Out-File $reportPath -Append

Write-Output "The system report will be saved to: $reportPath"
```

****What the Final Report Looks Like****

After each command runs and all system information is collected, the script creates a text file named Simple_System_Report.txt on the Desktop.
This file shows a clear summary of the computer’s basic information.

This report includes:

The computer’s name

System directory

Organization field (if configured)

Windows build number

Registered user

Serial number

Windows version

Free space on drive C:

Total space on drive C:


<img width="1142" height="818" alt="image" src="https://github.com/user-attachments/assets/6522508d-02db-42d8-9a09-fa5d9fcb6b62" />



---

<img width="1228" height="835" alt="image" src="https://github.com/user-attachments/assets/f2de5d1e-dae1-43c7-859c-f42f7b157216" />


---



<img width="1250" height="853" alt="image" src="https://github.com/user-attachments/assets/44d25f02-d0e8-4c18-b0c8-8ae94f78c06d" />

---


<img width="1285" height="852" alt="image" src="https://github.com/user-attachments/assets/754946e3-60af-4b7b-92af-bb2b7dcaa826" />





