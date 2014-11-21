Now that you have a library that OBS is recognizing as a plugin, let's make OBS do something with it. The OBS API provides a few abstract classes that plugin developers can use to extend the functionality of OBS. In this tutorial, we'll look at the `ImageSource` class that is provided.

The `ImageSource` class only has two pure virtual functions that must be implemented. There are many more virtual functions that can be overridden, but for the purpose of this tutorial, we will cover the bare minimum.

First, make a new class definition that extends `ImageSource`.

    class TutorialImage : public ImageSource{
        VertexBuffer *box;
        Texture *tex;
    public:
        TutorialImage(XElement *data);
        void Render(const Vect2 &pos, const Vect2 &size);
        Vect2 GetSize() const;
    };

`VertexBuffer` is a class that can hold a list of vertices that can be used to draw to the stream. `Texture` is a 2D image that can be drawn on top of geometry. Now, define the functions as follows:

    TutorialImage::TutorialImage(XElement *data){
        VBData *points = new VBData;
        points->VertList.SetSize(4);              //Reserve space for the vertices
        box = CreateVertexBuffer(points, FALSE);  //Store the vertex list in a vertex buffer
        UINT color = 0x801142FF;                  //Color is 0xAABBGGRR on little-endian machines
        tex = CreateTexture(1, 1, GS_RGBA, &color, FALSE, TRUE);  //Create a 1x1 translucent red texture
    }
    void TutorialImage::Render(const Vect2 &pos, const Vect2 &size){
        VBData *data = box->GetData();               //Fill our vertex buffer object with a square that
        data->VertList[0].Set(pos.x, pos.y, 0);      //occupies our entire drawing region
        data->VertList[1].Set(pos.x, pos.y + size.y, 0);
        data->VertList[2].Set(pos.x + size.x, pos.y, 0);
        data->VertList[3].Set(pos.x + size.x, pos.y + size.y, 0);
        box->FlushBuffers();         //Flush the software representation of the vertices to the GPU
        LoadVertexBuffer(box);     //Prepare the vertex buffer we just created for drawing
        LoadTexture(tex);          //Load our 1x1 texture for drawing
        Draw(GS_TRIANGLESTRIP);    //Draw our vertices as a triangle strip.
    }
    Vect2 TutorialImage::GetSize() const{  //Return the size of our image. This can be
        Vect2 ret;                         //anything you want. This just determines the
        ret.x = 100;                       //standard size of your plugin image.
        ret.y = 100;                       //You can also change this as much as you want
        return ret;                        //during runtime.
    }

And now we need to tell OBS that we have this new image source that it should take into consideration. First, make a function that will create our video source and return a pointer to an `ImageSource`

    ImageSource* STDCALL CreateTutorialImage(XElement *data){
        return new TutorialImage(data);
    }

And now in our `LoadPlugin` function, add the line

    OBSRegisterImageSourceClass(TEXT("VideoSource"), TEXT("Tutorial"), (OBSCREATEPROC)CreateTutorialImage, (OBSCONFIGPROC)NULL);

If you did everything right, you should now have a program that looks like this.

TutorialPlugin.h:

    #pragma once
    #include "OBSApi.h"
    extern "C" __declspec(dllexport) bool LoadPlugin();
    extern "C" __declspec(dllexport) void UnloadPlugin();
    extern "C" __declspec(dllexport) CTSTR GetPluginName();
    extern "C" __declspec(dllexport) CTSTR GetPluginDescription();
    
    class TutorialImage : public ImageSource{
        VertexBuffer *box;
        Texture *tex;
    public:
        TutorialImage(XElement *data);
        void Render(const Vect2 &pos, const Vect2 &size);
        Vect2 GetSize() const;
    };

TutorialPlugin.cpp:

    #include "TutorialPlugin.h"
    
    TutorialImage::TutorialImage(XElement *data){
        VBData *points = new VBData;
        points->VertList.SetSize(4);              //Reserve space for the vertices
        box = CreateVertexBuffer(points, FALSE);  //Store the vertex list in a vertex buffer
        UINT color = 0x801142FF;                  //Color is 0xAABBGGRR on little-endian machines
        tex = CreateTexture(1, 1, GS_RGBA, &color, FALSE, TRUE);  //Create a 1x1 translucent red texture
    }
    void TutorialImage::Render(const Vect2 &pos, const Vect2 &size){
        VBData *data = box->GetData();               //Fill our vertex buffer object with a square that
        data->VertList[0].Set(pos.x, pos.y, 0);      //occupies our entire drawing region
        data->VertList[1].Set(pos.x, pos.y + size.y, 0);
        data->VertList[2].Set(pos.x + size.x, pos.y, 0);
        data->VertList[3].Set(pos.x + size.x, pos.y + size.y, 0);
        box->FlushBuffers();         //Flush the software representation of the vertices to the GPU
        LoadVertexBuffer(box);     //Prepare the vertex buffer we just created for drawing
        LoadTexture(tex);          //Load our 1x1 texture for drawing
        Draw(GS_TRIANGLESTRIP);    //Draw our vertices as a triangle strip.
    }
    Vect2 TutorialImage::GetSize() const{  //Return the size of our image. This can be
        Vect2 ret;                         //anything you want. This just determines the
        ret.x = 100;                       //standard size of your plugin image.
        ret.y = 100;                       //You can also change this as much as you want
        return ret;                        //during runtime.
    }
    
    ImageSource* STDCALL CreateTutorialImage(XElement *data){
        return new TutorialImage(data);
    }
    
    bool LoadPlugin()
    {
        OBSRegisterImageSourceClass(TEXT("VideoSource"), TEXT("Tutorial"), (OBSCREATEPROC)CreateTutorialImage, (OBSCONFIGPROC)NULL);
        return true;
    }
    
    void UnloadPlugin()
    {
    }
    
    CTSTR GetPluginName()
    {
        return TEXT("Tutorial plugin");
    }
    
    CTSTR GetPluginDescription()
    {
        return TEXT("This plugin is designed to help teach people how to write plugins for OBS.");
    }

Now you should have a new video source called `Tutorial`. Try adding it to a scene. 

[[Previous Tutorial: Building Your First Plugin|Plugin-Tutorial-\-Building-Your-First-First-Plugin]]