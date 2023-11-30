https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-installation-configuration.html

Winlogbeat is part of the Elastic Stack and is used for shipping Windows event logs to Elasticsearch. To install Winlogbeat via PowerShell, you can follow these steps:

1. **Download Winlogbeat:**
   Go to the official Elastic website to download the latest version of Winlogbeat: [Download Winlogbeat](https://www.elastic.co/downloads/beats/winlogbeat).

2. **Extract the Archive:**
   Once the download is complete, extract the contents of the archive to a location of your choice.

3. **Configure Winlogbeat:**
   Navigate to the extracted Winlogbeat directory and locate the `winlogbeat.yml` configuration file. Open it in a text editor and configure the necessary settings, such as Elasticsearch output settings.

   Here is a basic example of a configuration file:

   ```yaml
   winlogbeat.event_logs:
     - name: Application
     - name: Security
     - name: System

   output.elasticsearch:
     hosts: ["your-elasticsearch-host:9200"]
   ```

   Modify the `output.elasticsearch.hosts` setting to point to your Elasticsearch instance.

4. **Install Winlogbeat as a Service:**
   Open a PowerShell console with administrative privileges and navigate to the directory where Winlogbeat is extracted.

   Run the following command to install Winlogbeat as a service:

   ```powershell
   .\install-service-winlogbeat.ps1
   ```

   If you encounter an execution policy issue, you might need to set the execution policy temporarily to run the script. You can do this by running:

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Process
   ```

   After installation, you can revert the execution policy:

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Restricted -Scope Process
   ```

5. **Start the Winlogbeat Service:**
   Start the Winlogbeat service using the following command:

   ```powershell
   Start-Service winlogbeat
   ```

   You can check the status of the service with:

   ```powershell
   Get-Service winlogbeat
   ```

   Make sure the service is running without any errors.

Winlogbeat should now be installed and configured to ship Windows event logs to your Elasticsearch instance. Adjust the configuration based on your specific needs, especially in a production environment. Always refer to the official Winlogbeat documentation for the most up-to-date information: [Winlogbeat Documentation](https://www.elastic.co/guide/en/beats/winlogbeat/current/index.html).