# Reusing Mediation Sequences

## What you'll build

In this sample scenario, you will use a **Sequence Template**
and reuse it in multiple places of the medation flow. You can reuse the
mediation flow that was defined in the [Sending a Simple Message to a Service](../sending-a-simple-message-to-a-service) tutorial and then replace its sections with the sequence template . See [Creating Templates](../../develop/creating-artifacts/creating-sequence-templates.md) for details
on how to work with templates using WSO2 Integration Studio.

## Let's get started!

### Step 1: Set up the workspace

Set up WSO2 Integration Studio as follows:

1.  Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
2.  If you did not try the [Exposing Several Services as a Single Service](exposing-several-services-as-a-single-service.md) tutorial yet:
    1.  Open WSO2 Integration Studio and go to **File -> Import**. 
    2.  Select **Existing WSO2 Projects into workspace** under the **WSO2** category, click **Next**, and then upload the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/ExposingSeveralServicesTutorial.zip).

Optionally, you can set up the **CLI tool** for artifact monitoring. This will later help you get details of the artifacts that you deploy in your Micro Integrator.

1.  Go to the [WSO2 Micro Integrator website](https://wso2.com/integration/#). 
2.  Click **Download -> Other Resources** and click **CLI Tooling** to download the tool. 
3.  Extract the downloaded ZIP file. This will be your `MI_CLI_HOME` directory. 
4.  Export the `MI_CLI_HOME/bin` directory path as an environment variable. This allows you to run the tool from any location on your computer using the `mi` command. Read more about the [CLI tool](../../../administer-and-observe/using-the-command-line-interface).

### Step 2: Develop the integration artifacts

#### Create a Sequence Template

1.  Once you have exported the ESB project as described above, the project directory will appear with the artifacts as shown below.

    ![](../../assets/img/tutorials/sequence-temp-project-explorer.png)

2.  Right-click on **SampleServices** and navigate to **New -> Template** . The **New Template Artifact** dialog will open.
3.  Select the **Create a New Template** and click **Next**.
4.  Enter the following details and click **Finish**.
    <table>
        <tr>
            <th>Parameter</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Template Name</td>
            <td>HospitalRoutingSeq</td>
        </tr>
        <tr>
            <td>Template Type</td>
            <td>Sequence Template</td>
        </tr>
    </table>

    ![](https://lh6.googleusercontent.com/jlYENAKLNtl-psHJCLm1dG_ziOYkinAK755IrObL-xdK2KXYmCkMju76X957PeSMQ8Hn5Q5RfNRgv_Uq-wOjE6apTyTlU3-jhf0ZUylfuydaOTCp8EZPtWkQ4hS9_cLADSME168K)

5.  The template artifact will open in the canvas as shown below.

    ![](../../assets/img/tutorials/sequence-canvas-1.png)

6.  Open the **Properties** tab of the sequence template by clicking on
    the canvas (outside the sequence box).  

7.  Click the ![](../../assets/img/tutorials/plus-icon.png) icon
    to start adding parameters .

    ![](../../assets/img/tutorials/sequence-canvas-2.png) 

8.  In the **Template Parameter** dialog that opens, enter 'sethospital' as the parameter name and click **Finish** .

9.  Add a **Log** mediator to the sequence template as shown below. This
    will print a message indicating to which hospital a requested
    message is routed.

    ![](../../assets/img/tutorials/log-mediator-in-sequence.png) 

10. Open the **Properties** tab of the log mediator and specify the
    following:

    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Log Category</td>
            <td>INFO</td>
        </tr>
        <tr>
            <td>Log Level</td>
            <td>CUSTOM</td>
        </tr>
    </table>

11. Click the ![](../../assets/img/tutorials/plus-icon.png) icon
    to start defining a property. Then add the following details for the
    property:

    <table>
        <tr>
            <th>Property Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td> Name</td>
            <td>message</td>
        </tr>
        <tr>
            <td>Type</td>
            <td>EXPRESSION</td>
        </tr>
        <tr>
            <td>Property Expression</td>
            <td><code>fn:concat('Routing to ', get-property('Hospital')) </code></td>
        </tr>
    </table>

    We select EXPRESSION because the required properties for the log
    message must be extracted from the request, which we can do using an
    XPath expression.  

12. Add a **Property** mediator just after the **Log** mediator to store
    the value for uri.var.hospital.

    ![](https://lh6.googleusercontent.com/Q4ulnFRxB7JyfuTexWUkGz_BurFUn8K45OPKrKDy-fZ1dS9fbC3Y0CBTdCMcKanKdPD2Httx-7C6S746QEb96ixJ-y48On_IHgaxqG2FqnAQfM4JemIkN2EnSpoJgqvF2FGztgA-) 

13. With the **Property** mediator selected, access the **Properties**
    tab and enter the information given below:
    <table>
        <tr>
            <th>Property</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Property Name</td>
            <td>Select New Property</td>
        </tr>
        <tr>
            <td>New Property Name</td>
            <td>uri.var.hospital  </td>
        </tr>
        <tr>
            <td>URI Template</td>
            <td>Select set </td>
        </tr>
        <tr>
            <td>Value Type</td>
            <td>Select EXPRESSION</td>
        </tr>
        <tr>
            <td>Property Data Type</td>
            <td>Select STRING</td>
        </tr>
        <tr>
            <td>Value Expression</td>
            <td><code>$func:sethospital</code></td>
        </tr>
        <tr>
            <td>Description</td>
            <td>Set Hospital Variable</td>
        </tr>
    </table>

#### Update the mediation flow

1.  Open the design view of the HealthcareAPI.xml and delete 'GrandOak'
    **Log** mediator by right clicking the mediator and selecting
    **Delete from Model** .  
    ![](https://lh4.googleusercontent.com/6Lsp6WbAQJwjSbmsyWlKnIMkP6Q3cB9hiwzvGRkyFUtbZKteu--ePnwvaA2LqfD-7AyLqy6U1Im42We67PsSP4JhL41vlZ8nwTC8S2KrF11Hpfu365bBXhCNOZX8cMlgdBgGX7e-)

2.  Delete the 'Set Hospital Variable' **Property** mediator.

3.  Add a **Call Template** mediator to the sequence as shown below.  
    ![](https://lh6.googleusercontent.com/1SUm23IRnOH-vp_o0QWpSS3bhIawxhmsKN_1Y_PYyEaZ2k6_2tuDFeyyJtp18c8Q-p6mubMToqA6v4tyWDtBMgQcJdXet00Vtxa-IDnu7TTh5eSvqnEfSi9YEoxEhuE1yJO62w4B)

4.  Open the **Properties** tab of the **Call Template** mediator and
    select ' HospitalRoutingSeq' from the list of available templates.

5.  Click the ![](../../assets/img/tutorials/plus-icon.png) icon
    to start adding parameters. Enter the following parameter details
    and click **Finish** .

    <table>
        <tr>
            <th>Parameter</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Parameter Name</td>
            <td>sethospital</td>
        </tr>
        <tr>
            <td>Parameter Type</td>
            <td>value</td>
        </tr>
        <tr>
            <td>Value/Expression</td>
            <td>grandoaks</td>
        </tr>
    </table>

    ![](https://lh5.googleusercontent.com/Pu0UusC_42jmt_VQ2NEA1PdwOI8z2VyC7bDm7HSJ6iJgf9J8Jz4k87PZ2e9UAuT62FEHdpFXXGlXx5n78qtBBvQxEmQbDkMlg3lCfTIn5grDxKDaZW0XGItxqJ72XmC0_uE84gKO)

6.  Repeat the above steps to add **Call Templates** for 'Clemency' and
    'Pine Valley' hospitals. Add **clemency** and **pinevalley** as the
    respective parameter values.

7.  Save the configuration.

### Step 3: Package the artifacts

Package the artifacts in your composite application project (SampleServicesCompositeApplication project) to be able to deploy the artifacts in the server.

1.  Open the `pom.xml` file in the composite application project POM editor.
2.  Ensure that the following artifacts are selected in the POM file.

    -   `HealthcareAPI`
    -   `HospitalRoutingSeq`

3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab. 


### Step 5: Testing the use case

Let's test the use case by sending a simple client request that invokes the service.

#### Start the backend service

1. Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0-EI7.jar).
2. Open a terminal, navigate to the location where your saved the [back-end service](#step-1-set-up-the-workspace).
3. Execute the following command to start the service:

    ```bash
    java -jar Hospital-Service-2.0.0-EI7.jar
    ```

#### Get details of deployed artifacts (Optional)

Let's use the **CLI Tool** to find the URL of the REST API (that is deployed in the Micro integrator) to which you will send a request.

!!! Tip
    Be sure to set up the CLI tool for your work environment as explained in the [first step](#step-1-set-up-the-workspace) of this tutorial.

1.  Open a terminal and execute the following command to start the tool:
    ```bash
    mi
    ```
    
2.  Log in to the CLI tool. Let's use the server administrator user name and password:
    ```bash
    mi remote login admin admin
    ```

    You will receive the following message: *Login successful for remote: default!*

3.  Execute the following command to find the APIs deployed in the server:
    ```bash
    mi api show
    ```

    You will receive the following information:

    *NAME : HealthcareAPI*            
    *URL  : http://localhost:8290/healthcare*
     
Similarly, you can get details of Connectors as well as other artifacts deployed in the server. Read more about [using the CLI tool](../../../administer-and-observe/using-the-command-line-interface).

#### Send the client request

Send a simple request to invoke the service.

1.  Create a JSON file names `request.json` with the following request payload.

    ```json
    {
     "name": "John Doe",
     "dob": "1940-03-19",
     "ssn": "234-23-525",
     "address": "California",
     "phone": "8770586755",
     "email": "johndoe@gmail.com",
     "doctor": "thomas collins",
     "hospital": "grand oak community hospital",
     "cardNo": "7844481124110331",
     "appointment_date": "2025-04-02"
    }
    ```

2.  Open a command line terminal and execute the following command from
    the location where the request.json file you created is saved:

    ```bash
    curl -v -X POST --data @request.json http://localhost:8290/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

2.  You will see the response in the command line terminal.

    ```bash
    {"appointmentNo":2,"doctorName":"thomas collins","patient":"John Doe","actualFee":7000.0,"discount":20,"discounted":5600.0,"paymentID":"cc7e4c23-a66d-4d60-8b72-2cf9143c6335","status":"Settled"}
    ```
