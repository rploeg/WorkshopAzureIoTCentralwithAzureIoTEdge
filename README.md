# Workshop - Integrating Azure IoT Edge with Azure IoT Central

-- this will only work with the preview templates in Azure IoT Central --

This tutorial shows you how to add and configure an Azure IoT Edge device to your Azure IoT Central application. In this tutorial, we chose an IoT Edge-enabled Linux VM from Azure Marketplace.

This tutorial is made up of two parts:

* First, as an operator, you learn how to do cloud first provisioning of an IoT Edge device.
* Then, you learn how to do "device first" provisioning of an IoT Edge device.

In this tutorial, you learn how to:

> [!div class="checklist"]
>
> * Add a new IoT Edge device
> * Configure the IoT Edge device to help provision by using a shared access signature (SAS) key
> * View dashboards and module health in IoT Central
> * Send commands to a module running on the IoT Edge device
> * Set properties on a module running on the IoT Edge device

## Prerequisites

To complete this tutorial, you need an Azure IoT Central application. Follow [this quickstart to create an Azure IoT Central application](https://docs.microsoft.com/en-us/azure/iot-central/preview/quick-deploy-iot-central).

## Enable Azure IoT Edge enrollment group
From the **Administration** page, enable SAS keys for Azure IoT Edge enrollment group.

![Screenshot of Administration page, with Device connection highlighted](./images/groupenrollment.png)

## Provision a "cloud first" Azure IoT Edge device	
In this section, you create a new IoT Edge device by using the environment sensor template, and you provision a device. 
Select **Devices** > **Environment Sensor Template**. 

![Screenshot of Devices page, with Environment Sensor Template highlighted](./images/deviceexplorer.png)

Select **+ New**, and enter a device ID and name of your choosing. Select **Create**.

![Screenshot of Create new device dialog box, with Device ID and Create highlighted](./images/cfdevicecredentials.png)

The device goes into **Registered** mode.

![Screenshot of Environment Sensor Template page, with Device status highlighted](./images/cfregistered.png)

## Deploy an IoT Edge enabled Linux VM

> [!NOTE]
> You can choose to use any machine or device. The operating system can be Linux or Windows.

For this tutorial, we're using an Azure IoT enabled Linux VM, created on Azure. In [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview), select **GET IT NOW**. 

![Screenshot of Azure Marketplace, with GET IT NOW highlighted](./images/cfmarketplace.png)

Select **Continue**.

![Screenshot of Create this app in Azure dialog box, with Continue highlighted](./images/cfmarketplacecontinue.png)


You're taken to the Azure portal. Select **Create**.

![Screenshot of the Azure portal, with Create highlighted](./images/cfubuntu.png)

Select **Subscription**, create a new resource group, and select **(US) West US 2** for VM availability. Then, enter user and password information. These will be required for future steps, so remember them. Select **Review + create**.

![Screenshot of Create a virtual machine details page, with various options highlighted](./images/cfvm.png)

After validation, select **Create**.

![Screenshot of Create a virtual machine page, with Validation passed and Create highlighted](./images/cfvmvalidated.png)

It takes a few minutes to create the resources. Select **Go to resource**.

![Screenshot of deployment completion page, with Go to resource highlighted](./images/cfvmdeploymentcomplete.png)

### Provision VM as an IoT Edge device 

Under **Support + troubleshooting**, select **Serial console**.

![Screenshot of Support + troubleshooting options, with Serial console highlighted](./images/cfserialconsole.png)

You'll see a screen similar to the following:

![Screenshot of console](./images/cfconsole.png)

Press Enter, provide the user name and password as prompted, and then press Enter again. 

![Screenshot of console](./images/cfconsolelogin.png)

To run a command as administrator (user "root"), enter: **sudo su –**

![Screenshot of console](./images/cfsudo.png)

Check the IoT Edge runtime version. At the time of this writing, the current GA version is 1.0.8.

![Screenshot of console](./images/cfconsoleversion.png)

Install the vim editor, or use nano if you prefer. 

![Screenshot of console](./images/cfconsolevim.png)

![Screenshot of console](./images/cfvim.png)

Edit the IoT Edge config.yaml file.

![Screenshot of console](./images/cfconsoleconfig.png)

Scroll down, and comment out the connection string portion of the yaml file. 

**Before**

![Screenshot of console](./images/cfmanualprovisioning.png)

**After** (Press Esc, and press lowercase a, to start editing.)

![Screenshot of console](./images/cfmanualprovisioningcomments.png)

Uncomment the symmetric key portion of the yaml file. 

**Before**

![Screenshot of console](./images/cfconsolesymmcomments.png)

**After**

![Screenshot of console](./images/cfconsolesymmuncomments.png)

Go to IoT Central. Get the scope ID, device ID, and symmetric key of the IoT Edge device.
![Screenshot of IoT Central, with various device connection options highlighted](./images/cfdeviceconnect.png)

Go to the Linux computer, and replace the scope ID and registration ID with the device ID and symmetric key.

Press Esc, and type **:wq!**. Press Enter to save your changes.

Restart IoT Edge to process your changes, and press Enter.

![Screenshot of console](./images/cfrestart.png)

Type **iotedge list**. After a few minutes, you'll see three modules deployed.

![Screenshot of console](./images/cfconsolemodulelist.png)


## IoT Central device explorer 

In IoT Central, your device moves into provisioned state.

![Screenshot of IoT Central Devices options, with Device status highlighted](./images/cfprovisioned.png)

The **Modules** tab shows the status of the device and module on IoT Central. 

![Screenshot of IoT Central Modules tab](./images/cfiotcmodulestatus.png)


You'll see cloud properties in a form, from the device template you created in the previous steps. Enter values, and select **Save**. 

![Screenshot of My Linux Edge Device form](./images/deviceinfo.png)

Here's a view presented in the form of a dashboard tile.

![Screenshot of My Linux Edge Device dashboard tiles](./images/dashboard.png)