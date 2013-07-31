# Lightstreamer StockList Demo Client for Real-Time Data (RTD) #

This project includes a demo project showing integration between Lightstreamer .NET/C# Client and RTD Server for Excel.

## Excel RTD Stock-List Demo ##

<table>
  <tr>
    <td style="text-align: left">
      &nbsp;<a href="http://demos.lightstreamer.com/DotNet_RTDDemo/RTDLibraryExcelDemoSetup.msi" target="_blank"><img src="http://www.lightstreamer.com/img/demo/screen_rtd.png"></a>&nbsp;
      
    </td>
    <td>
      &nbsp;Click here to download and install the application:<br>
      &nbsp;<a href="http://demos.lightstreamer.com/DotNet_RTDDemo/RTDLibraryExcelDemoSetup.msi" target="_blank">http://demos.lightstreamer.com/DotNet_RTDDemo/RTDLibraryExcelDemoSetup.msi</a>
    </td>
  </tr>
</table>

[Real-Time Data (RTD)](http://en.wikipedia.org/wiki/Microsoft_Excel#Using_external_data) is a technology introduced in Microsoft Excel starting from 2002, aimed at replacing DDE for updating spreadsheets in real time.<br>
This demo is made up of a DLL library that acts as an RTD Server, which receives updates from Lightstreamer Server on one side and injects them into Excel on the other side. The library has been developed with C#.NET (full source code is provided, see below). It leverages the <b>.NET Client API for Lightstreamer</b> to subscribe to 30 stock items and the <b>Microsoft Office library</b> to set up the RTD server.

## Dig the code ##

The main class is RtdServer, found in RtdServer.cs, which contains an implementation of the IRtdServer interface. The same class will load a Form showing information regarding lightstreamer updates coming in and Excel updates pushed out.
LightstreamerClient.cs, StocklistConnectionListener.cs and StocklistHandyTableListener.cs contain classes used to interface to the Lightstreamer .NET Client library, as seen in the .NET StockListDemo project.
  
Check out the sources for further explanations.

<i>NOTE: not all the functionalities of the Lightstreamer .NET/C# Client & Local RTD Server for Excel demo are exposed by the classes listed above. You can easily expand those functionalities using the .NET/C# Client API as a reference: [http://www.lightstreamer.com/docs/client_dotnet_api/frames.html](http://www.lightstreamer.com/docs/client_dotnet_api/frames.html).</i>


# Build #

If you want to skip the build and deploy processes of this demo please note that you can click the image or link above to download a ".msi" file ready for installation on your machine. Installation instructions:<br>

1. You need Microsoft Excel 2002 or newer installed on your PC.
2. Click the link or image above to download the application installer (a ".msi" file).
3. Execute the downloaded file to install the application.
4. From the Start menu, go to the "Lightstreamer RTD Demo" folder, click the "Start Lightstreamer Excel RTD Server Demo" link. This will open Excel and automatically load the "ExcelDemo.xls" spreadsheet (which, by the way, is contained in "C:\Program Files (x86)\Lightstreamer .NET RTD Server Demo library for Excel\static").
5. The spreadsheet will activate the Lightstreamer RTD library, which will open a control window, where you can see the data traffic.
<br>
The control windows shows a counter with the updates received from Lightstreamer Server and logs such updates in real time. Is also shows the notifications issued towards Excel. To pause the notification calls, uncheck "Data stream to Excel".

In the Excel spreadsheet, you will see several cells changing in real time. If the update rate looks slow (that is, you don't see several updates in a second), it means that the RTD ThrottleInterval of Excel is set to a high value. In order to activare real-time dispatching, please follow these instructions:
* In Excel, go to the Visual Basic Editor (by pressing ALT+F11 or clicking Visual Basic Editor from the Macro menu (Tools menu).
* Open the Immediate window (press CTRL+G or click Immediate Window on the View menu).
* In the Immediate window type this code and press ENTER: Application.RTD.ThrottleInterval = 0

Otherwise, in order to compile the project, a version of Excel implementing RTD features must be installed (usually Excel 2002, 2003, 2007, 2010+). For more information regarding the RTD technology please visit this [page](http://social.msdn.microsoft.com/Search/en-us?query=RTD).
For more information regarding Visual C# 2010 Express and how to run it, please go to: [http://www.microsoft.com/express/Downloads/#2010-Visual-CS](http://www.microsoft.com/express/Downloads/#2010-Visual-CS).
  
<i>NOTE: You may also use the sources included in this project with another Microsoft IDE or without any IDE but such approach is not covered in this readme.</i>

You just need to create a Visual Studio project for a Class library (DLL) target, then include the sources and properties files and include references to the Microsoft.Office.Interop.Excel1 and Lightstreamer.NET Client API (for Lightstreamer binaries files DotNetClient_N2.dll and DotNetClient_N2.pdb) from the [latest Lightstreamer distribution](http://www.lightstreamer.com/download). After the compilation of your DLL, you need to run RegAsm.exe tool in order to register it against COM. RegAsm.exe is part of the .NET SDK and just generates some Registry entries, like the ones in the example RTDServiceRegistrationExample.reg file.

## Run ##
As said, the shipped data contains a Visual C# Library project, thus not directly runnable. In order to run it, please follow the instructions at the Compile section above. Once RTDLibraryExcelDemo.dll is registered, ExcelDemo.xls has to be opened.
If the registration went successful, Excel will load RTDLibraryExcelDemo.dll, a status window will appear and real-time data will start to be delivered to it. A quick and easy way to avoid dealing with Registry entries is to install the shipped version of the library with the downloadable [RTDLibraryExcelDemoSetup.msi](http://demos.lightstreamer.com/DotNet_RTDDemo/RTDLibraryExcelDemoSetup.msi), then just
compile your own .dll replacing the one installed.

# Deploy #
  
Please note that the RTD technology works on top of <b>DCOM</b>, this means that you could even deploy a centralized remote RTD Server that can be used across your network. In the case of this demo, both RTD Server and Excel will run on the same local computer.
Internet connection is required to make the RTD Server, controlling a Lightstreamer connection, being able to deliver real-time data to Excel.<br>

Obviously you could test the application against your Lightstreamer server installed somewhere, but in this case, you have to recompile the source code after having changed "pushServerHost" and "pushServerPort" variables in RtdServer.cs.
The example requires that the [QUOTE_ADAPTER](https://github.com/Weswit/Lightstreamer-example-Stocklist-adapter-java) and [LiteralBasedProvider](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java) have to be deployed in your local Lightstreamer server instance. 
The factory configuration of Lightstreamer server already provides this adapter deployed.<br>
By default, this release points to our demo Lightstreamer server, so if you just want to see how the application works, you can skip this step.

# See Also #

## Lightstreamer Adapters needed by this demo client ##

* [Lightstreamer StockList Demo Adapter](https://github.com/Weswit/Lightstreamer-example-Stocklist-adapter-java)
* [Lightstreamer Reusable Metadata Adapter in Java](https://github.com/Weswit/Lightstreamer-example-ReusableMetadata-adapter-java)

## Similar demo clients that may interest you ##

* [Lightstreamer StockList Demo Client for JavaScript](https://github.com/Weswit/Lightstreamer-example-Stocklist-client-javascript)
* [Lightstreamer StockList Demo Client for jQuery](https://github.com/Weswit/Lightstreamer-example-StockList-client-jquery)
* [Lightstreamer StockList Demo Client for Dojo](https://github.com/Weswit/Lightstreamer-example-StockList-client-dojo)
* [Lightstreamer StockList Demo Client for .NET](https://github.com/Weswit/Lightstreamer-example-StockList-client-dotnet)
* [Lightstreamer StockList Demo Client for Java SE](https://github.com/Weswit/Lightstreamer-example-StockList-client-java)

# Lightstreamer Compatibility Notes #

- Compatible with Lightstreamer .NET Client Library version 2.1 or newer.