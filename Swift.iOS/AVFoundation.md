## Play video using AVPlayerViewController

> let url1 = Bundle.main.url(forResource: "video1", withExtension:"mp4")!
> let url2 = Bundle.main.url(forResource: "video2", withExtension:"mp4")!

```swift
let player = AVPlayer(url: url1)
let vcPlayer = AVPlayerViewController()
vcPlayer.player = player
self.present(vcPlayer, animated: true) {
     playerViewController.player!.play()
}
```

## Playing multiple video track simultaneously

```swift

let item1 = AVPlayerItem(url: url1)
let item2 = AVPlayerItem(url: url2)
let player = AVQueuePlayer(items: [item1, item2])
let playerViewController = AVPlayerViewController()
playerViewController.player = player
self.present(playerViewController, animated: true) {
    playerViewController.player!.play()
 }
 
 ```
 
 ## Get thumbnail image from video at specific time 
 
 ```swift 
 
    func imageAtPoint(time: Float64 = 1.0) -> UIImage? {
        let asset = AVAsset(url: url1)
        let assetImgGenerate = AVAssetImageGenerator(asset: asset)
        assetImgGenerate.appliesPreferredTrackTransform = true
        //Can set this to improve performance if target size is known before hand
        //assetImgGenerate.maximumSize = CGSize(width,height)
        let time = CMTimeMakeWithSeconds(time, 600)
        do {
            let img = try assetImgGenerate.copyCGImage(at: time, actualTime: nil)
            let thumbnail = UIImage(cgImage: img)
            return thumbnail
        } catch {
          print(error.localizedDescription)
          return nil
        }
    }
 ```

## Get current play head time

```swift
  player.addPeriodicTimeObserver(forInterval: CMTime(value: 1, timescale: 10), queue: DispatchQueue.main) { [weak self] (time) in
     print("Second:", time.seconds)
}
```
