# Install to Windows using a ZIP file

:warning: **SignalFx Smart Agent is deprecated. For details, see the [Deprecation Notice](./smartagent-deprecation-notice.md)** :warning:

Install the SignalFx Smart Agent to a Windows host using a standalone package in
a ZIP file.

## Prerequisites

* Windows Server 2012 or higher
* Windows PowerShell access
* Windows decompression application, such as WinZip or the native Windows
  decompression feature.
* .NET Framework 3.5 or higher
* TLS 1.0 or 1.2
* Administrator account in which to run the Smart Agent

If you want to invoke a Python script for non-default monitors such as `exec`,
you must have Python 3 installed. The ZIP file includes the Python 3.8.0
runtime.

## Install using a ZIP file

1. Remove collector services such as `collectd`

2. To get the latest Windows standalone installation package, navigate to
   [Smart Agent releases](https://github.com/signalfx/signalfx-agent/releases)
   and download the following file:

   ```
   signalfx-agent-<latest_version>-win64.zip
   ```

   For example, if the latest version is **5.1.6**, follow these steps:

   1. In the **releases** section, find the section called **v5.1.6**.
   2. In the **Assets** section, click `signalfx-agent-5.1.6-win64.zip`
   3. The ZIP file starts downloading.

3. Uncompress the ZIP file. The
   package contents expand into the `signalfx-agent` directory.

## Configure the ZIP file installation

Navigate to the `signalfx-agent` directory, then create a configuration
file for the agent:

1. In a text editor, create a new file called signalfx-agent\agent-config.yml.
2. In the file, add your hostname and port number:

   ```
   internalStatusHost: <local_hostname>
   internalStatusPort: <local_port>
   ```

3. Save the file.

The Smart Agent collects metrics based on the settings in
`agent-config.yml`. The `internalStatusHost` and `internalStatusPort`
properties specify the host and port number of the host that's running the Smart Agent.

### Start the Smart Agent

To run the Smart Agent as a Windows program, run the following command in a console window:

  ```
  SignalFxAgent\bin\signalfx-agent.exe -config SignalFxAgent\agent-config.yml > <log_file>`
  ```

> The default log output for Smart Agent goes to `STDOUT` and `STDERR`.
> To persist log output, direct the log output to `<log_file>`.

To install the Smart Agent as a Windows service, run the following command in a console window:

  ```
  SignalFxAgent\bin\signalfx-agent.exe -service "install" -logEvents -config SignalFxAgent\agent-config.yml
  ```

To start the Smart Agent as a Windows service, run the following command in a console window:

  ```
  SignalFxAgent\bin\signalfx-agent.exe -service "start"
  ```

To learn about other Windows service options, see
[Service Configuration](https://docs.signalfx.com/en/latest/integrations/agent/windows.html#service-configuration).

### Verify the Smart Agent

To verify your installation and configuration, perform these steps:

* For infrastructure monitoring, perform these steps:
  1. In SignalFx, open the **Infrastructure** built-in dashboard
  2. In the override bar, select **Choose a host**. Select one of your hosts from the dropdown list.

  The charts display metrics from your infrastructure.

  To learn more, see [Built-In Dashboards and Charts](https://docs.signalfx.com/en/latest/getting-started/built-in-content/built-in-dashboards.html).

* For Kubernetes monitoring, perform these steps:
  1. In SignalFx, from the main menu select **Infrastructure** > **Kubernetes Navigator** > **Cluster map**.
  2. In the cluster display, find the cluster you installed.
  3. Click the magnification icon to view the nodes in the cluster.

  The detail pane on the right hand side of the page displays details of your cluster and nodes.

  To learn more, see [Getting Around the Kubernetes Navigator](https://docs.signalfx.com/en/latest/integrations/kubernetes/get-around-k8s-navigator.html)

* For APM monitoring, learn how to install, configure, and verify the Smart Agent for Microservices APM (**µAPM**). See
  [Get started with SignalFx µAPM](https://docs.signalfx.com/en/latest/apm/apm-getting-started/apm-index.html).

### Uninstall the Smart Agent

1. If the `signalfx-agent` service was installed, run the following PowerShell
   commands to stop and uninstall the service:
   ```powershell
   SignalFxAgent\bin\signalfx-agent.exe -service "stop"
   SignalFxAgent\bin\signalfx-agent.exe -service "uninstall"
   ```

1. Back up any files as necessary and delete the `SignalFxAgent` folder.
