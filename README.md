# DriveWorks Live - Integration Theme Example - Using Constants
### Version: 1.0.1
#### Minimum DriveWorks Version: 18.0

A distributable example that demonstrates getting and setting Constants using the Integration Theme, to pass data.

---

Please note: DriveWorks are not accepting pull requests for this example.  
Join our [online community](https://my.driveworks.co.uk) for discussion, resources and to suggest other examples.

---

### In this example:

`index.html`
- Creates a new Specification.
- Renders a DriveWorks Form (required to register Constant change delegate)
- Retrieves all accessible Constants, outputs values (controlled via `DriveWorksConfigUser.xml`)
- Updates the value of a Constant (`config.constantName`).
- Automatically reacts to Constant changes, outputs new values.

`/send-json/`
- Creates a new Specification (un-rendered)
- Constructs a JSON object from multiple data sources.
- Sets JSON object as the value of a single Constant (`config.constantName`).
- Transitions Specification to "Save" state (to easily view data passed)
- Provides help and example rules to extract the data sent within DriveWorks Rules.

`/batch-update/`
- Creates a new Specification (un-rendered)
- Constructs an array (`constants`), containing details of each Constant to update (name/value).
    - By default, this array contains example Constants (e.g. `ConstantOne`) for demonstration, but these can be modified as required.
- Updates all Constant values in a single 'batch' using `Promise.all()`, rather than a series of chained `await` statements.
    - Requests are sent in parallel, rather than in series - after each previous request completes.
    - Note: this allows the logic to continue on error, so potential errors should be handled accordingly.
- The Constants listed in this example should be exposed using `DriveWorksConfigUser.xml` - see 'Step 4' below.

### To use:
1. Clone this repository, or download as a .zip archive.

2. In `DriveWorksLiveConfigUser.xml`, ensure a Group Alias for a Group containing Constants is present e.g. `name="UsingConstants"`.
    * See [Integration Theme Connection Settings](https://docs.driveworkspro.com/Topic/IntegrationThemeSettings#Connection-Settings) for additional guidance.

3. Enter your Integration Theme details into `config.js`.
    * `serverUrl` - The URL that hosts your Integration Theme, including any ports.
    * `groupAlias` - The public alias created for the Group containing the DriveApps to render - as configured in `DriveWorksConfigUser.xml`.
        * This *must* be specified for each Group you wish to use.
        * This allows you to mask the true Group name publicly, if desired.
        * See [Integration Theme Connection Settings](https://docs.driveworkspro.com/Topic/IntegrationThemeSettings#Connection-Settings) for additional guidance.
    * `projectName` - The name of the Project to render a Specification from.
        * These is pre-filled with the supplied Project's name, but can be changed if required.
    * `constantName` - The name of the Constant to update.
        * Note: for the "Batch Update" example, the Constant names in the `constants` array are used instead.

4. To allow Constant values to be read and set, ensure they are exposed via `DriveWorksConfigUser.xml`
    * Constants can be exposed like so:
        ```
        <connections>
            <sharedGroupAlias OR individualGroupAlias ...>
                <permissions>
                    <project name="MyProjectName">
                        <constant name="ConstantOne">
                            <team name="Administrators" canEdit="true"/>
                            <team name="Guests"/>
                        </constant>
                        <constant name="ConstantTwo">
                            <team name="Guests"/>
                        </constant>
                    </project>
                </permissions>
            </sharedGroupAlias OR /individualGroupAlias>
        </connections>
        ```
    * For further help, see [Integration Theme Permission Settings](https://docs.driveworkspro.com/Topic/IntegrationThemeSettings#Permissions).
    * Note: the "Batch Update" example contains a list of pre-defined Constant names in the `constants` array e.g. "ConstantOne", "ConstantTwo".
        * This array can be modified to fit the configured Project - both names and values - or the defaults used.
        * The listed Constants should be exposed using `DriveWorksConfigUser.xml` to ensure that the example is functional.

5. Ensure that the Integration Theme server is running, using any of the available methods (e.g. Personal Web Edition, DriveWorks Live, IIS)
    * For more information, see [Selecting the Integration Theme](https://docs.driveworkspro.com/Topic/IntegrationThemeSelect).

6. Open the example HTML files locally (localhost) or on a remote server.
    * Ensure `<corsOrigins>` in `DriveWorksConfigUser.xml` permits requests from this location.
    * See [Integration Theme Configuration Settings](https://docs.driveworkspro.com/Topic/IntegrationThemeSettings#Configuration-Settings) for additional guidance.

7. If encountering any issues, check the browser's console for error messages. (F12)

### Potential Issues:
* When serving this example for a domain different to your DriveWorks Live server, e.g. api.my-site.com from company.com, 'SameSite' cookie warnings may be thrown when the Client SDK attempts to store the current session id.
    * This appears as "Error: 401 Unauthorized" in the browser console, even with the correct configuration set. 
    * To resolve:
        * Ensure you are running DriveWorks 18.2 or above, HTTPS is enabled in DriveWorks Live's settings and a valid SSL certificate has been configured via DriveWorksConfigUser.xml.
        * See [Integration Theme Settings](https://docs.driveworkspro.com/Topic/IntegrationThemeSettings) for additional guidance.

---

This source code has been made available to demonstrate how you can integrate with DriveWorks using the DriveWorks Live API.
This code is provided under the MIT license. For more details, see the included LICENSE file.

The example requires that you have the latest DriveWorks Live SDK installed, operational and remotely accessible.
