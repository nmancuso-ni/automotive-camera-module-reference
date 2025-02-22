# PXIe-148X Getting Started Example - Basic Generation Tutorial
{: .no_toc }

This tutorial teaches you the steps needed to configure a generation using a PXIe-148X module and familiarize you with the basic functionality provided by the Generation Getting Started Example and Create CSI-2 Packet TDMS Files VI.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites

Review and complete all setup from the [PXIe-1486 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1486-getting-started/) or [PXIe-1487 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1487-getting-started/).

Basic knowledge of LabVIEW and LabVIEW FPGA concepts. 

A supported interface module on a PXI system running a compatible Windows version.

| **Interface Modules**   |
|------------------------|
| PXIe-1486 (8 Out)      |
| PXIe-1486 (4 In 4 Out) |
| PXIe-1487 (8 Out)      |
| PXIe-1487 (4 In 4 Out) |

## Initial Hardware Setup

Complete installation of hardware as described in the Getting Started Guide for your interface module. No physical connections to other modules or devices are required to complete this tutorial.

## Initial Software Setup

Use the NI Example Finder to create a default project for your interface module.

1.  Complete installation of software specified in the Getting Started Guide for your interface module.
2.  Open LabVIEW and click **Help » Find Examples...**
    
    ![Open NI Example Finder](../../images/LabVIEW-Help-FindExamples.png)
    
3. In the NI Example Finder dialog, double-click **Hardware Input and Output » FlexRIO » Integrated IO » Getting Started » Getting Started FlexRIO Integrated IO.vi**
    
    ![NI Example Finder](../../images/NIExampleFinder-GSEFlexRIOWithIntegratedIO.png)
    
4. In the FlexRIO with Integrated IO Project Creator dialog, set the **Project Name** to <font face = "courier new">PXIe-148X Tutorial</font>, make the **FlexRIO Integrated IO** selection for the desired PXIe-148X interface module, and click **OK**.
    
    ![FlexRIO with Integrated IO Project Creator](../../images/FlexRIOWithIntegratedIOProjectCreator-1486-PXIe-148XTutorial.png)

## Create TDMS Files for Generation
Create a TDMS file to house the image data you will generate during the tutorial.

> Note: For the purposes of this tutorial, all input control values not specified should be left with their default values.

1. Double-click the Create CSI-2 Packet TDMS Files VI in the LabVIEW project.

    ![Open Create CSI-2 Packet TDMS Files VI](../../images/PXIe-148X-CreateTDMS-Project.png)

    > The opened front panel of the Create CSI-2 Packet TDMS Files VI utility is similar to the figure below.

    ![Open Create CSI-2 Packet TDMS Files Front Panel](../../images/PXIe-148X-CreateTDMS-FrontPanel.png)

