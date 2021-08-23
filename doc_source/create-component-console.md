# Create a component using the Image Builder console<a name="create-component-console"></a>

To create an AWSTOE application component using the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Select **Components** from the navigation pane\. Then select **Create component**\.

1. On the **Create component** page, under **Component details**, enter the following:

   1. **Image Operating system \(OS\)**\. Specify the operating system that the component is compatible with\.

   1. **Component category**\. From the dropdown, select the type of build or test component that you are creating\.

   1. **Component name**\. Enter a name for the component\.

   1. **Component version**\. Enter the version number of the component\.

   1. **Description**\. Provide an optional description to help you identify the component\.

   1. **Change description**\. Provide an optional description to help you understand the changes made to this version of the component\.

1. Under **Definition document**, which is the document that defines the actions that Image Builder performs on the build and test instances to create your image, enter the document content in YAML format in the provided box\. You can optionally use the example provided by AWS \(auto\-filled when you select **Use example**\) and edit the content inline\. For more information about the phases, steps, and syntax for AWSTOE YAML application component documents, see [Use documents in AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-use-documents.html)\.

1. After you have entered the component details, select **Create component**\.
**Note**  
To see your new component when you create or update a recipe, apply the **Owned by me** filter to the build or test component list\. The filter is located at the top of the component list, next to the search box\.

1. To delete a component, from the **Components** page, select the check box next to the component that you want to delete\. From the **Actions** dropdown, select **Delete component**\.

To create a new component version, follow these steps:

1. Depending on where you start:
   + From the **Components** list page – Select the check box next to the component name, then select **Create new version** from the **Actions** menu\.
   + From the component detail page – Choose the **Create new version** button in the upper right corner of the heading\.

1. The component information is already populated with the current values when the **Create Component** page displays\. Follow the create a component steps to update the component\. This ensures that you enter a unique semantic version in the **Component version**\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.