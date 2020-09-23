<div align="center">

## Dynamic Text Image Creation


</div>

### Description

Create text graphics on the fly using GDI+. Supports any TrueType font and allows multiple style options. See comments for usage. Please vote or post questions.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Daniel M\. Hendricks](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/daniel-m-hendricks.md)
**Level**          |Intermediate
**User Rating**    |4.5 (18 globes from 4 users)
**Compatibility**  |C\#, ASP\.NET
**Category**       |[Graphics/ Sound](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics-sound__10-15.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/daniel-m-hendricks-dynamic-text-image-creation__10-2735/archive/master.zip)





### Source Code

```
<%@ Page Language="C#" Debug="true" %>
<%@ Import Namespace="System" %>
<%@ Import Namespace="System.IO" %>
<%@ Import Namespace="System.Text" %>
<%@ Import Namespace="System.Drawing" %>
<%@ Import Namespace="System.Drawing.Imaging" %>
<%@ Import Namespace="System.Drawing.Text" %>
<%@ Import Namespace="System.Drawing.Drawing2D" %>
<%
/* USAGE:
// script.aspx?<parameters...>
//
// VALID PARAMETERS:
// text = The text to display
// color = The Hex color of the text
// bgcolor = The hex background color
// font = Name of truetype font
// size = Size of the font in points
// bold = If not null, bold
// italic = If not null, italicized
// strikeout = If not null, striked
// underline = If not null, underlined
// antialias = If not null, anti-aliased
//
// EXAMPLE USAGE:
// script.aspx?text=Hello&size=16
// script.aspx?text=Hello&bold=true
//
// REQUIREMENTS:
// Either change the 'fontDir' variable
// to the location your TrueType fonts
// are stored in, or copy your fonts to
// the same folder as the script (ie,
// arial.ttf, tahoma.ttf, etc).
*/
// SET VARIABLE VALUES
string fontDir = Server.MapPath("./");
string text = ((Request.QueryString["text"] == null) ? "Default Text" : Request.QueryString["text"].ToString() );
string fgColor = ((Request.QueryString["color"] == null) ? "#000000" : "#"+Request.QueryString["color"].ToString() );
string bgColor = ((Request.QueryString["bgcolor"] == null) ? "#FFFFFF" : "#"+Request.QueryString["bgcolor"].ToString() );
string fontName = ((Request.QueryString["font"] == null) ? "arial" : Request.QueryString["font"].ToString() );
int fontSize = ((Request.QueryString["size"] == null) ? 12 : Convert.ToInt32(Request.QueryString["size"]) );
// LOAD TRUETYPE FONT
PrivateFontCollection privateFontCollection = new PrivateFontCollection();
privateFontCollection.AddFontFile(fontDir + fontName + ".ttf");
FontFamily fontFamily = privateFontCollection.Families[0];
// SET FONT STYLE
FontStyle style = FontStyle.Regular;
if(Request.QueryString["bold"] != null) style = style | FontStyle.Bold;
if(Request.QueryString["italic"] != null) style = style | FontStyle.Italic;
if(Request.QueryString["strikeout"] != null) style = style | FontStyle.Strikeout;
if(Request.QueryString["underline"] != null) style = style | FontStyle.Underline;
Font font = new Font(fontFamily, fontSize, style, GraphicsUnit.Pixel);
// INITIALIZE GRAPHICS
Bitmap img = new Bitmap(1,1);
Graphics g = Graphics.FromImage(img);
// DETERMINE CANVAS WIDTH AND HEIGHT
SizeF stringLength = g.MeasureString(text, font, 300);
int width = Convert.ToInt32(stringLength.Width);
int height = fontSize;
// SET COLORS
SolidBrush fgBrush = new SolidBrush(ColorTranslator.FromHtml(fgColor));
SolidBrush bgBrush = new SolidBrush(ColorTranslator.FromHtml(bgColor));
// SET GRAPHIC OBJECT
RectangleF rectF = new RectangleF(-1, -1, width+1, height+1);
img = new Bitmap(width, height, PixelFormat.Format24bppRgb);
g = Graphics.FromImage(img);
g.SmoothingMode = SmoothingMode.AntiAlias;
if (Request.QueryString["antialias"] != null) g.TextRenderingHint = TextRenderingHint.AntiAlias;
g.FillRectangle(bgBrush, rectF);
// SET ALIGNMENT
StringFormat format = new StringFormat();
format.LineAlignment = StringAlignment.Center;
// DRAW THE FONT
MemoryStream memStream = new MemoryStream();
g.DrawString(text, font, fgBrush, rectF, format);
Response.ContentType = "image/png";
img.Save(memStream, ImageFormat.Png);
memStream.WriteTo(Response.OutputStream);
// CLEAN UP
img.Dispose();
%>
```

