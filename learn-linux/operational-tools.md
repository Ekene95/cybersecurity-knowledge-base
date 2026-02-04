# Operational & Monitoring Tools

Collection of data collection tools, trace utilities, and operational commands for enterprise monitoring systems (OBM, OMi, BSM).

## OA Data Collection

### Linux/Solaris/HPUX Monitored Nodes

```bash
# Navigate to OpC directory
cd /opt/OV/contrib/OpC

# Run data collector with -asp flag
./oa_data_collector.sh -asp

# Output file location
/var/opt/OV/tmp/oa_data_<timestamp>.tar.gz
```

### AIX Monitored Nodes

```bash
# Navigate to OpC directory
cd /usr/lpp/OV/contrib/OpC

# Run data collector
./oa_data_collector.sh -asp

# Output file
/var/opt/OV/tmp/oa_data_<timestamp>.tar.gz
```

### Windows Monitored Nodes

```cmd
REM Navigate to contrib directory
cd %OvInstallDir%\contrib\OpC

REM Run data collector
oa_data_collector_win.bat ADC

REM Zip the output folder
REM Location: %OvDataDir%\tmp\oa_data
```

**Source:** Enterprise monitoring system documentation

---

## Log Grabber Tool

### Windows

```cmd
REM Silent mode
%topaz_home%\tools\LogGrabber\go.bat

REM GUI mode
%topaz_home%\tools\LogGrabber\go-gui.bat
```

### Linux

```bash
# Run log grabber
/opt/HP/BSM/tools/LogGrabber/saveLogs.sh

# Creates file: 2009.05.15.05.58.42.logs.zip (timestamp-based)
```

### OPR-Checker Tool

```cmd
REM Windows
%TOPAZ-HOME%\opr\support\opr-checker.bat
```

---

## Management Pack for Infrastructure

### Content Manager Commands

#### Windows

```cmd
<obm_home>\opr\bin\contentmanager.bat -l -user admin
```

#### Linux

```bash
<obm_home>/opr/bin/contentmanager.sh -l -user admin
```

---

## Trace Configuration

### Agent Tracing

#### Enable Trace

```bash
# Enable comprehensive tracing on agent
ovconfchg -ns eaagt \
  -set OPC_TRACE TRUE \
  -set OPC_TRACE_AREA ALL,DEBUG \
  -set OPC_TRC_PROCS opcmona,opcmsga \
  -set OPC_DBG_PROCS opcmona,opcmsga \
  -set OPC_DBG_EXCLUDE_AREA MUX,QM \
  -set OPC_TRACE_TRUNC FALSE
```

**Process:**
1. Enable tracing (command above)
2. Send test messages (note exact messages and timestamps using `date`)
3. Let trace run for ~10 minutes
4. Monitor file size - stop if it gets too large

#### Disable Trace

```bash
# Clear all trace settings
ovconfchg -ns eaagt \
  -clear OPC_TRACE \
  -clear OPC_TRACE_AREA \
  -clear OPC_TRC_PROCS \
  -clear OPC_DBG_PROCS \
  -clear OPC_DBG_EXCLUDE_AREA \
  -clear OPC_TRACE_TRUNC
```

#### Collect Trace Files

**Linux:**
```bash
/var/opt/OV/log/System.txt
/var/opt/OV/tmp/OpC/trace
```

**Windows:**
```cmd
%ovdatadir%\log\System.txt
%ovdatadir%\tmp\OpC\trace
```

---

## LDAP Configuration

### Show LDAP Configuration

```cmd
<LocalDisk>:\HPBSM\opr\support>opr-support-utils.bat -showLdapConfig
```

### Check LDAP Connection

```cmd
<LocalDisk>:\HPBSM\opr\support>opr-support-utils.bat -checkLdap
```

---

## Event Flow Tracing

### Gateway Server (GWS) Tracing

#### Configuration Files

```
%TOPAZ_HOME%\conf\core\Tools\log4j\wde\opr-gateway.properties
```

