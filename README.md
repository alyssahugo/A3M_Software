This contains the processor Files from the [CIDR RIV32IMC Project](https://gitlab.eee.upd.edu.ph/cidr-p3-public/pipelined-RV32IMC/-/tree/master?ref_type=heads), with modifications from Hora et al.

# Files to Download

1.  [[Second_import_files]{.underline}](https://drive.google.com/drive/folders/1OLKL3d8LRpCeD8sCe7np8FxyHxcBp54E?usp=drive_link)

2.  [**[[NEW! second_import_files]{.underline}]{.mark}**](https://drive.google.com/drive/folders/1VVjZdFEsaMmFBCsvrEgEZ5Bqd3OeqhHY?usp=sharing)

3.  [[MIG_7]{.underline}](https://drive.google.com/drive/folders/1m5bgVZMtVzj3Vllg9JPMsSTLAW96qnyu?usp=sharing) - put this in the same folder as second_import_files

4.  [[Nexys Video board files]{.underline}](https://drive.google.com/drive/folders/1OLKL3d8LRpCeD8sCe7np8FxyHxcBp54E?usp=drive_link)

    a.  D:\Xilinx\Vivado\2024.1\data\boards\board_files

5.  [[Datamem and Instmem COE files]{.underline}](https://drive.google.com/drive/folders/1OLKL3d8LRpCeD8sCe7np8FxyHxcBp54E?usp=sharing)

6.  [**[[MCS Folder]{.underline}]{.mark}**](https://drive.google.com/drive/folders/1jLJ0glUKPrnkScBCat9Iahh-sUfTC03I?usp=sharing)

# Importing the Processor Core

1.  Open Vivado.

2.  Create a New Project.

3.  Click Next until you go to the Boards tab. Click and Select Nexys Video.

4.  Open the project_run.tcl file in Notepad.  
    > ![](media/image2.png){width="5.859375546806649in" height="0.5258409886264217in"}

5.  Change the source directory to where the second_import_files folder is located (or where you downloaded it).

6.  Go to the top right of Vivado, click Tools \> Run tcl script \> project_run.tcl file. (Checkpoint: 47 files in the Design Sources) **EDIT:** This folder contains three new verilog files for the QSPI. **AND the Block Design TCL already includes the DMA controller and CSR modules**, so **make sure** that those modules are also in your Design Sources. So, aside from importing the second_import_files, also upload the dma controller and csr verilog files alongside it. Because the second_import_files folder does not have the verilog files for that.

7.  Right click on the Constraints \> a7_200t folder \> Make Active

8.  Open uart_bd.tcl. Search for datamem_run and instmem_run. Replace the directory where datamem_run.coe and instmem_run.coe is in your own files.

> ![](media/image6.png){width="5.828125546806649in" height="1.3823118985126859in"}

9.  Go to the top right of Vivado and click Tools \> Run Tcl script \> uart_bd_with_qspi.tcl. It should build the block design. You should now see the block design being built. As long as the modules are all present in the processor folder inside the second_import_files, it should be okay, and the block design must be built successfully.

# For generating Divider IP, follow these instructions through the images. {#for-generating-divider-ip-follow-these-instructions-through-the-images.}

1.  Click IP Catalog on the left project manager.

2.  Search for Divider Generator, click on it.

3.  Click Customize IP.

4.  Make sure to name it div_gen_signed and div_gen_unsigned.

5.  Apply the settings on the picture.

![](media/image8.png){width="6.5in" height="2.7222222222222223in"}

![](media/image11.png){width="2.2375339020122484in" height="1.4504615048118985in"}

![](media/image7.png){width="4.098958880139983in" height="1.2991229221347331in"}

![](media/image5.png){width="5.859375546806649in" height="4.306990376202974in"}![](media/image1.png){width="5.380208880139983in" height="3.9413156167979in"}![](media/image4.png){width="4.4396281714785655in" height="1.9010422134733158in"}

# Generating Bitstream Steps:

1.  Make sure all modified verilog modules are saved. If you made changes, click Report IP Status and select Upgrade Selected. Also, Generate Output Products of the new upgraded IP. Select Global.

2.  After all IPs are updated, right click on the Block Design and select Validate Design.

3.  After validating, Ctrl + S to save block design.

4.  On the Sources pane, click on IP sources.  
    > ![](media/image9.png){width="3.1458333333333335in" height="2.3645833333333335in"}

5.  Right-click on uart_bd. Select Reset Output Products.

6.  Then right-click again. Select Create HDL Wrapper.

7.  Ignore all the warnings if it just says some pins are not connected.

8.  Right-click, Select Generate Output Products: Global.

9.  Then you may select Generate Bitstream.

#   {#section}

# [Running:]{.mark}

1.  After generating bistream, click "Open Hardware Manager" in the Flow Navigator.

2.  Click Auto Connect when finding a new target.

3.  The board will appear.

4.  You will also see Add Configuration Memory Device. Click that. Then on the search bar, search: **s25fl256sxxxxxx0-spi-x1_x2_x4**

> ![](media/image3.png){width="2.6666666666666665in" height="1.375in"}

5.  Click on it and it will ask you Do You want to program memory configuration device now. Click OK. It will then show you a window that you will put the .MCS file in. I will send the MCS file. Click on the ellipsis button to load your file. Make sure these are checked:![](media/image10.png){width="3.0364588801399823in" height="4.2089523184601925in"}

> Click Appy. Then Click OK.

6.  You will see the blue command lines appearing on the console both after clicking Apply and OK. If there are no errors, it will proceed with the Programming and Verification steps. I think it was like 4 steps. It's just a dialogue box.

7.  You should by now **redirect** to the [[UART PUTTY]{.underline}](?tab=t.0) steps. You must open the Serial terminal before or during Programming configuration memory device.

8.  After the PUTTY terminal setup, proceed back here. On the green task area at the top, click on Program Device. It will show you the dialogue box that shows the bitstream and ltx debug ILA file, that is usually the file generated in the same project window you're in. Make sure it's the right bitstream, then click Program.

9.  At first, there will be nothing printing on the serial terminal. That's the QSPI bootloading active time. After 10-15 seconds, something should print, depending on the program you're running.

# Remarks

- I am not completely sure if this will be successful without manually importing the MIG_7 IP in the block design instead of relying on the bd tcl. Please tell me asap if there are errors.
