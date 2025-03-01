<!--- OVERVIEW --->
# Quick Install

SignalFx Smart Agent is deprecated. For details, see the [Deprecation Notice](./smartagent-deprecation-notice.md).

SignalFx Smart Agent Integration installs the Smart Agent application on a single host machine from which you want to collect monitoring data. Smart Agent collects infrastructure monitoring, µAPM, and Kubernetes data.

For other installation options, including bulk deployments to production,
see [Install and Configure the Smart Agent](https://docs.signalfx.com/en/latest/integrations/agent/agent-install-methods.html).

## Installation

### Prerequisites

#### General
- Ensure that you've installed the applications and services you want to monitor on a Linux or Windows host. SignalFx doesn't support installing the Smart Agent on macOS or any other OS besides Linux and Windows.
- Uninstall or disable any previously-installed collector agents from your host, such as `collectd`.
- If you have any questions about compatibility between the Smart Agent and your host machine or its applications and services, contact your Splunk support representative.

#### Linux
- Ensure that you have access to `terminal` or a similar command line interface application.
- Ensure that your Linux username has permission to run the following commands:
    - `curl`
    - `sudo`
- Ensure that your machine is running Linux kernel version 3.2 or higher.

#### Windows
- Ensure that you have access to Windows PowerShell 6.
- Ensure that your machine is running Windows 8 or higher.
- Ensure that .Net Framework 3.5 or higher is installed.
- While SignalFx recommends that you use TLS 1.2, if you use TLS 1.0 and want to continue using TLS 1.0, then:
    - Ensure that you support the following ciphers:
        - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
        - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
        - TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
        - TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
        - TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
        - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
        - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (secp256r1) - A
        - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
        - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (secp256r1) - A
        - TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
        - TLS_RSA_WITH_AES_256_GCM_SHA384 (rsa 2048) - A
        - TLS_RSA_WITH_AES_128_CBC_SHA256 (rsa 2048) - A
    - See [Solving the TLS 1.0 Problem, 2nd Edition](https://docs.microsoft.com/en-us/security/engineering/solving-tls1-problem) for more information.

### Steps

#### Access the SignalFx UI

This content appears in both the documentation site and in the SignalFx UI.

If you are reading this content from the documentation site, please access the SignalFx UI so that you can paste pre-populated commands.

To access this content from the SignalFx UI:
1. In the SignalFx UI, in the top menu, click **Integrations**.
2. Locate and select **SignalFx SmartAgent**.
3. Click **Setup**, and continue reading the instructions.

#### Install the Smart Agent on Linux

This section lists the steps for installing the Smart Agent on Linux. If you want to install the Smart Agent on Windows, proceed to the next section, **Install SignalFx Smart Agent on Windows**.

Copy and paste the following code into your command line or terminal:

```sh
curl -sSL https://dl.signalfx.com/signalfx-agent.sh > /tmp/signalfx-agent.sh;
sudo sh /tmp/signalfx-agent.sh --realm YOUR_SIGNALFX_REALM -- YOUR_SIGNALFX_API_TOKEN
```

When this command finishes, it displays the following:

```
The SignalFx Agent has been successfully installed.

Make sure that your system's time is relatively accurate or else datapoints may not be accepted.

The agent's main configuration file is located at /etc/signalfx/agent.yaml.
```

If your installation succeeds, proceed to the section **Verify Your Installation**. Otherwise, see the section **Troubleshoot Your Installation**.

#### Install the Smart Agent on Windows

Copy and paste the following code into your Windows PowerShell terminal:

```sh
& {Set-ExecutionPolicy Bypass -Scope Process -Force; $script = ((New-Object System.Net.WebClient).DownloadString('https://dl.signalfx.com/signalfx-agent.ps1')); $params = @{access_token = "YOUR_SIGNALFX_API_TOKEN"; ingest_url = "https://ingest.YOUR_SIGNALFX_REALM.signalfx.com"; api_url = "https://api.YOUR_SIGNALFX_REALM.signalfx.com"}; Invoke-Command -ScriptBlock ([scriptblock]::Create(”. {$script} $(&{$args} @params)”))}
```

The agent files are installed to `\Program Files\SignalFx\SignalFxAgent`, and the default configuration file is installed at `\ProgramData\SignalFxAgent\agent.yaml` if it does not already exist.

The install script starts the agent as a Windows service that writes messages to the Windows Event Log.

If your installation succeeds, proceed to the section **Verify Your Installation**. Otherwise, see the section **Troubleshoot Your Installation**.

### Verify Your Installation

1. To verify that you've successfully installed the Smart Agent, copy and paste the following command into your terminal.

**For Linux:**

```sh
sudo signalfx-agent status
```

**For Windows:**

```sh
& ”\Program Files\SignalFx\SignalFxAgent\bin\signalfx-agent.exe” status
```

The command displays output that is similar to the following:

```sh
SignalFx Agent version:           5.1.0
Agent uptime:                     8m44s
Observers active:                 host
Active Monitors:                  16
Configured Monitors:              33
Discovered Endpoint Count:        6
Bad Monitor Config:               None
Global Dimensions:                {host: my-host-1}
Datapoints sent (last minute):    1614
Events Sent (last minute):        0
Trace Spans Sent (last minute):   0
```

2. To perform additional verification, you can run any of the following commands:

- Display the current Smart Agent configuration.

```sh
sudo signalfx-agent status config
```

- Show endpoints discovered by the Smart Agent.

```sh
sudo signalfx-agent status endpoints
```

- Show the Smart Agent's active monitors. These plugins poll apps and services to retrieve data.

```sh
sudo signalfx-agent status monitors
```

### Troubleshoot Smart Agent Installation
If the Smart Agent installation fails, use the following procedures to gather troubleshooting information.

#### General troubleshooting
To learn how to review signalfx-agent logs, see [Frequently Asked Questions](./faq.md).

#### Linux troubleshooting

To view recent error logs, run the following command in terminal or a similar application:

- For sysv/upstart hosts, run:

```sh
tail -f /var/log/signalfx-agent.log
```

- For systemd hosts, run:

```sh
sudo journalctl -u signalfx-agent -f
```

#### Windows troubleshooting
Open **Administrative Tools > Event Viewer > Windows Logs > Application** to view the `signalfx-agent` error logs.

### Uninstall the Smart Agent

#### Debian

To uninstall the Smart Agent on Debian-based distributions, run the following
command:

```sh
sudo dpkg --remove signalfx-agent
```

**Note:** Configuration files may persist in `/etc/signalfx`.

#### RPM

To uninstall the Smart Agent on RPM-based distributions, run the following
command:

```sh
sudo rpm -e signalfx-agent
```

**Note:** Configuration files may persist in `/etc/signalfx`.

#### Windows

The Smart Agent can be uninstalled from `Programs and Features` in the Windows
Control Panel.

**Note:** Configuration files may persist in `\ProgramData\SignalFxAgent`.
