# SurfaceView

`SurfaceView` can be updated on the background thread.

- `SurfaceView` has dedicate surface buffer while all the view share one surface buffer that is allocated by ViewRoot. In another word, `SurfaceView` cost more resources.

- `SurfaceView` cannot be hardware accelerated (as of JB4.2) while 95% operations on normal View are HW accelerated using openGL ES.

- More work should be done to create your customized `SurfaceView`. You need to listener to the surfaceCreated/Destroy Event, create a render thread, more importantly, synchronized the render thread and main thread. 

- The timing to update is different. Normal view update mechanism is constraint or controlled by the framework: You call `view.invalidate()` in the UI thread or `view.postInvalidate()` in other thread to indicate to the framework that the view should be updated. However, the view won't be updated immediately but wait until next VSYNC event arrived. The easy approach to understand VYSNC is to consider it is as a timer that fire up every 16ms for a 60fps screen. In Android, all the normal view update, is synchronized with VSYNC to achieve better smoothness.

## Links
https://developer.android.com/reference/android/view/SurfaceView
