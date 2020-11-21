# Create components<a name="image-builder-create-component"></a>

Components are software scripts that define the custom configuration for an image\. 

You can create the following types of components to add to your image recipes:

Build components  
Build components are software scripts that define a sequence of steps for downloading, installing, and configuring software packages\. They also define validation steps\.

Test components  
Test components are a sequence of steps used to verify that the output image built by your image pipeline functions as expected\.

To create a component:

1. Select **Components** from the navigation pane\. Then select **Create component**\.

1. On the **Create component** page, under **Component details**, enter the following:
   + **Image Operating system \(OS\)**\. Specify the OS with which the component is compatible\. 
   + **Component category**\. From the dropdown, select the type of build or test component you are creating\. 
   + **Component name**\. Enter a name for the component\.
   + **Component version**\. Enter the version number of the component\. 
   + **Description**\. Provide an optional description to help you identify the component\.
   + **Change description**\. Provide an optional description to help you understand the changes made to this version of the component\. 

1. Under **Definition document**, which is the document that defines the actions that Image Builder performs on your image, enter the document content in YAML format in the provided field\. You can optionally use the AWS\-provided example \(auto\-filled when you select **Use example**\) and edit the content inline\. 

1. After you have entered the component details, select **Create component**\.
**Note**  
To see your new component when you create or update a recipe, apply the **Owned by me** filter to the build or test component list\. The filter is located at the top of the component list, next to the search box\.

1. To delete a component, from the **Components** page, select the check box next to the component that you want to delete\. From the **Actions** dropdown, select **Delete component**\. 

You can create a new component version by selecting the check box next to the component and, under the **Actions** dropdown, select **Create new version**\. This will take you to the **Create Component** page, where you can create a new component version\. 