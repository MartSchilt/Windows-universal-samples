<!---
  category: Data
  samplefwlink: http://go.microsoft.com/fwlink/p/?LinkId=620565
--->

# Logging sample

Shows how to use the Logging APIs in the
Windows.Foundation.Diagnostics namespace, including LoggingChannel,
LoggingActivity, LoggingSession, and FileLoggingSession. These classes are
designed for diagnostic logging within a modern application. 

> **Note:** This sample is part of a large collection of UWP feature samples. 
> If you are unfamiliar with Git and GitHub, you can download the entire collection as a 
> [ZIP file](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip), but be 
> sure to unzip everything to access shared dependencies. For more info on working with the ZIP file, 
> the samples collection, and GitHub, see [Get the UWP samples from GitHub](https://aka.ms/ovu2uq). 
> For more samples, see the [Samples portal](https://aka.ms/winsamples) on the Windows Dev Center. 

These APIs were
added in Windows 8.1. The LoggingChannel and LoggingActivity APIs have been
extended in Windows 10 to support writing complex events using TraceLogging
event encoding.

- **LoggingChannel:** The LoggingChannel class is used to generate events.
  The core LoggingChannel APIs can create simple events - events with
  a name and a string value, or events with a name, a string value, and an
  integer value. Starting with Windows 10, the LoggingChannel class can use
  TraceLogging event encoding to create complex events with arbitrary
  structured data.
- **LoggingActivity:** The LoggingActivity class is used to encapsulate an
  activity by writing a Start event when the activity is created and a Stop
  event when the activity is closed. Starting with Windows 10, the
  LoggingActivity class can use TraceLogging event encoding to write
  complex events associated with the activity and to support nested activities.
- **LoggingSession:** The LoggingSession class captures events into an
  in-memory circular buffer with the ability to save the buffer contents to a
  log file on-demand.
- **FileLoggingSession:** The FileLoggingSession class captures events directly
  to a sequence of log files, switching to a new log file when the maximum file
  size is reached.

The Logging classes are based on Windows ETW APIs. Events from these classes
can be captured using ETW tools such as xperf. The log files are generated in
ETL format so they can be viewed and processed by the Windows Performance
Toolkit (WPT), as well as utilities such as tracerpt.exe or xperf.exe.

**Note** The Windows universal samples require Visual Studio 2017 to build and Windows 10 to execute.
 
To obtain information about Windows 10 development, go to the [Windows Dev Center](http://go.microsoft.com/fwlink/?LinkID=532421)

To obtain information about Microsoft Visual Studio and the tools for developing Windows apps, go to [Visual Studio](http://go.microsoft.com/fwlink/?LinkID=532422)

## How to capture logging output

You can collect the events generated by the logging classes with xperf or another
ETL controller tool. To collect these events in an ETL file, first start the capture
session by running the command

    xperf -start MySession -f MyFile.etl -on Id

where the Id is given below.

Next, run the sample and run the appropriate scenario.

When done, run the command

    xperf -stop MySession

After collecting the ETL file, you can decode the trace using xperf, wpa,
or tracerpt. For example, to decode MyFile.etl with tracerpt, run the command

    tracerpt MyFile.etl

This generates the file `dumpfile.xml`.

Note that decoding TraceLogging events requires Windows 10. Earlier versions
of Windows can only reliably decode the simple (manifest-based) events.

### Windows 8.1 style events

If you use the `LoggingChannel` class's one-parameter constructor,
the result is a Windows 8.1-style logging channel.

* The channel's ETW Id is `4bd2826e-54a1-4ba9-bf63-92b73ea1ac4a`
* The channel's ETW Name is `Microsoft-Windows-Diagnostics-LoggingChannel`
* A LoggingChannelName field containing the Channel Name is automatically added to each event.
* Simple events will be written using manifested-event encoding.

Therefore, you would use the command line

    xperf -start MySession -f MyFile.etl -on 4bd2826e-54a1-4ba9-bf63-92b73ea1ac4a

to start capturing events with xperf.

Note that complex events can be written even when using Windows 8.1 mode.
Complex events always use TraceLogging encoding.

The 1-parameter constructor is marked as obsolete to ensure that
developers are aware of the changes in semantics. The Windows 10 semantics
are useful because they enable use of the ETW Provider Id for event
filtering.

### Windows 10 style events

If you use the `LoggingChannel` class's two-parameter or three-parameter constructor,
the result is a Windows 10-style logging channel.

* The channel's ETW Id is specified by the third constructor parameter, if present; otherwise it is generated from the Channel Name by the same hashing algorithm as the .NET EventSource class.
* The channel's ETW Name is specified by the first constructor parameter.

In the sample, the channel name is "SampleProvider", which hashes to `eff1e128-4903-5093-096a-bdc29b38456f`.

Therefore, you would use the command line

    xperf -start MySession -f MyFile.etl -on eff1e128-4903-5093-096a-bdc29b38456f

to start capturing events with xperf.

xperf version 10.0.16299 and higher support specifying the channel name with a leading asterisk:

    xperf -start MySession -f MyFile.etl -on *SampleProvider

## Related topics

### Samples

[Logging Sample](/Samples/Logging)  

### Reference

[LoggingChannel](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.aspx)  
[LoggingActivity](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.aspx)  
[LoggingSession](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingsession.aspx)  
[FileLoggingSession](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.fileloggingsession.aspx)  
[Windows Performance Toolkit](https://docs.microsoft.com/en-us/windows-hardware/test/wpt/index)  

## System requirements

**Client:** Windows 10

**Server:** Windows Server 2016 Technical Preview

**Phone:** Windows 10

## Build the sample

1. If you download the samples ZIP, be sure to unzip the entire archive, not just the folder with the sample you want to build. 
2. Start Microsoft Visual Studio 2017 and select **File** \> **Open** \> **Project/Solution**.
3. Starting in the folder where you unzipped the samples, go to the Samples subfolder, then the subfolder for this specific sample, then the subfolder for your preferred language (C++, C#, or JavaScript). Double-click the Visual Studio Solution (.sln) file.
4. Press Ctrl+Shift+B, or select **Build** \> **Build Solution**.

## Run the sample

The next steps depend on whether you just want to deploy the sample or you want to both deploy and run it.

### Deploying the sample

- Select Build > Deploy Solution. 

### Deploying and running the sample

- To debug the sample and then run it, press F5 or select Debug >  Start Debugging. To run the sample without debugging, press Ctrl+F5 or selectDebug > Start Without Debugging. 
