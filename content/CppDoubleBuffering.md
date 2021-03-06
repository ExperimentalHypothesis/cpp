
 

 

 

 

 

([C++](Cpp.md)) [Double buffering](CppDoubleBuffering.md)
===========================================================

 

Technique to prevent flicker when working with
[graphics](CppGraphics.md) (it has nothing to do with the [data
type](CppDataType.md) [double](CppDouble.md)).

 

The idea of double buffering is to first paint all screen graphics to an
invisible buffer, then drawing this buffer to screen.

 

Drawing to screen directly is called single buffering and causes
flicker. Single buffering and [double buffering](CppDoubleBuffering.md)
can be compared using the program [DemoBuffering](CppDemoBuffering.md).

 

To clean a buffer, often the background is drawn on the buffer first.

 

In C++ Builder, this looks somewhat like the code below (but see the
program [DemoBuffering](CppDemoBuffering.md) for a complete program):

 

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` #include <cassert> #include <vcl.h> #pragma hdrstop   #include "UnitFormDoubleBuffering.h"  #pragma package(smart_init) #pragma resource "*.dfm" TForm1 *Form1;  __fastcall TForm1::TForm1(TComponent* Owner)   : TForm(Owner) {   this->OnResize(0); }   void __fastcall TForm1::Timer1Timer(TObject *Sender) {   //Clean the buffer by drawing the background on the buffer   ImageBuffer->Canvas->Draw(0,0,ImageBackground->Picture->Graphic);     //Draw the sprites on the buffer   const int nSprites = 100;   for (int i=0; i!=nSprites; ++i)   {     const int x = std::rand() % (ClientWidth - ImageSprite->Width );     const int y = std::rand() % (ClientHeight - ImageSprite->Height);     ImageBuffer->Canvas->Draw(x,y,ImageSprite->Picture->Graphic);   }    //Finally, copy the buffer to the Form   this->Canvas->Draw(0,0,ImageBuffer->Picture->Graphic); }   void __fastcall TForm1::FormResize(TObject *Sender) {   //Assume all Images are invisible   assert(!ImageBuffer->Visible);   assert(!ImageBackground->Visible);   assert(!ImageSprite->Visible);   //Set the Buffer and Background to the right size   ImageBuffer->Picture->Bitmap->Width = ClientWidth;   ImageBuffer->Picture->Bitmap->Height = ClientHeight;   ImageBackground->Picture->Bitmap->Width = ClientWidth;   ImageBackground->Picture->Bitmap->Height = ClientHeight; }`
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

 

