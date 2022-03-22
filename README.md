# Communication with physical space and our phones


As we start building our final projects, a popular immersive technique is combining the 'real world' with some digital content on a guest's smartphone. 

We can elegantly prompt a phone interaction with different 'real world' triggers. We'll be quickly overviewing how to do this 
(sort of) with: QR codes, Image markers, NFC (near field communication) tags.

We will start with using p5 and the editor, but eventually extract the concepts from p5 and code on our local computers. 

The most challenging aspect of this will be hosting your code for guests to access on their phones. It need to be accessible for them over a server, and for many things over the web, that server needs to be secure (https instead of http). For this tutorial, I'm making use of Github pages. 

## p5 editor

Launch the [p5 Editor](https://editor.p5js.org/).

Basic p5 code:

```
let x = 50;
let y = 100;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
  circle(x, y, 20);
}
```

Now let's [look at an example](https://editor.p5js.org/tigoe/sketches/-BEzcjfMF) of using a QR code with the p5 editor, made by Tom Igoe. The JavaScript documentation is [here](https://github.com/kazuhikoarase/qrcode-generator/tree/master/js).

```
/*
  QR Code generator

  Draws a QR code using a text string.  Uses 
  https://github.com/kazuhikoarase/qrcode-generator
  as the QR Code generator library. It's hosted at this CDN:
  https://unpkg.com/qrcode-generator@1.4.4/qrcode.js"

  created 19 July 2020
  by Tom Igoe
*/

// a string to diisplay in the QR code
// (the URL of this sketch):
let inputString = parent.location.href;
// an HTML div to display it in:
let tagDiv;

function setup() {
  createCanvas(windowWidth, windowHeight);
  // make the HTML tag div:
  tagDiv = createDiv();
  // position it:
  tagDiv.position(30, 30);
  console.log(inputString);
}

function draw() {
  background(255);
  fill(0);
  text(inputString, 10, 10);

  // make the QR code:
  let qr = qrcode(0, 'L');
  qr.addData(inputString);
  qr.make();
  // create an image from it:
  // paaramtetrs are cell size, margin size, and alt tag
  // cell size default: 2
  // margin zize defaault: 4 * cell size
  let qrImg = qr.createImgTag(5, 20, "qr code");
  // put the image into the HTML div:
  tagDiv.html(qrImg);
}
```


Now you an easily just print off the created QR code, and use it in your physical project. 

You don't really even need p5 or code to generate a QR code. Free sites like [QR Code Monkey](https://www.qrcode-monkey.com/) will do it for you. 

We likely won't just load an external website though, we'll want to direct guests to some content we've created. Creating your own website would be better for this, but an easy way to do it that doesn't delve into server side programming is using p5. 

Let's put [this](https://vimeo.com/253989945) video in a standard p5 sketch. The iFrame embed we'll use is this: 

```
<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/253989945?h=c6db007fe5&color=ef0800&title=0&byline=0&portrait=0" style="position:absolute;top:0;left:0;width:100%;height:100%;" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>
```

YouTube works as well, the positioning just needs to be finetuned a bit more to make it responsive. 

Let's make a new p5 sketch for this. 

If you've never noticed, in the p5 sketch, there's an arrow beside 'sketch.js' text where we can access the html and css. 

![p5 Layout](./assets/p5_sketch_layout.png)

So we just drop the above iFrame into the html. Also we can minimize (or even delete) the JavaScript sketch. 

We save this sketch, note the URL, and now use that url in the QR code sketch for the variable 'inputString'. 

So this is for playing a video that's hosted elsewhere, which would be the recommended method for videos because they're large (and p5 probably has a file size max). You can easily do the same thing with images that are already hosted online. If they are not, you can upload files from the same tab as when you find the html and css files. 


- Click on the arrow beside 'Sketch Files'
- And click upload file. 
- From there, we can draw the image on to our sketch.


```
let puppy;

function preload() {
  puppy = loadImage('puppy.jpg');
}

function setup() {
  createCanvas(400, 400);
  
}

function draw() {
  background(220);
  image(puppy, 0, 0);
  noLoop();
}
```

Now you have many options! If you need to create a multi-page website, that could get difficult with p5 sketches. Github pages would be easier for that, but, you sort of need to know how to use Github. A wordpress or squarespace would also work, as you only need to have a unique URL where the content is, and note that in thr QR code. 


## AR.js

Augmented reality is getting more prevalent and easier to use. (AR.js)[https://ar-js-org.github.io/AR.js-Docs/] is a web library to use AR right in your mobile browser. In theory, it's really great! But in practice, it's going to be really difficult to make work on any guest's phone. 

The library is good for markers (like QR codes or alternatives) and image tracking. 

Though it would be nice to use p5 with this, I'm not sure it's possible currently due to integration and security issues. For now, we're going to be using plain old HTML and JS. 

We're going to need to access the Github pages I've created on our phones, and learning from this tutorial, I've generated a QR code for this URL for your phones. How meta!

![QR code for this page](./assets/qr-code.png)

### Marker

I have adapted the example for using a marker and it's viewable [here](https://saxani.github.io/phone-trigger-tutorial/marker.html)

The code is [here](https://github.com/saxani/phone-trigger-tutorial/blob/main/marker.html).

Assuming you can load the page on your phone, just scan this Hiro image and it should overlay a 3D model. 

![Hiro image](./assets/hiro.png)

The advantage of this library is we can use AR over semi-boring real life triggers, rather than just link to a URL. 

Instead of a 3D model, we can add shapes, but replacing the 'entity' with this code:

```
<a-entity geometry="primitive: sphere; radius: 1.5"
  light="type: ambient; color: white;"
  material="color: white;"
  position="0 0 0"
  scale="0.25 0.25 0.25"
  >
</a-entity>
```

### Image Tracking

The most fun option (but most difficult) is using custom images as markers. 

[Here](https://saxani.github.io/phone-trigger-tutorial/image-tracking.html) is a customized example of the one on the documentation page that should use this image as a marker:

![pinball](./assets/pinball.jpg)

The code is [here](https://github.com/saxani/phone-trigger-tutorial/blob/main/image-tracking.html).

Big problem: It is super memory intensive, and as I tried to customize this, I kept getting memory errors. 


### Overly

As I was searching for easier ways to integrate image tracking, I found [this app](https://overlyapp.com/). It's really simple to use, and no code. Problem: In the free version, you can only overlay a video over an image. It's still a pretty seamless interaction, except you'd need to have guests use the Overly app. 


## NFC

NFC (Near Field Communiction) is built on RFID technology. It's the technology we use when we pay for something with our phone, bank cards, etc. 

There is new experimental Google Chrome technology to let your phone read NFC tags. The stipulations:
- Newer Android phone
- Only through Chrome

Unfortunately, I don't have working devices to test it. But if someone does, they can see if it works here:
https://googlechrome.github.io/samples/web-nfc/

Usually our phones work the other way, utilizing a 'tag' in the phone itself, and using readers. This approach wouldn't work for our experiences though, without some heavy engineering. The order would have to go something like:
- Guest launches website for your project
- Scans RFID scanner, connected to Arduino, connected to the web
- RFID scanner communicates to a server
- Server communicates to the website that the guest is on, and triggers something to happen

This would take a long time to demo, so this will be a specialty case. 