2.  Review the instructions on the VI front panel; for this tutorial, the default settings are used to generate a TDMS file for channel SO0 containing 10 frames at 1920 × 1080 and 30 fps.

    > Note: During the first run of the VI, the **TDMS File Directory** control automatically populates with a value pointing to a subfolder (\"TDMS Files\"). This subfolder is automatically created within the project folder to store any generated TDMS files.

3.  Run the VI to generate the TDMS file.

4.  Open Windows Explorer and navigate to `<yourprojectdir>\Host\Gen\TDMS Files`.

    The newly created TDMS file has the prefix "SI0_" to indicate it is associated with the first channel 'SO0'. Although the 'SI' prefix suggests 'serial input', when used with the Generation GSE, the TDMS file actually associates with serial output channel 'SO0'.

    ![Create CSI-2 Packet TDMS Files Explorer View](../../images/PXIe-148X-CreateTDMS-ExplorerView.png)

The newly created TDMS file is used in the next part of the tutorial to perform a simple generation.

## Performing a Simple Generation
Complete this part of the tutorial to generate images, which will be stored to the TDMS file you created.

> Note: For the purposes of this tutorial, all input control values not specified should be left with their default values.

1. Double-click the Generation Example VI in the LabVIEW project.

    ![Open Create CSI-2 Packet TDMS Files VI](../../images/PXIe-148X-GenGSE-Project.png)

    > The opened front panel of the Generation Getting Started Example is similar to the figure below.

    ![Generation GSE Front Panel](../../images/PXIe-148X-GenGSE-FrontPanel.png)

2.  Select the **Resource** tab and make the following modifications.
    - Select the **RIO Device** from the dropdown menu that corresponds to your interface module.
    - Set the **Bitfile Path** to the bitfile that corresponds to your interface module. The default shipping bitfile is in the `FPGA Bitfiles` subfolder located in the project folder (created at the Project Root location specified in Project Creator):

    | **Interface Module**   | **Bitfile**                                          |
    |------------------------|------------------------------------------------------|
    | PXIe-1486 (8 Out)       | FPGA Bitfiles\\PXIe_1486_8\_Out.lvbitx              |
    | PXIe-1486 (4 In 4 Out) | FPGA Bitfiles\\PXIe_1486_4\_In_4\_Out_Gen.lvbitx |
    | PXIe-1487 (8 Out)       | FPGA Bitfiles\\PXIe_1487_8\_Out.lvbitx              |
    | PXIe-1487 (4 In 4 Out) | FPGA Bitfiles\\PXIe_1487_4\_In_4\_Out_Gen.lvbitx |

    > The values on the **Resource** tab of **Configuration Settings** are now similar to the figure below.

    ![Configuration Settings Resource Tab](../../images/PXIe-148X-GenGSE-ResourceTab-BasicSimpleGen.png)

    > Note: **TDMS File Directory** will be automatically populated when you run the VI.

3.  Run the VI to perform a generation using the TDMS file created in the previous part of the tutorial.
    
    The Generation GSE example loads the bitfile and then buffers available data from disk to the onboard DRAM of the interface module. After buffering, the **Waiting for Serializer Setup** indicator illuminates to indicate the module is ready for generation.

    ![Generation GSE Front Panel Waiting for Serializer Setup](../../images/PXIe-148X-GenGSE-FrontPanel-Waiting.png)

5.  Click the **Serializer Setup Complete** control button to start generation. Generated images display automatically in the First Display Channel tab.

    ![Configuration Settings Resource Tab](../../images/PXIe-148X-GenGSE-FirstDisplayChannel-BasicSimpleGen.png)

    The VI stops automatically after all data from the TDMS file is generated.

## Performing a Simple Generation with GPIO Logging

> Note: This part of the tutorial assumes that all input parameters are still configured as specified in *Performing a Simple Generation*.

1.  Select the **Resource** tab and make the following modifications.
    
    - Enable **Log GPIO to Disk**.

    > Note: During the first run of the VI in *Performing a Simple Generation*, the **TDMS File Directory** control was automatically populated with a value pointing to a subfolder (\"TDMS Files\"). This subfolder was automatically created within the project folder to load any existing TDMS files.

    > The values on the **Resource** tab of **Configuration Settings** are now similar to the figure below.
    
    ![Configuration Settings Resource Tab Log to Disk](../../images/PXIe-148X-GenGSE-ResourceTab-LogGPIOToDisk.png)

2.  Select the **GPIO** tab and make the following modifications.
    - Add a GPIO line to the **GPIO to Display** array with the GPIO Bank value set to **Ser0 GPIO** and the GPIO Number set to <font face = "courier new">0</font>. Setting these values enables display of GPIO traffic for the GPIO 0 line on the SO0 channel.

    > The values on the **GPIO** tab of **Configuration Settings** are now similar to the figure below.

    ![Configuration Settings GPIO Tab with Des0 GPIO0](../../images/PXIe-148X-Gen-GPIOTab-DisplaySer0GPIO0.png)

3.  Run the VI, wait for the **Waiting for Serializer Setup** indicator to illuminate, and then click the **Serializer Setup Complete** control button to start the generation. Results are displayed on the various tabs in the **Data Output** indicator.

4.  Click the **GPIO Timestamps** tab to view GPIO timestamp data for the GPIO lines included in the **GPIO to Display** array in Step 2.

    ![GPIO Timestamps](../../images/PXIe-148X-Gen-GPIOTimestamps-BasicSimpleGen.png)

    > Note: The module interface does not read/write (*toggle*) to GPIO by default. Any change shown in the figure above reflects GPIO level reset when the example VI is first run and may vary by module.

## Related Documents
- [PXIe-148X Getting Started Example - Common Generation Tutorials](./gse-gen-common.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md)
- [PXIe-148X Getting Started Example - Basic Acquisition Tutorial](./gse-acq-basic.md)
- [PXIe-148X Getting Started Example - Basic Tap Tutorial](./gse-tap-basic.md)
    
