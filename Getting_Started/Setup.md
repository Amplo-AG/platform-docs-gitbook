# Platform Setup
This page describes how you can get started as quick as possible!

## Login & Team Environment
You can easily sign up at https://portal.amplo.ch. Make sure you use your business email,
as this will be used to recognize your colleagues. After verifying your account, the first
colleague to register enters the setup screen. Here he/she sets up the machines and incidents.


## Machines & Incidents
Within the Smart Maintenance Platform, you have separated environments for each type of machine,
with their own data connectors, processors, incidents and models.
Every machine you add will have its own list of incidents. Note that these incidents will
have their all have their own model, and therefore define what is being predicted.

> **_NOTE:_**  
    When your machines have many custom elements or models, the level of segregation may be
    difficult to determine. Try to segregate machines that have different sensor measurements,
    data processing steps, incidents or key indicators for root cause while limiting the total
    number of machines. Every added machine requires its own set of training data, integration
    and setup.

> **_NOTE:_** 
    The level of segregation for incidents may also not be straight forward. In general,
    we advise to define the incidents as per resolution. So if two different incidents are
    solved by the same resolution, it is smart to combine the two into one resolution, e.g.
    full sub-system replacement.


## Data Integration

Though all functionality of the platform is available without integrating a single element,
the true power only becomes apparent when integrated deeply into your existing workflows
and IT landscape. It automates manual steps, saving time while gaining robustness and consistency.
We have three types of data integrations, `Storage Connections`, `Event Driven` and `Data Streams`. Event Driven data
connectors only send measurement data when a specific event is triggered, such as the creation
of a service ticket or a specific alarm. Data Streams is a continuous connection, where measurement
data is sent at a fixed interval (often once a second to once a minute).

We have three data integrations:

