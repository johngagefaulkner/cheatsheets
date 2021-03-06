# PrintUI.dll and PrintUIEntry
Helpful guide for installing, editing and removing local & network printers via command line.

## Usage
```cmd
rundll32 printui.dll,PrintUIEntry [options] [@commandfile]
```

### Options

 - `/a[file]` binary file name
 - `/b[name]` base printer name
 - `/c[name]` unc machine name if the action is on a remote machine
 - `/dl` delete local printer
 - `/dn` delete network printer connection
 - `/dd` delete printer driver
 - `/e` display printing preferences
 - `/f[file]` either inf file or output file
 - `/F[file]` location of an INF file that the INF file specified with /f may depend on
 - `/ga` add per machine printer connections (the connection will be propagated to the user upon logon)
 - `/ge` enum per machine printer connections
 - `/gd` delete per machine printer connections (the connection will be deleted upon user logon)
 - `/h[arch]` driver architecture one of the following, x86, x64 or Itanium
 - `/ia` install printer driver using inf file
 - `/id` install printer driver using add printer driver wizard
 - `/if` install printer using inf file
 - `/ii` install printer using add printer wizard with an inf file
 - `/il` install printer using add printer wizard
 - `/im` install printer using add printer wizard skiping network listed printers
 - `/in` add network printer connection
 - `/ip` install printer using network printer installation wizard
 - `/j[provider]` print provider name
 - `/k` print test page to specified printer, cannot be combined with command when installing a printer
 - `/l[path]` printer driver source path
 - `/m[model]` printer driver model name
 - `/n[name]` printer name
 - `/o` display printer queue view
 - `/p` display printer properties
 - `/q` quiet mode, do not display error messages
 - `/r[port]` port name
 - `/s` display server properties
 - `/Ss` Store printer settings into a file
 - `/Sr` Restore printer settings from a file
   Store or restore printer settings option flags that must be placed at the end of command:
   
    ```
	[2] PRINTER_INFO_2
	[7] PRINTER_INFO_7
	[c] Color Profile
	[d] PrinterData
	[s] Security descriptor
	[g] Global DevMode
	[m] Minimal settings
	[u] User DevMode
	[r] Resolve name conflicts
	[f] Force name
	[p] Resolve port
	[i] Driver name conflict
    ```
 - `/u` use the existing printer driver if it's already installed
 - `/t[#]` zero based index page to start on
 - `/v[version]` driver version one of the following, "Type 2 - Kernel Mode" or "Type 3 - User Mode"
 - `/w` prompt the user for a driver if specified driver is not found in the inf
 - `/y` set printer as the default
 - `/Xg` get printer settings
 - `/Xs` set printer settings
 - `/z` do not auto share this printer
 - `/Y` do not auto generate a printer name
 - `/K` changes the meaning of /h to accept 2, 3, 4 for x86, x64, or Itanium (respectively), and /v to accept 3 for "Type 3 - User Mode"
 - `/Z` share this printer, can only be used with the /if option
 - `/?` help this message
   @[file] command line argument file
 - `/Mw[message]` show a warning message before committing the command
 - `/Mq[message]` show a confirmation message before committing the command
 - `/W[flags]` specifies flags and switches for the wizards (for APW & APDW)
 - `/r` Make the wizards to be restart-able from the last page.
 - `/G[flags]` specifies global flags and switches
- `/W` Suppress setup driver warnings UI (super quiet mode)
 - `/R` force selected driver to replace exisiting driver

