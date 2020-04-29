# WorkSpace Bundles and Images<a name="amazon-workspaces-bundles"></a>

A *WorkSpace bundle* is a combination of an operating system, and storage, compute, and software resources\. When you launch a WorkSpace, you select the bundle that meets your needs\. The default bundles available for WorkSpaces are called *public bundles*\. For more information about the various public bundles available for Amazon WorkSpaces, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.

If you've launched a Windows or Amazon Linux WorkSpace and have customized it, you can create a custom image from that WorkSpace\. 

A *custom image* contains only the OS, software, and settings for the WorkSpace\. A *custom bundle* is a combination of both that custom image and the hardware from which a WorkSpace can be launched\.

After you create a custom image, you can build a custom bundle that combines the custom WorkSpace image and the underlying compute and storage configuration that you select\. You can then specify this custom bundle when you launch new WorkSpaces to ensure that the new WorkSpaces have the same consistent configuration \(hardware and software\)\. 

If you need to perform software updates or to install additional software on your WorkSpaces, you can update your custom bundle and use it to rebuild your WorkSpaces\.

**Topics**
+ [Create a Custom WorkSpaces Image and Bundle](create-custom-bundle.md)
+ [Update a Custom WorkSpaces Bundle](update-custom-bundle.md)
+ [Copy a Custom WorkSpaces Image](copy-custom-image.md)
+ [Delete a Custom WorkSpaces Bundle or Image](delete_bundle.md)
+ [Bring Your Own Windows Desktop Licenses](byol-windows-images.md)