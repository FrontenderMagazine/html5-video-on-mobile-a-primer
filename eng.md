Support for HTML5 video has improved over the last few years, but nonetheless,
video is one of the HTML5 features that has more fractured browser support, and 
the inconsistencies are even more apparent on mobile platforms. Especially 
because on mobile you often lack the ability to fall back on Flash, it’s even 
more critical to ensure HTML5 video works. This is a primer to help you navigate
your way around implementing HTML5 video for mobile platforms.

## The Basics

### Video Encoding

With Firefox adding support for MP4 on mobile platforms this past month, all
major mobile browsers—Chrome, Safari, Firefox, IE, Opera (Mobile not Mini) and 
Blackberry—have converged on a single video standard, which is great news 
because now you only have to encode your video in one format: MP4 with the H.264
codec.

However, Firefox and Opera on desktop don’t support MP4/H.264 yet, so you may
still need to encode in another format, e.g. Ogg format with the Theora codec.

H.264 does have licensing issues though, which you should be aware of; unlike
Ogg and WebM, which are royalty-free and open-source friendly.

There is various software, including desktop apps and command line tools, to
get your videos into the right format. If you have user-uploaded videos, you may
want to look into an encoding service like
 [Zencoder][1], or [Amazon Elastic Encoder][2].

### The Markup

Now that you have the videos, let’s get them to play. Using the HTML5 <video> 
tag is straightforward, it’s like including an image!

|  | <video src="/videos/example.mp4"width="640"height="320"controls></video
> |
||

If you want to add fallback formats, you need to include them as separate <
source
> child elements inside <video>.

|  | <video width="640"height="320"controls>

The browser will play the first video format it supports.

## Considerations

### Responsive Layout

A great benefit of HTML5 video is that you can treat the video element as
another block in your layout. For example, giving the video element 100% width (
and no height) allows the video to automatically scale according to its parent 
container dimensions, which is great for responsive layouts. While it’s easy to 
visually resize the video, you may want to consider serving smaller sized videos
to mobile devices, where bandwidth is slower and costly. You could browser sniff
on the server and serve the corresponding video, or dynamically load the source 
URL for the video in JavaScript on the client.

### Autoplay

Mobile platforms tend to discourage the autoplay attribute, and instead require
a user action, such as clicking on the video, in order to trigger the video to 
start playing. The rationale is that bandwidth can be expensive for mobile users
on data plans, and downloading large video files that the user did not 
explicitly want to see is unwarranted. You could write some creative JavaScript 
to get around this limitation if you really need to.

### Custom Controls

Another key benefit of using HTML5 video is the ability to create your own
controls that hook into the video API and easily style them using CSS. However, 
you may encounter some gotchas on mobile, for example, no ability to trigger 
full-screen mode from custom controls, or having custom controls overridden by 
native controls once in full-screen mode.

## Troubleshooting

Some common reasons why your video doesn’t play:

### Incorrect MIME Type

You need to make sure where you’re serving the videos from attaches the right
MIME type for your video, e.g. video/mp4, video/ogg, etc. An easy way to verify 
the MIME type is via the curl command in your terminal.

|  | curl-Ihttp://mysite.com/videos/example.mp4 |
||

If the returned Content-Type property doesn’t say video, you need to make
changes to your web server configuration.

### Controls don’t display properly

Check your CSS and see if there is an overly broad style that is affecting the
look of the native control buttons.

### HTML5 Page Is Inside a WebView

If you’re embedding HTML5 pages with video inside an iOS or Android app,
things get a bit more complicated.

### iOS UIWebView

To support playing HTML5 video inline, which is the default elsewhere, you need
to set the
 [allowsInlineMediaPlayback][3] property, and include the webkit-playsinline 
attribute on the video tag, otherwise the video will play in the default media 
player.

You also need to set the [mediaPlaybackRequiresUserActio<wbr></wbr>n][3] to NO
in order to support autoplay.

### Android WebView

Android WebView requires some explicit settings in order to support HTML5 video
. You need to enable hardware acceleration (addandroid:hardwareAccelerated="
true
" to your AndroidManifest.xml file) and set [WebChromeClient][4] on your
WebView.

There are certainly a lot of things to consider when implementing HTML5 video
for mobile, but steady improvements in browser support is making it easier for 
you to give your users seamless in-browser video playback.

 [1]: http://zencoder.com/
 [2]: http://aws.amazon.com/elastictranscoder

 [3]: https://developer.apple.com/library/ios/documentation/uikit/reference/UIWebView_Class/Reference/Reference.html

 [4]: http://developer.android.com/reference/android/webkit/WebChromeClient.html