## Examples:
 - Run server properties: 
    - `rundll32 printui.dll,PrintUIEntry /s /t1 /c:\machine`
 - Run printer properties: 
    - `rundll32 printui.dll,PrintUIEntry /p /n:\machine\printer`
 - Run add printer wizard locally: 
    - `rundll32 printui.dll,PrintUIEntry /il `
 - Run add printer wizard on machine: 
    - `rundll32 printui.dll,PrintUIEntry /im /\\machine`
 - Run queue view: 
    - `rundll32 printui.dll,PrintUIEntry /o /n:\machine\printer`
 - Run inf install: 
    - `rundll32 printui.dll,PrintUIEntry /if /b "Test Printer" /f c:\infpath\infFile.inf /r "lpt1:" /m "Brother DCP-128C"`
 - Run inf install (with inf dependency). In the example, prnbr002.inf depends on ntprint.inf
    - `rundll32 printui.dll, PrintUIEntry /ia /m "Brother DCP-128C" /K /h x64 /v 3 /f "c:\infpath\prnbr002.inf" /F "c:\infpath\ntprint.inf"`
 - Run add printer wizard using inf: 
    - `rundll32 printui.dll,PrintUIEntry /ii /f c:\infpath\infFile.inf`
 - Add printer using inbox printer driver: 
    - `rundll32 printui.dll,PrintUIEntry /if /b "Test Printer" /r "lpt1:" /m "Brother DCP-128C"`
 - Add per machine printer connection (the connection will be propagated to the user upon logon): 
    - `rundll32 printui.dll,PrintUIEntry /ga /c:\machine /n:\machine\printer /j"LanMan Print Services"`
 - Delete per machine printer connection (the connection will be deleted upon user logon): 
    - `rundll32 printui.dll,PrintUIEntry /gd /c:\machine /n:\machine\printer`
 - Enumerate per machine printer connections: 
    - `rundll32 printui.dll,PrintUIEntry /ge /c:\machine`
 - Add printer driver using inf: 
    - `rundll32 printui.dll,PrintUIEntry /ia /c:\machine /m "Brother DCP-128C" /h "x86" /v "Type 3 - User Mode" /f c:\infpath\infFile.inf`
 - Add printer driver using inf: 
    - `rundll32 printui.dll,PrintUIEntry /ia /K /c:\machine /m "Brother DCP-128C" /h "x86" /v 3`
 - Add inbox printer driver: 
    - `rundll32 printui.dll,PrintUIEntry /ia /c:\machine /m "Brother DCP-128C" /h "Intel" /v "Type 3 - Kernel Mode"`
 - Remove printer driver: 
    - `rundll32 printui.dll,PrintUIEntry /dd /c:\machine /m "Brother DCP-128C" /h "x86" /v "Type 3 - User Mode"`
 - Remove printer driver: 
    - `rundll32 printui.dll,PrintUIEntry /dd /K /c:\machine /m "Brother DCP-128C" /h "x86" /v 3`
 - Set printer as default: 
    - `rundll32 printui.dll,PrintUIEntry /y /n "printer"`
 - Set printer comment: 
    - `rundll32 printui.dll,PrintUIEntry /Xs /n "printer" comment "My Cool Printer"`
 - Get printer settings: 
    - `rundll32 printui.dll,PrintUIEntry /Xg /n "printer"`
 - Get printer settings saving results in a file: 
    - `rundll32 printui.dll,PrintUIEntry /f "results.txt" /Xg /n "printer"`
 - Set printer settings command usage:
    - `rundll32 printui.dll,PrintUIEntry /Xs /n "printer" ?`
 - Store all printer settings into a file: 
    - `rundll32 printui.dll,PrintUIEntry /Ss /n "printer" /a "file.dat"`
 - Restore all printer settings from a file: 
    - `rundll32 printui.dll,PrintUIEntry /Sr /n "printer" /a "file.dat"`
 - Store printer information on level 2 into a file : 
    - `rundll32 printui.dll,PrintUIEntry /Ss /n "printer" /a "file.dat" 2`
 - Restore  from a file printer security descriptor: 
    - `rundll32 printui.dll,PrintUIEntry /Sr /n "printer" /a "file.dat" s`
 - Restore  from a file printer global devmode and printer data: 
    - `rundll32 printui.dll,PrintUIEntry /Sr /n "printer" /a "file.dat" g d`
 - Restore  from a file minimum settings and resolve port name: 
    - `rundll32 printui.dll,PrintUIEntry /Sr /n "printer" /a "file.dat" m p`
 - Enable Client Side Rendering for a printer: 
    - `rundll32 printui.dll,PrintUIEntry /Xs /n "printer" ClientSideRender enabled`
 - Disable Client Side Rendering for a printer: 
    - `rundll32 printui.dll,PrintUIEntry /Xs /n "printer" ClientSideRender disabled`
