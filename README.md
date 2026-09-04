# Blazor Web App BasePath Validation

This repository contains samples for validating the .NET 11 **BasePath** component feature.

The feature was tested in the following scenarios:

1. **New .NET 11 App (Created and Published)**
2. **Upgraded App (.NET 10 to .NET 11) and Published**

### Repository Structure

- `net11-new-app/` contains samples used for validating newly created .NET 11 applications.
- `net10-to-net11-upgrade/` contains samples used for validating applications upgraded from .NET 10 to .NET 11.

---

## Running the Samples in Debug Mode

### Steps

1. Open the desired sample project in **Visual Studio 2026 Insiders**.
2. Run the application by pressing **F5** or selecting **Run**.
3. Verify that the application is launched in the browser.

> **Note:** For **InteractiveWebAssembly** samples, the **Server** project should be set as the startup project before execution.

---

## Deploying and Testing the Published Output in IIS

### Published Output Packages

The published output files are provided in the following **Evidence** folder locations:

- [New .NET 11 App - Published Binaries](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net11-new-app/PublishedBinaries.zip)
- [Upgraded .NET 10 to .NET 11 App - Published Binaries](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net10-to-net11-upgrade/PublishedBinaries.zip)

### Steps

1. Open **Internet Information Services (IIS) Manager**.

2. Right-click **Sites** and select **Add Website**.

3. Enter a site name and map the website to an empty folder, as the application is intended to be tested under a subpath for validating the BasePath component.

4. Configure the site binding:
   - Set **Type** to `https`.
   - Select an available **Port** (for example, `443`).
   - Select an SSL certificate.
     - A self-signed certificate can be used for local validation.
     - If a certificate is not available, create one and bind it to the website.

5. Click **OK** to create the website.

6. Right-click the created website and select **Add Application**.

7. Specify the required application alias (for example, `staticssr`, `interactiveserver`, or `interactivewebassembly`).

8. Map the application to the corresponding published output folder.

9. Select the required application pool.

10. Add the required host name.

11. Browse to the application using the HTTPS URL. For example:

    ```text
    https://blazorwebapp.basepathvalidation.com/staticssr
    ```

12. Verify that the BasePath component behaves correctly and that navigation, assets, and routing work as expected under the configured application path.

---

## Testing Behind a Reverse Proxy

A reverse proxy was used to expose the application through a different domain while forwarding requests to the backend application.

### Example

**Public URL (Reverse Proxy)**

```text
https://reverse.proxy.com/interactivewebassembly
```

**Backend Application URL**

```text
https://blazorwebapp.basepathvalidation.com/interactivewebassembly
```

**Reverse Proxy Rule**

The following IIS URL Rewrite rule was used:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="ReverseProxyInboundRule1" stopProcessing="true">
                    <match url="(.*)" />
                    <action type="Rewrite"
                            url="https://blazorwebapp.basepathvalidation.com/interactivewebassembly/{R:1}" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```

### Reverse Proxy Configuration files

The reverse proxy configurations can be extracted and used to reproduce the validation scenarios. The corresponding web.config files can be found in the following locations:

#### New .NET 11 App Scenario

- [Static SSR](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net11-new-app/ReverseProxy/StaticSSR.zip)
- [Interactive Server](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net11-new-app/ReverseProxy/InteractiveServer.zip)
- [Interactive WebAssembly](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net11-new-app/ReverseProxy/InteractiveWebAssembly.zip)

#### .NET 10 to .NET 11 Upgrade Scenario

- [Static SSR](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net10-to-net11-upgrade/ReverseProxy/StaticSSR.zip)
- [Interactive Server](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net10-to-net11-upgrade/ReverseProxy/InteractiveServer.zip)
- [Interactive WebAssembly](https://github.com/BharatRamsf3693/BlazorWebApp-BasePath-Validation/blob/main/Evidence/net10-to-net11-upgrade/ReverseProxy/InteractiveWebAssembly.zip)
