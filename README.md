###What does it do?

<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClamAVScanReport-Big.png?raw=true">
[(click here to view an actual generated report)](http://danlynn.org)

Like many unix geeks, I’ve tooled up my desktop with lots of interesting custom shell scripts to enhance my user experience. One such enhancement that I’ve been using for a long time was a bash shell script that wrapped the execution of the ClamAV clamscan so that it could parse the log when the scan was completed and display a growl notification showing the summary info that appears at the end of the scan log.

This was fine, but it didn’t really give me the functionality that I needed or the information that I wanted. First of all, I wanted the ClamAV virus definitions database to be updated every night before the scan. There are solutions for setting up automatic updates. However, these occur on an independent schedule. I wanted to make sure that I had the very latest definitions before running the scan.

Secondly, the clamscan log is long and littered with tons of messages that you don’t really care about. Besides having to filter through all the debris to find just the files that have been flagged, I found that what I really wanted to know was what were the changes between this scan and the last. This is useful because many of the files flagged by ClamAV are emails which appear to be phishing scams. Since I usually don’t spend the time to delete these emails right away, it would be nice to have any newly flagged files hilited for me. Additionally, as long as we are hiliting new infections, we might as well hilite all the changed values found elsewhere on the new scan report.

In the image above, you can see that the first file in the list of infections is hilited in green. This tells us that it is a new infection. However, in the ‘infections count’ field above the tabs, we see that it too is hilited. Not only that, but the differing value from the previous scan appears next to it. Now this is the sort of diff information that I was looking for. Each of the infections that appear on this tab have file links that should open a file browser to the directory which contains the infected file. This is a handy way to get hold of that file for deletion or manual quarantining.

####Scan Info Tab
<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClamAVScanReport-ScanInfo.png?raw=true">

The Scan Info tab displays some summary info regarding the files scanned. It also lists the file extensions which were excluded from the scan (as configured in config/clamav.yml).

####Virus Definitions Tab
<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClamAVScanReport-VirusDefs.png?raw=true">

The Virus Definitions tab lists the total number of known viruses in the ClamAV database as well as the ClamAV engine version. If any error or info messages were generated during the virus definitions update then they will be displayed in a log at the bottom. In this instance, the definitions were already up to date (last 2 lines). However, the engine version is out of date (warnings in red). The engine can’t be updated by the clamav.rb script – so you will have to update the ClamAV engine manually when you get a chance.

####Clamscan Log Tab
<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClamAVScanReport-ClamscanLog.png?raw=true">

The Clamscan Log tab lists the full --quiet (less verbose) log generated by the clamscan process. Even with the --quiet option, clamscan is pretty verbose. My clamav.rb script takes the liberty of coloring the log so that all excluded/empty files are grayed out and the names of the viruses found are hilited in red. This information actually all appears on the other tabs. The log is just provided for completeness.

One log which doesn’t appear in the report is the clamav.rb run log. This log (found in log/run.log) contains only info and warnings that apply to the running of the clamav.rb report script itself. If something really bad happens and no report is generated then this log would be the place to check for clues as to what went wrong.

####Charts Tab
<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClamAVScanReport-Charts.png?raw=true">

The charts tab graphs the changes in the number infections against the changes in the number of new virus definitions. A correlation between the two may indicate that your jump in new infections reflects an existing problem that is just now being identified due to the new virus definitions. Non-correlated spikes reflect new infections (like new phishing emails or actual, new files containing viruses, or a spreading infection). Ideally, any spiked increase in infections would be immediately followed by a corresponding negative spike as the files which had been identified were disinfected or removed. Note that the reports and charts are based on previous runs on the same scan directory. Therefore, even though all scan data may be saved in the same database, you can have multiple configurations defined and scheduled which scan other drives or directories without having to worry about confusing any of the change tracking.

####Other Features
<img src="/danlynn/ClamAvReport/blob/master/docs/images/safari_icon_100x100.png?raw=true" style="float: right">Even though the scan report is generated dynamically by rails components, it is stored statically on disk as a regular HTML file so that it can be easily opened in your default browser each morning. A nice side benefit of this is that you can just bookmark this file location in your browser or drag it to your Dock for convenient access to the latest scan report.

<img src="/danlynn/ClamAvReport/blob/master/docs/images/ClockGreen-100.png?raw=true" style="float: left">As an added bonus, a simple command-line option has been provided for setting up ClamAV Scan Report to run on a daily schedule via an OSX LaunchAgent. See the following Installation and Configuration section to learn how to do this. In addition to this generated LaunchAgent, you may create additional ones to monitor certain directories and then report on any changes (eg: downloads, public, email).
