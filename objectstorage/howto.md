# Object Storage How To Guides

<!--
  To update the TOC:
  * install nodejs (https://nodejs.org/en/)
  * run the following command:
    * npx doctoc@2.0.1 --github --notitle objectstorage/howto.md
-->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Create an instance](#create-an-instance)
- [Configure an instance](#configure-an-instance)
- [Delete an instance](#delete-an-instance)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## Create an instance

- Make sure that you're logged in to the management console.
- From the left panel, select **Object Storage** > **Buckets Stores**.
- From the right side, click on **Add Instance** button. A dialog box will
  appear.
- Select **Zone** from the drop-down list.
- Type in the **Instance Name**.
- Click on the **Add Instance** button.
- The new instance will appear on the right side.

Demo:

![Create Instance Demo](images/console/demo-create-instance.gif)

## Configure an instance

- Click on the **Open** button
  ![Open](images/console/btn-open.png) of the instance that
  you want to configure.
- A new panel will appear on the right side showing all the properties and
  configurations of the selected instance. The right expanded panel will include
  the following tabs/sub-menus:

  - INFO
  - CONFIGURE
  - CACHE
  - REPORTS
  - LOGS

- The expanded right panel of an instance can be closed by clicking on the
  **Close** button ![Close](images/console/btn-close.png).

Demo:

![Configure Instance](images/console/demo-configure-instance.gif)

## Delete an instance

- Click on the **Actions** button
  ![Actions](images/console/btn-actions.png) on the instance
  you want to delete.
- From the drop-down menu, select the **Terminate** menu option. A confirmation
  dialog box will appear.
- Check on the **Check to allow Termination** checkbox.
- Click on the **Terminate Instance** button.

Demo:

![Delete Instance Demo](images/console/demo-delete-instance.gif)
