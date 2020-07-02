# Managing and Running Images Using the EC2 Image Builder Console<a name="managing-image-builder-console"></a>

This section contains information to help you manage and run images with EC2 Image Builder using the Image Builder console\.

**Topics**
+ [Edit Configuration Details and Additional Settings](#image-builder-configuration-details)
+ [Delete Pipeline](#image-builder-delete-pipeline)
+ [Create New Component](#image-builder-create-component)
+ [Working with Image Recipes](#image-builder-recipes)
+ [Testing](#image-builder-testing)
+ [Distribution](#image-builder-distribution)
+ [Sharing Resources](#image-builder-distribution)
+ [Compliance](#image-builder-compliance)

## Edit Configuration Details and Additional Settings<a name="image-builder-configuration-details"></a>

After you have created an image pipeline, you can edit its configuration details and additional settings\. To edit the configuration details and additional settings, use the following steps\. 

1. To edit configuration details, including description, build schedule, or infrastructure details, navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to edit\. Then select the **View details** button\. 

1. On the **Image pipeline details** page, note the **Summary** details of the image pipeline, and the **Output image**, **Configuration**, and **Image recipe** tabs\.

1. Under the **Configuration** tab, select **Edit** next to **Configuration details**\. 

1. On the **Edit configuration details** page, you can edit your configuration\. Then select **Save changes**\. 

1. To edit additional settings, including associated licenses, AMI distribution settings, image export settings, and SNS notification settings, navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to edit\. Select **View details**\. 
**Note**  
License Manager settings will not replicate across AWS Regions that must be enabled in your account, for example, between the `ap-east-1` \(HKG\) and the `me-south-1` \(BAH\) Regions\. 

1. On the **Image pipeline detail** page, under the **Configuration** tab, select **Edit** next to **Additional settings**\. 

1. On the **Edit additional settings** page, you can edit your configuration\. Then select **Save changes**\.

After an image recipe has been created, its components cannot be modified or replaced\. If you want to update the components in an image recipe, create a new recipe or recipe version\. 

## Delete Pipeline<a name="image-builder-delete-pipeline"></a>

To delete an image pipeline, use the following steps\. 

1. Navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to delete\. 

1. From the **Actions** dropdown, select **Delete**\.

1. You will be prompted to confirm deletion of the pipeline by entering **Delete** in the text box and selecting **Delete**\. 

When you delete your pipeline, its associated resources are disassociated and the configuration is deleted\. 

**Note**  
Image pipeline
Infrastructure configuration/Distribution configuration/Image recipe
Component
Image

## Create New Component<a name="image-builder-create-component"></a>

You can create components to add to your image recipes\. Components include build components and test components\. Build components are orchestration documents that define a sequence of steps for downloading, installing, and configuring software packages\. They also define validation and security hardening steps\. Test components are orchestration documents that define tests to run on software packages\. A component is defined using a YAML document format\. After an image recipe has been created, its components cannot be modified or replaced\. To update components after an image recipe has been created you must create a new recipe or recipe version\. To create a new component, use the following steps\.

1. Select **Components** in the left navigation pane\. Then select **Create component**\.

1. On the **Create component** page, under **Component details**, enter the following:
   + **Image Operating system \(OS\)**\. Specify the OS with which the component is compatible\. 
   + **Component category**\. From the dropdown, select the type of build or test component you are creating\. 
   + **Component name**\. Enter a name for the component\.
   + **Component version**\. Enter the version number of the component\. 
   + **Description**\. Provide an optional description to help you identify the component\.
   + **Change description**\. Provide an optional description to help you understand the changes made to this version of the component\. 

1. Under **Definition document**, which is the document that defines the actions that Image Builder performs on your image, enter the document content in YAML format in the provided field\. You can optionally use the AWS\-provided example \(auto\-filled when you select **Use example**\) and edit the content inline\. 

1. After you have entered the component details, select **Create component**\. When you [create a recipe](#create-recipe-version) or create a pipeline, the component will be available in the **Component** dropdown\.

1. To delete a component, from the **Components** page, select the check box next to the component that you want to delete\. From the **Actions** dropdown, select **Delete component**\. 

You can create a new component version by selecting the check box next to the component and, under the **Actions** dropdown, select **Create new version**\. This will take you to the **Create Component** page, where you can create a new component version\. 

## Working with Image Recipes<a name="image-builder-recipes"></a>

After you have created an image recipe, you can manage it from the **Recipes** page in the EC2 Image Builder console\. You can either delete an image recipe or create a new version of a recipe\. 

### Delete an Image Recipe<a name="delete-recipe"></a>

To delete an image recipe, select the check box next to the image recipe and, under the **Actions** dropdown, select **Delete recipe**\. When you delete an image recipe, images and components associated with the image recipe are disassociated, and the configuration is deleted\. A dialog box prompts you to confirm the deletion by entering **delete**\. 

**Note**  
Image pipeline
Infrastructure configuration/Distribution configuration/Image recipe
Component
Image

### Create a New Image Recipe Version<a name="create-recipe-version"></a>

To create a new image recipe version, select the check box next to the image recipe and, under the **Actions** dropdown, select **Create new version**\. This takes you to the **Create Recipe **page, where you can create a new image recipe version\. For instructions for creating an image recipe, see the steps under [Build and automate an operating system image deployment using the EC2 Image Builder console](image-builder-image-deployment-console.md)\.

## Testing<a name="image-builder-testing"></a>

Generally, each test consists of a test script, a test binary, and test metadata\. The test script contains the orchestration commands to start the test binary, which can be written in any language supported by the OS\. Exit status codes indicate the test outcome\. Test metadata describes the test and its behavior \(for example, the name, description, paths to test binary, and expected duration\)\.

To update the tests in an image recipe using the EC2 Image Builder console, follow the steps to [create a new recipe version](#create-recipe-version), and then update the **Test Components **under **Components**\. 

## Distribution<a name="image-builder-distribution"></a>

EC2 Image Builder can distribute AMIs to any AWS Region\. The AMI is copied to each Region that you specify in the account used to build the image\. You can define AMI launch permissions to control which AWS accounts are permitted to launch EC2 instances with the created AMI\. For example, you can make the image private, public, or share with specific accounts\. If you both distribute the AMI to other Regions and define launch permissions for other accounts, the launch permissions are propagated to the AMIs in all of the Regions in which the AMI is distributed\. 

To update your distribution settings using the EC2 Image Builder console, follow the steps to [create a new recipe version](#create-recipe-version), and on the **Configure additional settings** page, update the **AWS Regions** and/or **Launch permissions** under **AMI Distribution settings**\. 

## Sharing Resources<a name="image-builder-distribution"></a>

To share components, image recipes, or images with other accounts or within AWS Organizations, see [Resource Sharing in EC2 Image Builder](image-builder-resource-sharing.md)\.

## Compliance<a name="image-builder-compliance"></a>

For CIS, EC2 Image Builder uses Amazon Inspector to perform automatic assessments for exposure, vulnerabilities, and deviations from best practices and compliance standards\. For example, it assesses unintended network accessibility, unpatched CVEs, public internet connectivity, and remote root login enablement\. Amazon Inspector is offered as a test component that you can choose to add to your image recipe\. \. For hardening, EC2 Image Builder validates using STIG\. For a complete list of STIG components available through Image Builder, see [EC2 Image Builder STIG Components](image-builder-stig.md)\. For more information, see [Center for Internet Security \(CIS\) Benchmarks](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_cis.html) and [Amazon EC2 Windows Server AMIs for STIG Compliance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ami-windows-stig.html)\.