- [heading](#<Storage Connections>)
- [heading](#<Data Stream Connections>)
- [Event Driven](#<Event Driven Connections>)


### Storage Connections
With the purpose of creating the initial training data and getting a maximum amount of
operational data for anomaly detection, we have off the shelf data connectors for:

- [heading](#google-cloud-storage)
- [heading](#<Amazon Web Services S3>)
- [heading](#<Azure Blob Storage>)

> **_NOTE:_** 
    If your cloud vendor or storage service is not listed, we're more than happy to add a few names on our list!

All Storage Connections require some small setup. To do so, click on `Machine Data` in the `Integrations` section of the
menu. You see a card for Storage Connections with the various cloud services. Press the plus icon to set it up.


#### Google Cloud Storage

**1. Authentication**
First, you need to enable programmatic access to your bucket and create a Service Account with `storage.objects.get`
and `storage.buckets.get` permissions. Download the Service Key and add it as a file or the content as a raw JSON.
For more information, check the `Google Documentation <https://cloud.google.com/storage/docs/reference/libraries>`_.

**2. Bucket Name**
The Service Key already contains your `Project ID`, so we only need the `Bucket Name` (or names in case of multiple) to
identify the location of your measurement data.

**3. Timestamp & Serial Identification**
In order to link the data to the ticket system to determine whether it's incident or healthy data, we need to establish
a a timestamp and serial number per file / sample. This can be done either by filename with regular expressions or by
column inside the data.

**4. Convert**
In case you use a specific data format (such as Avro or CAN), you have the option to add a small data processor.
Standard, we support CSV, JSON, XML, Feather, Parquet and Stata. In case you use these file formats, you don't need to
use specify this.

#### Amazon Web Services S3
**1. Authentication**
First of all, you must have programmatic access enabled. Secondly, you need to create an Access Key. Paste the
`Access Key ID`, `Secret Access Key` and `Session Token` into the appropriate fields in the setup screen.
For more information, check the `Amazon Documentation <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey>`_.

**2. Bucket Information**
Additionally, you need to provide your `Bucket Name` and `Region Name`.

**3. Timestamp & Serial Identification**
In order to link the data to the ticket system to determine whether it's incident or healthy data, we need to establish
a a timestamp and serial number per file / sample. This can be done either by filename with regular expressions or by
column inside the data.

**4. Convert**
In case you use a specific data format (such as Avro or CAN), you have the option to add a small data processor.
Standard, we support CSV, JSON, XML, Feather, Parquet and Stata. In case you use these file formats, you don't need to
use specify this.

#### Azure Blob Storage
**1. Authentication**
First, create a storage account and copy the `Connection String` as described in the Azure's documentation. Paste the
Connection String into the appropriate field in the setup screen. For more information, check the
`Azure Documentation <https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python>`_.

**2. Container Information**
Next, you need to provide your `Container Name`.

**3. Timestamp & Serial Identification**
In order to link the data to the ticket system to determine whether it's incident or healthy data, we need to establish
a a timestamp and serial number per file / sample. This can be done either by filename with regular expressions or by
column inside the data.

**4. Convert**
In case you use a specific data format (such as Avro or CAN), you have the option to add a small data processor.
Standard, we support CSV, JSON, XML, Feather, Parquet and Stata. In case you use these file formats, you don't need to
use specify this.

### Secure Storage Connections
To ensure a safe and secure Storage Connection, make sure to follow RBAC's best practices:

- Assign only permissions we need
- Rotate Account Keys
- Separate duties for account roles

### Data Stream Connections
Amplo supports various protocols to continuously send data to the platform. More specifically, we support [heading](#MQTT) 
and [heading](#Webhooks). These data stream connections are necessary for continuous services, such as 
[heading](../Educational/Services.md#<Predictive Maintenance>), 
[heading](../Educational/Services.md#<Anomaly Detection>) and 
[heading](../Educational/Services.md#<Condition Monitoring>).

#### MQTT
MQTT is a lightweight, binary internet protocol specifically designed for IoT devices. It uses certificates & private keys 
to authenticate the connection. Therefore, it is necessary to register all Machine Units on the platform and pull the 
specific certificates and keys. 

To set up a MQTT connection, go to Integration > Machine Data. Under Data Stream Endpoints, click 'Create new'. As it is
possible to have multiple streams per machine type, give your new stream an intuitive name. Select MQTT as the connection type 
and click create. The connection will appear directly, and if you click on your new connection, the Host, Port and Topic 
of our MQTT broker will be displayed. This, along with the private key and certificate, allows your machines to communicate
with the machine learning models! Last step is to determine the payload, which you can find more about [here](#Payload).

#### Webhooks
A webhook uses the common HTTP POST protocol to send data. Though not as lightweight, it is often easy to implement.
To set up a Webhook, go to Integration > Machine Data. Under Data Stream Endpoints, click 'Create new'. As it is
possible to have multiple streams per machine type, give your new stream an intuitive name. Select Webhook as the connection type 
and click create. The connection will appear directly, and if you click on your new connection, the Host, and api key are displayed. 
The API key needs to be set in the HTTP header `x-functions-key`. Last step is to determine the payload, which you can find more about [here](#Payload).

#### Payload
Though we accept any kind of data, we do need to know which Machine Unit send the data, and when it did so. Therefore, 
we use the following schema:

```
{
  "ts": [str] Timestamp string YYYY-mm-dd HH:MM:SS,
  "sn": [str | int] Serial number,
  "data": [object | str] can be a json with key/value pairs or text data,
}
```
```json
{
  'ts': '2022-01-01 00:00:00',
  'sn': '286969-01',
  'data': {
    "temperature_01": 29.521,
    "pressure_03": 450.321,
    "voltage_02": 809.20,
  }
}
```




> **_NOTE:_** 
    Additional costs are associated with Data Streams.


### Event Driven Connections
Event Driven Connection goes through APIs. Event Driven integration is only used for [Automated Diagnostics](../Educational/Services.md#<Automated Diagnostics>)
and therefore the `Diagnostics` endpoint can be used. Visit our [API Documentation](https://portal.amplo.ch/api-docs)
for more information. Your `Team Identifier` and `API Key` can be found under `Settings` > `API Access`.

## Ticket System Integration

For a fully integrated solution, it's recommended to integrate your Service Ticket System alongside a full Data
Integration. This allows Amplo to find historic tickets and organise your training data, notice newly commissioned
machines and automatically diagnose incoming issues that were missed by our continuous ML Services.
Out of the box we support the following integrations, but note that we allow custom integration as well, so feel free
to reach out.

- [Freshdesk](#Freshdesk)
- [Salesforce](#Salesforce) **Coming Soon**

> **_NOTE:_** 
    If your ticket system provider is not listed, feel free to reach out as we're more than happy to add some to our list!


### Freshdesk

**1. Authenticate Freshdesk's API**
Freshdesk uses an API Key for authentication. When setting up this integration on the platform, please copy the API Key.
You can find the API Key by:

1. Logging into your Freshdesk Support Portal
2. Clicking on your profile picture
3. Going to Profile Settings Page
4. The API Key is available below the change password section.

**2. Integrate ML Services**
You can directly integrate our ML Services with webhooks. This works through `Freshdesk Automations <https://support.freshdesk.com/support/solutions/articles/132589-using-webhooks-in-automation-rules-that-run-on-ticket-updates>`_.
You are relatively free to integrate anything with these webhooks. The most common use case is to trigger the
[Automated Diagnostics](../Educational/Services.md#<Automated Diagnostics>) service upon the creation of a ticket. To set this up, use the following settings:

- Trigger Webhook
- Request Type: Post
- URL: https://portal.amplo.ch/api/diagnosis
- Requires Authentication
- Enter the `your API Key <https://portal.amplo.ch/settings?api_access>`_
- Create the body according to our `API Documentation <https://portal.amplo.ch/api-docs>`_


.. _ref-salesforce:

### Salesforce
Salesforce understands very well that their users need customization. We have developed a custom Lightning Component.
We have purposely have not developed an application on Salesforce's AppExchange as this is dedicated to the creation
of intellectual property within Salesforce, whereas our intellectual property remains secure on our own servers.

**1. Authenticate Salesforce's OpenAPI**
In order to pull data from your Salesforce org, we use `Connected Apps`. 