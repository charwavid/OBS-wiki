In order to add a video or image source to the active scene, you must go through a small process to tell OBS exactly what it is you want to add.

    //Create a new element for the scene
    XElement *myElement = OBSGetSceneElement()->CreateElement(TEXT("MyNewElement"));
    //Assign a data element to pass to the image source
    XElement *myElementData = myElement->CreateElement(TEXT("data"));
    //Set the class of our source. In this case I chose TextSource, but you could do BitmapImageSource or even TutorialImage.
    myElement->SetString(TEXT("TextSource"));
    //Change the values that are passed into the image source. In this case I'm telling the TextSource to set its text.
    myElementData->SetString(TEXT("text"), TEXT("My new text element is working!"));

Using the XElement class you have a very powerful mechanism for passing data to and from scene and image elements.

Elements added this way are not in the list of sources but can be moved by the streamer. Adding image sources directly to the scene this way can have some undesired effects, so it's recommended you add them as a sub-element of some other image source.