Set: `loglevel=DEBUG`

#### OMi UI Method

Navigate to:
```
Administration > Setup and Maintenance > Infrastructure Settings
> select Application=Operations Management
> set "Event Flow Logging Mode" to "file"
```

#### Log Files

```
%TOPAZ_HOME%\log\wde\opr-gateway.log
%TOPAZ_HOME%\log\wde\opr-gateway-flowtrace.log
```

#### TraceGUI Method

```
Configure -> Product Area Logging -> GWS -> Event Flow -> Enable
```

### Data Processing Server (DPS) Tracing

#### Configuration Files

```
%TOPAZ_HOME%\conf\core\Tools\log4j\opr-backend\opr-backend.properties
```

Set: `loglevel=DEBUG`

#### Log Files

```
%TOPAZ_HOME%\log\opr-backend\opr-backend.log
%TOPAZ_HOME%\log\opr-backend\opr-flowtrace-backend.log
```

#### TraceGUI Method

```
Configure -> Product Area Logging -> DPS -> Event Flow -> Enable
```

---

## XPL Trace (Windows)

### Enable Trace

```cmd
REM Navigate to support directory
cd %OvInstallDir%\support

REM Configure trace
ovtrccfg.exe -app opcmsgi opcmsga ovbbccb ovcd -sink c:\temp\opcmsg.trc -gc c:\temp\opcmsg.tcf

REM Apply configuration
ovtrccfg.exe -cf c:\temp\opcmsg.tcf
```

### Disable Trace

```cmd
cd %OvInstallDir%\support\
ovtrccfg.exe -off
```

### Simplified Version

```cmd
REM Enable
cd %OvInstallDir%\support
ovtrccfg.exe -app opcmsga -sink c:\temp\opcmsga.trc

REM Reproduce issue

REM Disable
ovtrccfg.exe -off
```

---

## XPL Trace (Linux)

### Enable Trace

```bash
# Set PATH
export PATH=$PATH:/opt/OV/support

# Disable any existing trace
ovtrccfg -off

# Enable trace (separate process names by space)
ovtrccfg -app opcmona opcmsga -sink /tmp/trace.trc

# Reproduce the problem

# Disable tracing
ovtrccfg -off
```

---

## Best Practices

### Trace Collection

1. **Before enabling:**
   - Note current time: `date`
   - Document the issue you're troubleshooting
   - Check available disk space

2. **During tracing:**
   - Reproduce the issue
   - Note exact timestamps of test actions
   - Monitor trace file size
   - Limit run time to ~10 minutes unless needed

3. **After tracing:**
   - Disable trace immediately
   - Collect all relevant log files
   - Document what was tested

### File Collection

Always collect these alongside traces:
- System logs
- Application logs
- Configuration files
- Timestamp documentation
- Issue description

### Windows vs Linux Paths

| Component | Windows | Linux |
|-----------|---------|-------|
| Install Dir | `%OvInstallDir%` | `/opt/OV` |
| Data Dir | `%OvDataDir%` | `/var/opt/OV` |
| Log Dir | `%OvDataDir%\log` | `/var/opt/OV/log` |
| Temp Dir | `%OvDataDir%\tmp` | `/var/opt/OV/tmp` |

## Troubleshooting Tips

### High Trace File Size

```bash
# Monitor trace file growth
watch -n 1 ls -lh /var/opt/OV/tmp/OpC/trace

# Limit trace to specific components if too large
ovconfchg -ns eaagt -set OPC_TRACE_AREA specific_component,DEBUG
```

### Trace Not Generated

1. Check permissions
2. Verify paths exist
3. Ensure process names are correct
4. Check disk space

### Policy Deployment Issues

```bash
# Check policy status
ovpolicy -list

# Force policy update
ovpolicy -force
```

## See Also

- [Services](services.md) - Service management and monitoring
- [File Manipulation](file-manipulation.md) - Log analysis techniques
- [Finding Files](finding-files.md) - Locating log and trace files
