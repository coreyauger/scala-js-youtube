# scala-js-youtube
YouTube embedded player types for scalajs

# Getting Started
The code file is small enought that you should be fine to copy and past the source into your project.

# Usage
Note that this file asyncronosly loads the youtube javascript file, so there is no need to include it.  

Firt off you will create a target element where the player will get inserted.

```html
<div id="player"></div>
```

To load the JS file from your Scala project you will have some code like the following:
```scala
// 2. This code loads the IFrame Player API code asynchronously.
var tag = org.scalajs.dom.document.createElement("script").asInstanceOf[org.scalajs.dom.html.Script]
tag.src = "https://www.youtube.com/iframe_api"
var firstScriptTag = org.scalajs.dom.document.getElementsByTagName("script").item(0)
firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)
```

The youtube player will call a `window` level function named `onYouTubeIframeAPIReady` when it is done loading and ready to use.

Here is an example of how to handle this call.
```scala
// 3. This function creates an <iframe> (and YouTube player) after the API code downloads.
(org.scalajs.dom.window.asInstanceOf[js.Dynamic]).onYouTubeIframeAPIReady = () => {
  val player = new Player("player", PlayerOptions(
    width = "100%",
    height = "100%",
    videoId = "M7lc1UVf-VE",
    events = PlayerEvents(
      onReady = onPlayerReady _,
      onError = onPlayerError _,
      onStateChange = onPlayerStateChange _
    ),
    playerVars = PlayerVars(
      playsinline = 1
    )
  ))
}
```

## That's It !
You know have a typesafe scala friendly youtube player with the full API availavle to you here.
[https://developers.google.com/youtube/iframe_api_reference](https://developers.google.com/youtube/iframe_api_reference)

```scala
class Player protected() extends js.Object {
  def this(divId:String, settings:PlayerOptions) = this()

  //https://developers.google.com/youtube/iframe_api_reference?hl=en#Playback_controls
  // queueing
  def loadVideoById(videoId:String, startSeconds:Double, suggestedQuality:String):Unit =  js.native
  def loadVideoById(opts:VideoIdStartOptions):Unit =  js.native
  def cueVideoById(videoId:String,startSeconds:Double,suggestedQuality:String):Unit =  js.native
  def cueVideoById(opts:VideoIdStartOptions):Unit = js.native
  // TODO: obj syntax
  def cueVideoByUrl(mediaContentUrl:String,startSeconds:Double,suggestedQuality:String):Unit = js.native
  def loadVideoByUrl(mediaContentUrl:String,startSeconds:Double,suggestedQuality:String):Unit = js.native

  // controls
  def playVideo():Unit = js.native
  def pauseVideo():Unit = js.native
  def stopVideo():Unit = js.native
  def seekTo(seconds:Double, allowSeekAhead:Boolean):Unit = js.native
  def clearVideo():Unit = js.native
  def nextVideo():Unit = js.native
  def previousVideo():Unit = js.native
  def playVideoAt(index:Double):Unit = js.native
  def mute():Unit = js.native
  def unMute():Unit = js.native
  def isMuted():Boolean = js.native
  def setVolume(vol:Double):Unit = js.native
  def getVolume():Double = js.native
  def setSize(w:Double, h:Double):Unit = js.native
  def getPlaybackRate():Double = js.native
  def setPlaybackRate(rate:Double):Unit = js.native
  def getAvailablePlaybackRates():js.Array[Double] = js.native
  def setLoop(loop:Boolean):Unit = js.native
  def setShuffle(shufflePlaylist:Boolean):Unit = js.native
  def getVideoLoadedFraction():Double = js.native

  // -1 – unstarted
  // 0 – ended
  // 1 – playing
  // 2 – paused
  // 3 – buffering
  // 5 – video cued
  def getPlayerState():Double = js.native

  def getCurrentTime():Double = js.native

  // values are highres, hd1080, hd720, large, medium and small. It will also return undefined
  def getPlaybackQuality():String = js.native
  def setPlaybackQuality(quality:String):Unit = js.native
  def getAvailableQualityLevels():js.Array[String] = js.native
  def getDuration():Double = js.native
  def getVideoUrl():String = js.native
  def getVideoEmbedCode():String = js.native
  def getPlaylist():js.Array[String] = js.native
  def getPlaylistIndex():Double = js.native
  def addEventListener(event:String, functionName:String):Unit = js.native
  def removeEventListener(event:String, functionName:String):Unit = js.native

  // dom
  def getIframe():js.Object = js.native
  def destroy():Unit = js.native

}
```
