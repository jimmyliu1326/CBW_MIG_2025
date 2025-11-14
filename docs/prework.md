# CBW Module 5 Pre-work

Trainees are highly encouraged to install `Bandage` and `BLAST` on their own machines in advance to attending the workshop.

Bandage is a popular desktop application/CLI tool for visualizing graph fragment assembly (.GFA) files. We will be using this tool extensively in Module 5 Lab to analyze bacterial pangenomes.

## Part 1: Installing Bandage

**Step 1. Download the tool**

Choose the download link matching your computer OS:

- [Windows 8/10/11](https://github.com/rrwick/Bandage/releases/download/v0.8.1/Bandage_Windows_v0_8_1.zip)

- [MacOS 11+ (Intel)](https://github.com/rrwick/Bandage/releases/download/v0.9.0/Bandage_macOS-x86-64_v0.9.0.zip)

- [MacOS 11+ (ARM)](https://github.com/rrwick/Bandage/releases/download/v0.9.0/Bandage_macOS-aarch64_v0.9.0.zip)

**Step 2. Decompress the .zip file**

**Step 3. Navigate to the extracted folder and open the Bandage application**

> [!NOTE]  
> If you run into "Bandage is damaged" error, open the terminal, navigate to location of the extracted folder and run `xattr -cr Bandage.app`
>
> For Mac users, you will need to move the `Bandage.app` folder to your Applications directory

If the application runs successfully, you should expect to see the following window:
![img](https://raw.githubusercontent.com/jimmyliu1326/CBW_MIG_2025/refs/heads/main/img/bandage.png)

## Part 2: Installing BLAST locally

**Step 1. Download BLAST installer**

- [Windows 8/10/11](https://ftp.ncbi.nlm.nih.gov/blast/executables/LATEST/ncbi-blast-2.17.0+-win64.exe)
- [MacOS](https://ftp.ncbi.nlm.nih.gov/blast/executables/LATEST/ncbi-blast-2.17.0+-universal.dmg)

**Step 2. Run the installer**

To execute the installer, double click the downloaded `.exe`/`.dmg` file and follow the installer prompts to complete the process.

> [!NOTE]  
> For Mac users, if you receive a warning indicating `Unable to verify content is free of malware`, you can manually override this setting in System Settings > Privacy & Security > Scroll down to Security section > Click "Open Anyway"