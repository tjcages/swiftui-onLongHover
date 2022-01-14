# swiftui-onLongHover
An extension to add long hover support for SwiftUI

Use like:
```swift
ListItem() 
    .onLongHover(duration: 1.2) { hovering in
        if hovering {
            isPopover = true
        } else {
            isPopover = false
        }
    }
```
Copy this snippet and save the Extension in your Utility file
```swift
extension View {
    func onLongHover(duration: Double, perform action: @escaping (Bool) -> Void) -> some View {
        var timer: Timer?
        var interval: Double = 0.0
        var isHovering = false
        
        func checkHoverState() {
            if isHovering {
                action(true)
                timer?.invalidate()
            }
        }
        
        return self
            .onHover { hovering in
                isHovering = hovering
                if hovering {
                    let startTime = Date()
                    timer = Timer.scheduledTimer(withTimeInterval: 0.1, repeats: true) { _ in
                        interval = Date().timeIntervalSince(startTime)
                        if interval >= duration {
                            checkHoverState()
                        } else {
                            action(false)
                        }
                    }
                } else {
                    action(hovering)
                    timer?.invalidate()
                }
            }
    }
}
```

This works for my needs – might not work for yours ☹️

Can't believe this isn't natively supported in SwiftUI

![usage image](https://github.com/tjcages/swiftui-onLongHover/blob/main/usage.png)
